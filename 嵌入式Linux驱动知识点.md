# 嵌入式 Linux 内核驱动知识点 —— 源码级

> **对应简历**：专业技能 6 —— "了解 Linux 内核驱动开发，掌握设备树、platform 总线、字符设备以及 Input、SPI、I2C、PWM 子系统，能够完成简单外设驱动的编写与调试。"
>
> ⚠️ **先把边界说清楚（很重要）**：
> 简历用的词是"**了解**"，后面接的是"能完成**简单**外设驱动的编写与调试"。
> 这是一个**明确的定位** —— 不是"做过复杂驱动"。
> 面试时的正确姿态是：**框架能讲清、代码能写出骨架、能说清各子系统的分层关系**；
> 被问到没做过的地方（比如具体某个 SoC 的时钟树配置、内核源码的某个函数实现细节）**如实说边界**。
>
> ⚠️ **和项目的关联也要说准**：项目二的 misc 字符设备**驱动本体是团队提供的**，
> 你做的是**应用层基于 read 接口的接收逻辑**。这条千万别答成"我写过驱动"。
> 但反过来说 —— **你天天在用的就是这个驱动**，所以它**怎么注册、怎么建节点、read 走哪条路径**你完全可以说清楚，这是加分项。

---

## 目录

- [〇、先建地图：内核驱动的整体分层](#〇先建地图内核驱动的整体分层)
- [一、内核模块基础](#一内核模块基础)
- [二、Linux 设备驱动模型（bus / device / driver）](#二linux-设备驱动模型bus--device--driver)
- [三、设备树](#三设备树)
- [四、platform 总线](#四platform-总线)
- [五、字符设备](#五字符设备)
- [六、并发与竞态（内核视角）](#六并发与竞态内核视角)
- [七、中断处理](#七中断处理)
- [八、用户态接口](#八用户态接口)
- [九、Input 子系统](#九input-子系统)
- [十、SPI 子系统](#十spi-子系统)
- [十一、I2C 子系统](#十一i2c-子系统)
- [十二、PWM 子系统](#十二pwm-子系统)
- [十三、配套机制（GPIO / pinctrl / regmap / DMA）](#十三配套机制gpio--pinctrl--regmap--dma)
- [十四、内核启动与 probe 时机](#十四内核启动与-probe-时机)
- [十五、驱动调试手段](#十五驱动调试手段)
- [十六、交叉编译内核模块](#十六交叉编译内核模块)
- [十七、面试高频问答](#十七面试高频问答)

---

## 〇、先建地图：内核驱动的整体分层

**整体**：内核驱动不是"一堆孤立的文件"，它是一套**分层 + 总线化**的架构。**看懂这张图，后面每一节都能找到位置。**

```
┌───────────────────────────────────────────────────────────────┐
│  用户态                                                        │
│   应用程序 ／ Qt ／ 你的上位机                                   │
│        │  open/read/write/ioctl/poll/mmap                     │
├────────┼──────────────────────────────────────────────────────┤
│  VFS   │  （虚拟文件系统：把"文件操作"翻译成"驱动操作"）           │
├────────┼──────────────────────────────────────────────────────┤
│  子系统层（统一设备类型，对上提供统一接口）                       │
│   ┌──────────┬──────────┬──────────┬──────────┬────────────┐  │
│   │ 字符设备  │ Input    │ SPI      │ I2C      │ PWM        │  │
│   │ (cdev)   │ 子系统    │ 子系统    │ 子系统    │ 子系统      │  │
│   └──────────┴──────────┴──────────┴──────────┴────────────┘  │
├───────────────────────────────────────────────────────────────┤
│  驱动核心层（设备模型）                                          │
│    bus（总线） ── device（设备） ── driver（驱动）               │
│    kobject / sysfs / 引用计数 / 电源管理                         │
├───────────────────────────────────────────────────────────────┤
│  硬件层                                                        │
│    SoC 内建外设（UART/SPI/I2C/PWM 控制器）／外挂芯片／GPIO       │
└───────────────────────────────────────────────────────────────┘
```

**要抓住的三条主线**（面试可以按这个顺序讲）：

| # | 主线 | 一句话 |
|---|---|---|
| 1 | **设备模型** | 内核用 **bus/device/driver** 三个对象把"硬件"和"驱动代码"解耦 —— 匹配上了才调 `probe` |
| 2 | **设备树** | ARM 上**硬件信息不再写死在代码里**，而是由 bootloader 传一棵树给内核，驱动去这棵树里"领"自己的资源 |
| 3 | **子系统** | 同一类设备（输入设备、SPI 从设备、PWM）走**同一套框架和用户态接口** —— 驱动作者只填"差异部分" |

---

## 一、内核模块基础

### 1.1 第一个内核模块（源码级）

**整体**：内核模块是**可以运行时动态加载进内核的代码**（`.ko` 文件），它和内核在同一个地址空间、享受内核的全部权限。

```c
/* hello.c */
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>

static int __init hello_init(void)
{
    pr_info("hello: module loaded\n");
    return 0;                       /* 非 0 表示加载失败，内核会回滚 */
}

static void __exit hello_exit(void)
{
    pr_info("hello: module unloaded\n");
}

module_init(hello_init);            /* 加载时调用的入口 */
module_exit(hello_exit);            /* 卸载时调用的出口 */
MODULE_LICENSE("GPL");              /* ★ 必须，否则内核会标记为污染 */
MODULE_AUTHOR("...");
MODULE_DESCRIPTION("...");
```

**细节·🔴 `module_init` 到底是什么**：

它**不是函数**，是个宏，本质是**把函数指针放进一个特定的 ELF 段**：

```c
/* 简化后的定义（include/linux/module.h） */
#define module_init(initfn)                                     \
    static inline initcall_t __maybe_unused __inittest(void)     \
    { return initfn; }                                          \
    int init_module(void) __attribute__((alias(#initfn)));
/*                  ↑ 把 initfn 起个别名叫 init_module —— 内核就找这个符号 */
```

**所以"模块的入口"是靠符号名 `init_module` / `cleanup_module` 约定的**，不是内核"调用了你的函数"。

**细节·🔴 加载/卸载的完整流程**：

```
insmod hello.ko
   │
   ▼
用户态把 .ko 文件内容传给内核（init_module 系统调用）
   │
   ▼
内核：load_module()
   ├─ 检查模块签名 / 版本魔数（不匹配直接拒绝）
   ├─ 分配内存、把 .ko 的各个段搬进来
   ├─ ★ 解析并重定位符号（把模块引用的 printk 等指向内核的实际地址）
   ├─ 检查依赖的模块是否已加载
   ├─ 调模块的 init_module()
   │      ├─ 返回 0 → 加载成功
   │      └─ 返回非 0 → ★ 自动逆序执行已完成的注册（回滚）
   └─ 把模块挂到内核模块链表（lsmod 就能看到）
```

**细节·🔴 为什么 `MODULE_LICENSE("GPL")` 必须写**：
不写（或写非 GPL）的话，内核会**把模块标记为 "tainted"（污染）**，而且**很多只导出给 GPL 模块的内核符号你用不了**（`EXPORT_SYMBOL_GPL` 那些）。**加载时 `dmesg` 会打印警告**。

**追问·"为什么模块里有 `__init` 和 `__exit` 这些标记"**：
`__init` 把函数放进 `.init.text` 段 —— **模块加载完成后这块内存会被释放**（因为初始化函数不会再执行第二次）。`__exit` 同理。这是**省内存**的优化。注意：**如果驱动是编译进内核（`built-in`）而不是模块，`__exit` 函数根本不会被链接进去**（因为内核自己不会卸载）—— 这也是为什么有时候 `module_exit` 的函数里写的内容在内建时"不生效"。

---

### 1.2 内核模块 vs 用户态程序（**必考对比**）

**整体**：模块跑在内核态，**能用的东西少得多，但能做的事多得多**。这个取舍是安全性的代价。

| 维度 | 用户态程序 | 内核模块 |
|---|---|---|
| **地址空间** | 自己的虚拟地址空间，受 MMU 保护 | **和内核共享地址空间**，能访问任何物理内存 |
| **可用库** | 完整的 libc、STL、Qt… | ❌ **只有内核 API**（没有 libc） |
| **栈大小** | 默认 8MB | 🔴 **很小**（x86 是 8KB 或 16KB，ARM 常见 8KB）—— **别在栈上放大的数组** |
| **浮点** | 随便用 | 🔴 **默认不能用**（要用得显式保存/恢复 FPU 状态，且影响性能） |
| **错误后果** | 自己崩溃 | 🔴 **整机 oops / 内核 panic** |
| **内存分配** | `malloc`（可睡眠、可失败） | `kmalloc`/`vmalloc`；**`GFP_KERNEL` 可睡眠、`GFP_ATOMIC` 不可** |
| **printf** | `printf` | **`printk` / `pr_info` 等** |
| **并发** | 要考虑多线程 | 要考虑**中断、软中断、多核、抢占**（比用户态复杂得多） |

**细节·🔴 三件"内核态不能做"的事**（这些是硬约束，答错很扣分）：

| 不能做 | 为什么 | 替代 |
|---|---|---|
| **在任何地方睡眠** | 中断上下文、持有自旋锁时**绝对不能睡眠** | 中断里用 `GFP_ATOMIC` 分配、用工作队列把活推给进程上下文 |
| **用浮点** | FPU 状态不属于内核上下文，切换会破坏用户态现场 | 用**定点数**运算 |
| **无限循环阻塞** | 会卡住整个内核（不只是"卡住自己"） | 用等待队列 + 超时 |

**追问·"为什么内核栈这么小"**：
因为**每个进程都有一份内核栈**（一次系统调用/中断就用到），内核栈大了内存开销是"进程数 × 栈大小"。所以内核代码的写法是"**避免深调用链、避免大局部变量**"—— 这就是为什么内核里大量用 `kmalloc` 而不是栈数组。

---

### 1.3 模块参数与符号导出

**整体**：模块参数让**加载时可以传参**（不用重新编译）；符号导出让**模块之间能互相调用**。

```c
/* ① 模块参数 */
static int baud = 9600;
static char *name = "misc_dev";
module_param(baud, int, 0644);              /* /sys/module/<mod>/parameters/baud */
module_param(name, charp, 0644);
MODULE_PARM_DESC(baud, "baud rate");

/* 加载时传参 */
/* insmod mydev.ko baud=115200 */
/* 或者运行时改（看权限）：echo 115200 > /sys/module/mydev/parameters/baud */

/* ② 符号导出 —— 让别的模块能调用本模块的函数 */
EXPORT_SYMBOL(my_global_func);             /* 所有模块可用 */
EXPORT_SYMBOL_GPL(my_gpl_func);            /* ★ 只有 GPL 模块可用 */
```

**细节·🔴 `EXPORT_SYMBOL` 和 `EXPORT_SYMBOL_GPL` 的区别**：
后者**只对声明了 GPL 许可的模块开放**。这是内核社区对"专有驱动"的一种约束 —— 很多核心 API（比如 `struct file_operations` 相关的某些符号）是 GPL-only 的。

**细节·符号依赖怎么处理**：
如果 A 模块用了 B 模块的符号，加载顺序必须是 **B 先 A 后**。`insmod` 会检查（报 `Unknown symbol`），`modprobe` 会**自动按依赖顺序加载**（它读 `modules.dep`，那是 `depmod` 生成的）。

**追问·"模块版本不匹配怎么解决"**：
内核会检查模块的**版本魔数**（`vermagic`，含内核版本、SMP/抢占配置等）。不匹配就报 `version magic '...' should be '...'`。解法是**用目标内核的头文件重新编译**。`modprobe --force` 能绕过，但**极其危险**（结构体布局变了会导致内存错乱）。

---

## 二、Linux 设备驱动模型（bus / device / driver）⭐

**整体**：这是**整个驱动架构的地基**。核心思想是：**"硬件是什么"和"怎么操作它"分成两个对象，由"总线"负责把它们匹配起来。**

```
   ┌─────────────┐         ┌─────────────┐
   │  device     │◄────────┤  bus        ├────────►│  driver     │
   │（硬件描述）  │  匹配    │（匹配规则）  │          │（操作代码）  │
   │ 由设备树/BSP│         │ of_match /  │          │ 你写的      │
   │ 注册        │         │ id_table    │          │             │
   └─────────────┘         └─────────────┘          └─────────────┘
                                  │
                       匹配成功 ──► 调 driver->probe(device)
```

**细节·三个对象的职责**：

| 对象 | 内核结构 | 谁提供 | 内容 |
|---|---|---|---|
| **bus** | `struct bus_type` | **内核**（预定义） | 名字、match 函数、probe 包装 |
| **device** | `struct device` | **设备树/BSP** | 硬件资源（寄存器地址、中断号、时钟） |
| **driver** | `struct device_driver` | **驱动作者（你）** | `probe`/`remove` + 匹配表 |

**细节·🔴 匹配的优先级顺序**（`drivers/base/platform.c` 里的 `platform_match`）：

```c
static int platform_match(struct device *dev, struct device_driver *drv)
{
    struct platform_device *pdev = to_platform_device(dev);
    struct platform_driver *pdrv = to_platform_driver(drv);

    /* ① 优先用设备树匹配 —— of_match_table 里的 compatible */
    if (of_driver_match_device(dev, drv))
        return 1;
    /* ② 再用 ACPI 匹配（x86 服务器用） */
    if (acpi_driver_match_device(dev, drv))
        return 1;
    /* ③ 再用 id_table（旧的静态设备表） */
    if (pdrv->id_table)
        return platform_match_id(pdrv->id_table, pdev) != NULL;
    /* ④ 最后退化到"名字相同就算匹配" */
    return (strcmp(pdev->name, drv->name) == 0);
}
```

**所以顺序是：设备树 compatible → ACPI → id_table → 名字**。现代 ARM 平台上**基本都走第 ① 条**。

**细节·🔴 probe 在什么时候被调用**（**这题很能区分"背过"和"懂"**）：

```
       情况 A：驱动先注册，设备后注册
   driver_register() ──► bus_add_driver()
                            │
                            └─► 遍历总线上【已有】的设备 → 匹配 → 匹配上就 probe

       情况 B：设备先注册，驱动后注册（最常见）
   device_register() / 设备树展开 ──► bus_add_device()
                            │
                            └─► 遍历总线上【已有】的驱动 → 匹配 → 匹配上就 probe
```

**两种顺序都会触发 probe**，这就是"匹配是双向的"。

**细节·`probe` 里做的事（标准骨架）**：

```c
static int my_probe(struct platform_device *pdev)
{
    struct resource *res;
    void __iomem *base;
    int irq, ret;

    /* ① 从设备树/资源里"领"自己的硬件资源 */
    res  = platform_get_resource(pdev, IORESOURCE_MEM, 0);
    base = devm_ioremap_resource(&pdev->dev, res);
    if (IS_ERR(base))
        return PTR_ERR(base);                 /* ★ 返回错误码，不要返回 -1 */

    irq = platform_get_irq(pdev, 0);
    if (irq < 0)
        return irq;

    /* ② 申请中断 */
    ret = devm_request_irq(&pdev->dev, irq, my_isr, 0, "mydev", NULL);
    if (ret)
        return ret;

    /* ③ 把私有数据挂到 device 上，后面拿得到 */
    platform_set_drvdata(pdev, my_priv);

    /* ④ 注册到子系统（字符设备/Input/SPI…） */
    ...

    dev_info(&pdev->dev, "probed\n");
    return 0;                                  /* 0 = 成功 */
}
```

**细节·🔴 `probe` 返回值的规范**：
- **成功返回 `0`**
- **失败要返回负的 errno**（`-ENOMEM`、`-EINVAL`、`PTR_ERR(ptr)`…）
- **绝对不能返回 `-1`** —— 内核会把它当成 `-EPERM`，误导排查

**追问·"为什么要有设备模型，直接在代码里注册设备不行吗"**：
可以（老式平台代码就这么干），但问题很多：① **硬编码** —— 换个板子要改驱动；② **耦合** —— 驱动直接依赖具体的板级信息；③ **无法复用** —— 同一个外设芯片在不同板子上要改代码。设备模型把"**资源描述**"和"**操作逻辑**"分开后，**同一份驱动配不同设备树节点就能支持不同板子**。

---

## 三、设备树 ⭐

### 3.1 是什么、为什么需要它

**整体**：设备树是一棵**描述硬件的数据结构**，由 bootloader 传给内核。**内核启动时把它展开成一堆 `device`，驱动再去认领。**

**机制·为什么 ARM 需要它**（这段历史很能体现理解深度）：

```
   x86 时代：设备可枚举（PCI/ACPI），内核能"自己发现"硬件
        │
   ARM 早期：SoC 五花八门，硬件不可枚举
        │
   → 每个板子在内核里写一份 board-xxx.c（几百行硬编码）
   → 内核里堆了几千个板级文件（"ARM 垃圾场"）
        │
   Linus 2011 年发飙："This whole ARM thing is a f*cking pain in the ass"
        │
        ▼
   引入设备树：★ 硬件描述从内核代码里搬出去，变成 bootloader 传的数据
   → 一个内核镜像能启动多个板子
```

**细节·🔴 三种文件的关系**：

| 文件 | 内容 | 谁写 |
|---|---|---|
| **`.dts`** | 板级描述（一块具体的板子） | 板子厂商/BSP 工程师 |
| **`.dtsi`** | **可被 include 的公共部分**（SoC 级描述） | SoC 原厂 |
| **`.dtb`** | `.dts` **编译后的二进制** | `dtc` 编译产出 |

```dts
/* myboard.dts —— 板级文件 include SoC 级的 dtsi */
/dts-v1/;
#include "rk3568.dtsi"          /* SoC 公共定义 */
#include <dt-bindings/gpio/gpio.h>

/ {
    model = "My Board";
    compatible = "vendor,myboard", "rockchip,rk3568";

    /* 板级特有：启用并补充某个外设 */
    &uart2 {
        status = "okay";        /* dtsi 里默认 disabled，这里打开 */
        pinctrl-names = "default";
        pinctrl-0 = <&uart2m0_xfer>;
    };
};
```

**细节·怎么编译和查看**：

```bash
# 编译
dtc -I dts -O dtb -o myboard.dtb myboard.dts

# 在【运行中的目标机】上看展开后的设备树
ls /proc/device-tree/                       # 目录形式
cat /proc/device-tree/model
cat /sys/firmware/devicetree/base/model     # 同上
find /sys/firmware/devicetree -name "compatible" | head
```

> **调试技巧**：`/proc/device-tree/` 是**运行时的真实设备树**，如果它和你的 `.dts` 不一致，说明 bootloader 传的不是你以为的那个 dtb（**非常常见的坑**）。

---

### 3.2 常用属性 ⭐

**整体**：设备树里的属性是**有约定的**（写在 `Documentation/devicetree/bindings/` 里）。驱动通过 `of_*` API 读它们。

| 属性 | 含义 | 驱动怎么读 |
|---|---|---|
| **`compatible`** | **最重要的属性** —— 驱动靠它匹配 | `of_match_table` 里的字符串对比 |
| **`reg`** | 寄存器地址 + 长度（`<addr len>`） | `platform_get_resource()` |
| **`interrupts`** | 中断号和触发类型 | `platform_get_irq()` / `irq_of_parse_and_map()` |
| **`clocks` / `clock-names`** | 时钟引用 | `devm_clk_get()` |
| **`pinctrl-names` / `pinctrl-0`** | 引脚复用配置 | 内核**自动**应用（驱动不用管） |
| **`status`** | `okay` / `disabled` | 内核只在 `okay` 时创建设备 |
| **`#address-cells`/`#size-cells`** | 子节点 reg 的格式 | —— |
| **`gpios`** | GPIO 引用 | `devm_gpiod_get()` |
| **自定义属性** | 驱动私有参数（比如 `reset-delay-ms`） | `of_property_read_u32()` |

**细节·🔴 `compatible` 的格式约定**：

```dts
compatible = "vendor,device-model", "vendor,fallback";
/*            ↑ 最具体的放前面，越往后越通用 */
```

驱动的匹配表：

```c
static const struct of_device_id my_of_match[] = {
    { .compatible = "vendor,device-model" },
    { .compatible = "vendor,fallback" },
    { /* sentinel */ }                     /* ★ 必须有这个空项 */
};
MODULE_DEVICE_TABLE(of, my_of_match);      /* ★ 有了它 modprobe 才能按设备树自动加载模块 */
```

**细节·🔴 `MODULE_DEVICE_TABLE(of, ...)` 的作用**：
它把匹配表**导出到模块的元信息里**，`depmod` 会生成 `modules.alias` —— 这样**设备出现时 `udev` 能自动 `modprobe` 对应的驱动模块**。不加这个，模块不会自动加载。

**细节·常用 `of_*` API 清单**：

```c
/* 节点查找 */
struct device_node *np = pdev->dev.of_node;          /* 最常用：驱动直接从这里拿 */
of_find_node_by_path("/soc/uart@ff1a0000");
of_find_compatible_node(NULL, NULL, "vendor,dev");
of_get_child_by_name(np, "subnode");
of_parse_phandle(np, "my-ref", 0);                   /* 解析 phandle 引用 */

/* 读属性 */
of_property_read_u32(np, "reset-delay-ms", &val);
of_property_read_string(np, "label", &str);
of_property_read_bool(np, "vendor,special-mode");
of_get_child_count(np);

/* 遍历子节点 */
for_each_child_of_node(np, child) { ... }
for_each_available_child_of_node(np, child) { ... }   /* 只遍历 status=okay 的 */
```

**细节·🔴 `/proc/device-tree` 里看属性的实际样子**：

```bash
# 属性在 /proc/device-tree 里是【文件】，内容是【大端二进制】
xxd /proc/device-tree/soc/uart@ff1a0000/reg
# → 00000000 ff1a0000 0000 0100   （大端！地址 + 长度）
```

**⚠️ 注意：设备树里的数值是"大端"存储的**（因为 DT 规范如此）。所以 `of_property_read_u32()` 内部帮你做了转换 —— **手动读 `of_get_property()` 拿到的原始数据是 big-endian，要自己转换**。这和 `08-总线全解与字节序.md` 里的概念一脉相承。

---

## 四、platform 总线

**整体**：`platform` 是**内核虚拟出来的一条总线**，专门用来挂 **SoC 内建的外设** —— 这些东西**没有"可枚举"的机制**（不像 PCI/USB 能自己报出 ID），所以需要人（设备树）来告诉内核它们在不在。

**机制·为什么需要 platform 总线**：

```
   PCI 设备：插上去 → 内核能读到配置空间 → 知道是什么 → 自动匹配驱动
   USB 设备：插上去 → 枚举 → 读描述符 → 自动匹配
   ★ SoC 内建 UART：焊死在芯片上，没有"插"这个动作
        → 内核无法自动发现 → 必须由【设备树】明确声明
        → 这些设备就挂在 platform 临时总线上
```

**细节·🔴 两个结构体**：

```c
/* ===== 设备（由设备树自动生成，你一般不写）===== */
struct platform_device {
    const char *name;
    int   id;
    struct device dev;              /* 内嵌的通用 device */
    u32   num_resources;
    struct resource *resource;      /* 寄存器/中断等资源 */
};

/* ===== 驱动（这个是你写的）===== */
static struct platform_driver my_driver = {
    .probe  = my_probe,
    .remove = my_remove,            /* 现代内核里返回 void；老的返回 int */
    .driver = {
        .name           = "my-device",
        .of_match_table = my_of_match,   /* ★ 设备树匹配表 */
        .pm             = &my_pm_ops,    /* 电源管理（可选） */
    },
};
module_platform_driver(my_driver);   /* ← ★ 这一个宏代替了 init/exit 两个函数 */
```

**细节·🔴 `module_platform_driver` 宏展开成什么**：

```c
/* 简化版 */
#define module_platform_driver(drv)                     \
    module_driver(drv, platform_driver_register,        \
                       platform_driver_unregister)
/* 等价于：
   static int __init xxx_init(void) { return platform_driver_register(&drv); }
   static void __exit xxx_exit(void) { platform_driver_unregister(&drv); }
   module_init(xxx_init);
   module_exit(xxx_exit);
*/
```

**所以这个宏做的事情就是"注册/注销一个 platform 驱动"** —— 面试能说出这一点，说明不是死记的。

**细节·🔴 `devm_*` 资源管理（**强烈建议用**）**：

```c
/* ❌ 老写法：每个资源都要在 remove 和错误路径里手动释放 */
base = ioremap(res->start, size);
if (!base) goto err;
irq = platform_get_irq(pdev, 0);
if (ret) goto err_unmap;
ret = request_irq(...);
...
err_unmap: iounmap(base);
err: return ret;

/* ✅ devm 写法：资源"绑定到 device 生命周期"，driver detach 时自动释放 */
base = devm_ioremap_resource(&pdev->dev, res);
irq  = platform_get_irq(pdev, 0);
ret  = devm_request_irq(&pdev->dev, irq, ...);
/* ★ 不需要 remove 里手动释放，也不需要一堆 goto 错误路径 */
```

**`devm` 的机制**：内核给每个 device 维护一个"受管资源链表"，`devres_release_all()` 在设备卸载时**逆序释放**全部。**这大幅简化了错误处理**（这是内核里公认的进步）。

**细节·❌ `devm` 不适合什么**：
它绑的是**设备**的生命周期，不是"逻辑资源"的生命周期。如果某个资源要**早于设备卸载**就释放（比如你需要在 remove 里先停掉某个东西再做别的事），就不能只靠 devm，要显式释放。

**追问·"platform_device 什么时候被创建"**：
**两种来源**：
① **设备树**：内核启动时扫描 DT 的每个节点，**对每个 `status = "okay"` 且有 `compatible` 的节点创建一个 `platform_device`**（在 `of_platform_populate()` 里）；
② **老式静态注册**：`platform_device_register()`（BSP 代码里硬编码，现在基本淘汰）。

**所以现代 ARM 上：设备树的节点 → platform_device → 和你的 platform_driver 匹配 → probe。**

---

## 五、字符设备 ⭐

**整体**：字符设备是**最基础的一类设备驱动** —— 用户态用 `open/read/write/ioctl` 操作它，**数据按字节流处理，一般不支持随机访问**。项目二的 misc 设备就属于这一类。

**机制·三条路径**（**搞清楚这三条，就搞清楚"从 open 到你的函数"怎么走的**）：

```
① 【open("/dev/mydev") 怎么找到你的驱动】
       VFS 按路径找到 /dev/mydev 的 inode
         → 从 inode 里拿到设备号（主/次）
         → 用设备号在【cdev 映射表】(cdev_map) 里查
         → 找到对应的 struct cdev → 取出它的 file_operations
         → 把 file_operations 填进 file 结构

② 【read/write/ioctl 怎么调到你的函数】
       用户调 read(fd, ...)
         → VFS 从 file 里取出 f_op
         → 调 f_op->read(file, buf, count, &offset)
         → ★ 进入你的驱动函数

③ 【你的驱动怎么把数据给用户】
       在你的 read 里：
         → copy_to_user(用户buf, 内核buf, count)   ← ★ 必须用它，不能直接赋值
```

---

### 5.1 设备号与 cdev

**整体**：设备号是"驱动的身份证" —— **主设备号标识驱动，次设备号标识该驱动的第几个设备**。

```c
#include <linux/cdev.h>
#include <linux/fs.h>

dev_t devno;                    /* 32 位：高 12 位主设备号 + 低 20 位次设备号 */
unsigned int major, minor;

/* 拆分与合成 */
major = MAJOR(devno);
minor = MINOR(devno);
devno = MKDEV(major, minor);
```

**细节·🔴 两种注册方式（**面试常问差别**）**：

| | **`register_chrdev`**（老） | **`alloc_chrdev_region` + `cdev_add`**（新，推荐） |
|---|---|---|
| 主设备号 | 传 0 则动态分配，或指定 | `alloc_chrdev_region` 动态分配 |
| 次设备号 | **全占**（一个主设备号下 0~255 都归你） | 可以只申请一段（比如 0~3） |
| 内部做的事 | 内核帮你分配 + `cdev_add` 一起做了 | **分成两步**，更灵活 |
| 多个设备 | 要用 `minor` 自己区分 | **天然支持多设备**（一次申请多个次设备号） |
| 现状 | 已不推荐 | ✅ **现代标准做法** |

**细节·🔴 完整代码骨架（含错误回滚）**：

```c
#include <linux/module.h>
#include <linux/fs.h>
#include <linux/cdev.h>
#include <linux/device.h>
#include <linux/uaccess.h>
#include <linux/slab.h>

#define DEV_NAME    "mydev"
#define DEV_CNT     1
#define DEV_BUF_SZ  256

struct my_dev {
    struct cdev    cdev;
    struct class  *cls;
    struct device *dev;
    dev_t          devno;
    char           buf[DEV_BUF_SZ];
    int            buf_len;
    struct mutex   lock;              /* 保护 buf */
};

static struct my_dev *g_dev;

/* ===== file_operations：用户态的每个操作对应这里一个成员 ===== */
static int my_open(struct inode *inode, struct file *filp)
{
    struct my_dev *d = container_of(inode->i_cdev, struct my_dev, cdev);
    filp->private_data = d;            /* ★ 挂起来，后面 read/write 从这里拿 */
    pr_info("mydev: open\n");
    return 0;
}

static int my_release(struct inode *inode, struct file *filp)
{
    pr_info("mydev: close\n");
    return 0;
}

static ssize_t my_read(struct file *filp, char __user *ubuf,
                       size_t count, loff_t *ppos)
{
    struct my_dev *d = filp->private_data;
    int n;

    if (*ppos >= d->buf_len)           /* 读完了 */
        return 0;                      /* ★ 返回 0 表示 EOF，不是错误！ */
    n = min_t(int, count, d->buf_len - *ppos);

    if (mutex_lock_interruptible(&d->lock))
        return -ERESTARTSYS;           /* ★ 被信号打断，要返回这个 */
    if (copy_to_user(ubuf, d->buf + *ppos, n)) {
        mutex_unlock(&d->lock);
        return -EFAULT;                /* ★ 用户缓冲不可访问 */
    }
    mutex_unlock(&d->lock);

    *ppos += n;
    return n;                          /* ★ 返回实际读到的字节数 */
}

static ssize_t my_write(struct file *filp, const char __user *ubuf,
                        size_t count, loff_t *ppos)
{
    struct my_dev *d = filp->private_data;
    int n = min_t(int, count, DEV_BUF_SZ - 1);

    if (mutex_lock_interruptible(&d->lock))
        return -ERESTARTSYS;
    if (copy_from_user(d->buf, ubuf, n)) {
        mutex_unlock(&d->lock);
        return -EFAULT;
    }
    d->buf_len = n;
    d->buf[n] = '\0';
    mutex_unlock(&d->lock);
    return n;
}

static const struct file_operations my_fops = {
    .owner   = THIS_MODULE,            /* ★ 必须，防止模块在使用中被卸载 */
    .open    = my_open,
    .release = my_release,
    .read    = my_read,
    .write   = my_write,
    /* .unlocked_ioctl = my_ioctl, */
    /* .poll           = my_poll,  */
    /* .mmap           = my_mmap,  */
};

/* ===== 模块初始化 ===== */
static int __init my_init(void)
{
    int ret;

    g_dev = kzalloc(sizeof(*g_dev), GFP_KERNEL);
    if (!g_dev)
        return -ENOMEM;
    mutex_init(&g_dev->lock);

    /* ① 动态申请设备号 */
    ret = alloc_chrdev_region(&g_dev->devno, 0, DEV_CNT, DEV_NAME);
    if (ret < 0)
        goto err_free;
    pr_info("mydev: major=%d\n", MAJOR(g_dev->devno));

    /* ② 初始化并添加 cdev */
    cdev_init(&g_dev->cdev, &my_fops);
    g_dev->cdev.owner = THIS_MODULE;
    ret = cdev_add(&g_dev->cdev, g_dev->devno, DEV_CNT);
    if (ret < 0)
        goto err_unreg;

    /* ③ 创建设备类 + 设备节点（udev 会自动建 /dev/mydev） */
    g_dev->cls = class_create(DEV_NAME);       /* 新内核：class_create(DEV_NAME) */
    if (IS_ERR(g_dev->cls)) {
        ret = PTR_ERR(g_dev->cls);
        goto err_cdev;
    }
    g_dev->dev = device_create(g_dev->cls, NULL, g_dev->devno,
                               g_dev, DEV_NAME);
    if (IS_ERR(g_dev->dev)) {
        ret = PTR_ERR(g_dev->dev);
        goto err_class;
    }

    pr_info("mydev: loaded, /dev/%s ready\n", DEV_NAME);
    return 0;

    /* ★ 逆序回滚 —— 每一步失败都要撤销之前做的 */
err_class:  class_destroy(g_dev->cls);
err_cdev:   cdev_del(&g_dev->cdev);
err_unreg:  unregister_chrdev_region(g_dev->devno, DEV_CNT);
err_free:   kfree(g_dev);
    return ret;
}

static void __exit my_exit(void)
{
    device_destroy(g_dev->cls, g_dev->devno);
    class_destroy(g_dev->cls);
    cdev_del(&g_dev->cdev);
    unregister_chrdev_region(g_dev->devno, DEV_CNT);
    kfree(g_dev);
    pr_info("mydev: unloaded\n");
}

module_init(my_init);
module_exit(my_exit);
MODULE_LICENSE("GPL");
```

**细节·🔴 `class_create` + `device_create` 为什么能自动建 `/dev/mydev`**：

```
   内核在 /sys/class/mydev/mydev 下创建一个"设备"
        │
        │  这个动作会产生一个【uevent】广播到用户态
        ▼
   udev（或 mdev）收到 → 读设备号 → mknod 创建 /dev/mydev
        │
        ▼
   /dev/mydev 出现，权限按 udev 规则设置
```

**注意**：如果是嵌入式精简系统（比如 BusyBox + mdev），要确认 mdev 已经在跑；**不跑 udev/mdev 的话节点不会自动出现**，这时要么手动 `mknod`，要么在启动脚本里建。

**追问·"为什么必须用 `copy_to_user` 不能直接赋值"**：
三个原因：
① **地址空间不同** —— 用户态指针在驱动的上下文里**不能直接解引用**（可能指向别的进程、或者干脆是非法地址）；
② **合法性检查** —— `copy_*_user` 内部会做 `access_ok()` 检查（范围、权限），失败返回 `-EFAULT` 而不是 oops；
③ **架构差异** —— 有些架构（比如老的 ARM）用户和内核映射方式不同，直接访问会出错。
**一句话：`copy_*_user` 是用户态和内核态之间唯一的合法数据通道。**

---

### 5.2 file_operations 各成员详解

**整体**：`file_operations` 就是"**用户态的每个文件操作 → 驱动的一个函数**"的映射表。**没实现的成员置 NULL，内核会返回合适的错误**（比如 `-EINVAL`）。

| 成员 | 对应用户态 | 要点 |
|---|---|---|
| `.owner` | —— | 🔴 **必须是 `THIS_MODULE`**，否则模块可能在使用中被卸载（**崩溃**） |
| `.open` | `open()` | 做初始化、`private_data` 挂上下文；返回 0 或负错误码 |
| `.release` | `close()` | **不是每次 close 都调**（`dup` 出来的 fd 都关了才调） |
| `.read` | `read()` | 🔴 **返回 0 表示 EOF，不是错误**；返回正数是读到的字节数 |
| `.write` | `write()` | 返回实际写入的字节数 |
| `.llseek` | `lseek()` | 不支持就置 NULL（内核返回 `ESPIPE`） |
| **`.unlocked_ioctl`** | `ioctl()` | **驱动自己负责加锁**（名字里的 unlocked 就是这个意思） |
| `.poll` | `poll()`/`select()`/`epoll` | 支持多路复用（**能在有数据时唤醒**） |
| `.mmap` | `mmap()` | 把内核/设备内存映射到用户空间（**零拷贝**） |
| `.fasync` | `fcntl(F_SETFL, O_ASYNC)` | 异步通知 |

**细节·⚠️ 一个容易错的点：`release` 不等于 `close`**：

```c
fd1 = open("/dev/mydev", O_RDWR);
fd2 = dup(fd1);          /* fd2 和 fd1 指向同一个 file 对象 */
close(fd1);              /* ★ 不会调 release！因为引用计数还是 1 */
close(fd2);              /* 这时才真正调 release */
```
**所以"打开计数"要靠 `open`/`release` 配对，不能靠用户态的 close。**

**细节·`private_data` 的作用（**这是字符设备驱动的标准模式**）**：

```c
/* open 里：把"这次打开对应的私有数据"挂到 file 上 */
filp->private_data = my_dev;

/* 之后每个操作都从这里拿 —— ★ 这是支持"多个设备实例"的关键 */
struct my_dev *d = filp->private_data;
```
配合 `container_of(inode->i_cdev, struct my_dev, cdev)`，就能**从一个 cdev 反查到外包它的整个设备结构**（这是内核里极常用的技巧）。

---

### 5.3 misc 设备（**和你的项目直接相关**）⭐

**整体**：`misc` 是**字符设备的"简化版"** —— 它**共用一个主设备号（10）**，只分配不同的次设备号，而且**帮你把 cdev 注册和节点创建都做掉了**。

**细节·🔴 为什么简单场景要用 misc**：

| | 普通字符设备 | **misc 设备** |
|---|---|---|
| 主设备号 | 要 `alloc_chrdev_region` 申请 | **固定 10**（不用申请，省一个主设备号） |
| cdev | 要 `cdev_init` + `cdev_add` | **内核帮你做** |
| 设备节点 | 要 `class_create` + `device_create` | **内核帮你做** |
| 代码量 | 约 60 行初始化 | **约 10 行** |
| 次设备号 | 要自己管 | `misc_register` 自动分配 |

**细节·🔴 misc 驱动的最小骨架**：

```c
#include <linux/miscdevice.h>

static ssize_t my_read(struct file *filp, char __user *ubuf,
                       size_t count, loff_t *ppos) { ... }

static const struct file_operations my_fops = {
    .owner = THIS_MODULE,
    .read  = my_read,
};

static struct miscdevice my_misc = {
    .minor = MISC_DYNAMIC_MINOR,       /* ★ 动态分配次设备号 */
    .name  = "misc_sandblasting",      /* → 自动创建 /dev/misc_sandblasting */
    .fops  = &my_fops,
};

static int __init my_init(void)
{
    return misc_register(&my_misc);    /* ★ 一行搞定注册 + 建节点 */
}
static void __exit my_exit(void)
{
    misc_deregister(&my_misc);
}
module_init(my_init); module_exit(my_exit);
MODULE_LICENSE("GPL");
```

**细节·🔴 和你项目的对应关系（**这段最值得讲**）**：

```
   项目二的 /dev/misc_sandblasting
        │
        ├─ 就是这个 misc 驱动注册出来的（主设备号 10）
        │
        ├─ 应用层怎么用：
        │     QFile 打开 → 加 QSocketNotifier → fd 可读 → readAll()
        │        ↑
        │        这就是"基于驱动提供的 read 接口写数据接收逻辑"
        │
        └─ 数据从哪来：驱动在设备侧收到数据后
             唤醒等待队列 → 用户态的 read 返回数据
```

**追问·"misc 驱动怎么实现阻塞 read"**（**这个如果能答，说明你真的理解了，不只是"调用 read 接口"**）：

```c
/* 驱动侧：把"等数据"变成等待队列 */
static DECLARE_WAIT_QUEUE_HEAD(my_wq);
static int have_data = 0;

static ssize_t my_read(...)
{
    /* ① 没数据就睡在等待队列上（可被信号打断） */
    if (wait_event_interruptible(my_wq, have_data != 0))
        return -ERESTARTSYS;

    /* ② 有数据了，取走 */
    have_data = 0;
    if (copy_to_user(ubuf, kbuf, n))
        return -EFAULT;
    return n;
}

/* 中断/硬件回调里：有新数据了 → 唤醒 */
have_data = 1;
wake_up_interruptible(&my_wq);
```

**这就完整串起来了**：**驱动的"阻塞 read" → 用户态 `read` 返回 → Qt 的 `QSocketNotifier` 收到通知 → 你的 `readAll()` 拿到数据**。
**能讲清这条链，就说明你不仅"调用了 read 接口"，而是理解了它在整个系统里的位置。** 这是面试里最能加分的部分。

---

## 六、并发与竞态（内核视角）

**整体**：内核里的并发源**比用户态多得多**，所以"什么锁能用"有**硬约束**。这一节的核心就一句话：**能不能睡眠**。

**机制·🔴 内核里的五种并发源**：

| 并发源 | 说明 |
|---|---|
| **多核（SMP）** | 真并行 —— 两个 CPU 同时跑内核代码 |
| **中断** | 打断当前代码，**且中断可能在任何时候发生** |
| **软中断 / tasklet** | 中断下半部，也会打断进程上下文 |
| **内核抢占** | `CONFIG_PREEMPT` 打开后，内核代码也能被抢占 |
| **多进程** | 多个进程同时 open 同一个设备 |

**细节·🔴 锁的选型表（**这是本节最该背的**）**：

| 锁 | 能否睡眠 | 中断里能用 | 适用 |
|---|---|---|---|
| **自旋锁 `spinlock_t`** | ❌ **绝对不能睡眠** | ✅（用 `spin_lock_irqsave`） | **极短临界区**、中断上下文 |
| **互斥体 `struct mutex`** | ✅ 可睡眠 | ❌ **中断里绝对不能用** | **进程上下文、可能较长的临界区** ← **最常用** |
| **信号量 `struct semaphore`** | ✅ 可睡眠 | ❌ | 计数型资源（现代内核更推荐 mutex/completion） |
| **读写锁 `rwlock_t` / `rwsem`** | rwlock ❌ / rwsem ✅ | rwlock ✅ | 读多写少 |
| **原子操作 `atomic_t`** | —— | ✅ | **单个整数**（计数器、标志） |
| **完成量 `completion`** | ✅ | 可 `complete` | **"等某件事发生"**（比信号量语义更清晰） |
| **RCU** | —— | ✅ | **读多写极少**，读侧零开销 |

**细节·🔴 三条硬规则（**答错直接暴露**）**：

1. **中断上下文（硬中断/tasklet/软中断）里不能睡眠** → 所以**不能用 mutex**，只能用 `spinlock`（且要用 `_irqsave` 版本）
2. **持有自旋锁期间不能睡眠** → 不能调 `kmalloc(GFP_KERNEL)`、不能 `copy_to_user`（可能缺页睡眠）、不能 `mutex_lock`
3. **`copy_to_user` / `copy_from_user` 会睡眠**（缺页处理）→ **不能在中断里、不能在自旋锁里调用**

**细节·`spin_lock` 的几个变体（为什么要用 `_irqsave`）**：

```c
/* 场景：进程上下文和中断都可能访问同一份数据 */
unsigned long flags;

/* ❌ 只关抢占，不关中断 → 如果中断在临界区里进来抢同一把锁 → 死锁 */
spin_lock(&lock);

/* ✅ 关中断 + 关抢占，并保存标志（用完恢复） */
spin_lock_irqsave(&lock, flags);
   /* ... 临界区：极短，不能睡眠 ... */
spin_unlock_irqrestore(&lock, flags);
```

**为什么必须关中断**：自旋锁是"**忙等**"——如果低优先级的进程拿到了锁、然后被中断打断，而中断服务里又要抢同一把锁，那中断会**永远自旋**（因为持锁的进程根本跑不到），**死锁**。

**细节·`mutex` 的规范用法**：

```c
struct mutex my_lock;
mutex_init(&my_lock);

/* 不可打断（信号杀不掉，慎用） */
mutex_lock(&my_lock);
mutex_unlock(&my_lock);

/* ✅ 推荐：可被信号打断，用户 Ctrl+C 能退出 */
if (mutex_lock_interruptible(&my_lock))
    return -ERESTARTSYS;        /* ★ 被信号打断要返回这个，让内核处理重试 */
mutex_unlock(&my_lock);
```

**追问·"为什么 `mutex_lock_interruptible` 更好"**：
不可打断的版本下，如果持锁的线程卡住了，**用户只能 `kill -9` 整个进程**（而 `kill -9` 杀不掉正在内核里等的线程，只能等）。可打断版本让用户能被信号唤醒退出。**内核社区强烈不建议在用户可触发的路径上用不可打断版本。**

---

## 七、中断处理 ⭐

### 7.1 申请中断

```c
#include <linux/interrupt.h>

static irqreturn_t my_isr(int irq, void *dev_id)
{
    /* ★ 极短、不能睡眠、不能 copy_to_user */
    ...
    return IRQ_HANDLED;            /* 或者 IRQ_NONE（不是我的中断） */
}

/* 申请 */
ret = request_irq(irq, my_isr,
                  IRQF_TRIGGER_FALLING | IRQF_SHARED,
                  "mydev",              /* 会出现在 /proc/interrupts 里 */
                  dev_id);              /* ★ 共享中断时必须唯一，用来区分设备 */
if (ret)
    return ret;

/* 释放（devm 版就不用管） */
free_irq(irq, dev_id);
```

**细节·🔴 `dev_id` 的作用**：
① 共享中断时用来**区分是哪个设备的中断**；② `free_irq` 时用来**精确释放自己那一条**。**非共享中断也建议传**（注册到 `/proc/interrupts` 便于调试）。

**细节·常用 flag**：

| flag | 含义 |
|---|---|
| `IRQF_TRIGGER_RISING/FALLING/HIGH/LOW` | 触发方式（**设备树里也配了，以哪个为准看驱动**） |
| `IRQF_SHARED` | **共享中断线**（多个设备用同一根 IRQ） |
| 🔴 `IRQF_DISABLED` | 老内核里表示"处理期间关掉所有中断"——**已废弃，别用** |
| `IRQF_ONESHOT` | 线程化中断里保证"下半部执行完之前不重新使能中断" |
| `IRQF_NO_SUSPEND` | 系统挂起期间也不禁用 |

**细节·🔴 为什么"设备树里配了触发方式，驱动也配"会冲突**：
设备树里的 `interrupts` 属性**已经包含了触发类型**（`IRQ_TYPE_EDGE_FALLING` 之类），内核会用它。如果驱动里又写了 `IRQF_TRIGGER_*`，**两者不一致时行为取决于具体实现**（有的会以驱动为准并打警告）。**规范做法：触发类型写在设备树里，驱动里不加 `IRQF_TRIGGER_*`。**

---

### 7.2 上半部 / 下半部 ⭐

**整体**：中断处理必须**快**（否则影响其他中断和调度），但很多活又不得不做。**解法是把中断处理切成两半**。

```
   硬件中断
      │
      ▼
   ┌──────────────────────────────┐
   │ 上半部（硬中断，你注册的 ISR）  │  ★ 越短越好
   │  · 清中断标志（必须）           │
   │  · 读走硬件寄存器里的数据        │
   │  · 唤醒下半部 / 投递工作         │
   └──────────────┬───────────────┘
                  │ 推迟
                  ▼
   ┌──────────────────────────────┐
   │ 下半部                        │  ★ 可以长一点
   │  · 数据处理、协议解析           │
   │  · 唤醒等待的进程               │
   │  · 可能还要睡眠的操作           │
   └──────────────────────────────┘
```

**细节·🔴 四种下半部机制对比（**这张表是本节核心**）**：

| 机制 | 运行上下文 | 能否睡眠 | 并发性 | 适用 |
|---|---|---|---|---|
| **软中断 softirq** | 中断上下文（软中断） | ❌ | **同类型可在多核并行** | 内核核心（网络、块设备）——**驱动很少直接用** |
| **tasklet** | 中断上下文（软中断） | ❌ | **同一 tasklet 不并发**（序列化） | 简单的一次性推迟任务 |
| **工作队列 workqueue** | **进程上下文**（内核线程） | ✅ **可以** | 可并发 | 🔴 **需要睡眠/耗时的活**（最常用） |
| **线程化中断 `request_threaded_irq`** | **进程上下文**（内核线程） | ✅ | 可配 | 🔴 **现代推荐**：上半部极短（只清标志），其余全放线程里 |

**细节·🔴 优先级关系（**面试常考**）**：
```
   硬中断  >  软中断/tasklet  >  内核线程/进程
```
所以 **tasklet 会打断普通进程，但不会打断硬中断**。这也解释了为什么 tasklet 里不能睡眠 —— 它仍在"中断上下文"。

**细节·🔴 线程化中断（`request_threaded_irq`）的写法**：

```c
/* 上半部：只清标志，返回 IRQ_WAKE_THREAD 表示"去干活" */
static irqreturn_t my_hard_isr(int irq, void *dev_id)
{
    /* 清中断标志（必须） */
    return IRQ_WAKE_THREAD;         /* ★ 唤醒下面的线程 */
}

/* 下半部：完整处理，★ 这里可以睡眠 */
static irqreturn_t my_thread_fn(int irq, void *dev_id)
{
    /* 可以调 mutex_lock、可以 kmalloc(GFP_KERNEL)、可以 copy_to_user */
    return IRQ_HANDLED;
}

/* 注册 */
ret = request_threaded_irq(irq, my_hard_isr, my_thread_fn,
                           IRQF_TRIGGER_FALLING | IRQF_ONESHOT,
                           "mydev", dev_id);
```

**`IRQF_ONESHOT` 的意义**：保证**线程化处理完之前不会再触发这个中断** —— 因为上半部只清标志没读到数据，如果中断马上又来，会丢数据/重复唤醒。

**追问·"怎么选下半部机制"**：
一句话：**需要睡眠就用工作队列或线程化中断；不需要睡眠且很短就用 tasklet**。
但现代趋势是 **`request_threaded_irq`** —— 它让代码结构更清晰（上半部/下半部分成两个明确的函数），也不用自己管工作队列的创建销毁。**新的驱动应该优先考虑它。**

---

## 八、用户态接口

**整体**：驱动对外提供接口有**四种方式**，各有场景。**选对了接口，用户态代码就简单一半。**

| 接口 | 适合 | 优点 | 缺点 |
|---|---|---|---|
| **ioctl** | **二进制数据、复杂控制、原子多字段操作** | 灵活、一次一命令 | 要写测试程序、不好调试 |
| **sysfs** | **简单标量、配置项** | `echo`/`cat` 就能用、**调试友好** | 一页限制、不适合二进制 |
| **debugfs** | **纯调试用**（不进正式接口） | 随便加、不占 sysfs 命名空间 | 不许当正式接口（内核社区明确禁止） |
| **procfs** | 进程/内核状态（**新驱动不要用它做接口**） | 兼容老习惯 | 命名空间混乱、已不推荐 |

**细节·🔴 sysfs 属性的完整写法**：

```c
/* 定义：read 对应 cat，write 对应 echo */
static ssize_t gain_show(struct device *dev,
                         struct device_attribute *attr, char *buf)
{
    struct my_dev *d = dev_get_drvdata(dev);
    return sysfs_emit(buf, "%d\n", d->gain);   /* ★ 用 sysfs_emit，别用 sprintf */
}

static ssize_t gain_store(struct device *dev,
                          struct device_attribute *attr,
                          const char *buf, size_t count)
{
    struct my_dev *d = dev_get_drvdata(dev);
    int val, ret;

    ret = kstrtoint(buf, 0, &val);        /* ★ 用 kstrtoint，别用 simple_strtoul */
    if (ret)
        return ret;
    if (val < 0 || val > 100)             /* ★ 一定要做范围校验！ */
        return -EINVAL;

    d->gain = val;
    return count;                          /* ★ 返回 count（不是 0） */
}

static DEVICE_ATTR_RW(gain);              /* 声明成可读可写 */

/* 注册（推荐用属性组，一次注册多个） */
static struct attribute *my_attrs[] = {
    &dev_attr_gain.attr,
    NULL,
};
ATTRIBUTE_GROUPS(my);                     /* 生成 my_groups */
```

**细节·🔴 sysfs 接口的三条纪律**（**答出这些说明你看过内核文档**）：
1. **一个文件只放一个值** —— 不要用空格分隔的复合数据（用户态解析很麻烦）
2. **一定要做参数校验** —— 用户态传什么进来都可能（`echo abc > gain`）
3. **`store` 返回 `count`**（不是 0、不是 1），否则用户态 `echo` 会报错

**细节·`poll` 的实现（**支持非阻塞 + 多路复用**）**：

```c
static __poll_t my_poll(struct file *filp, poll_table *wait)
{
    struct my_dev *d = filp->private_data;
    __poll_t mask = 0;

    poll_wait(filp, &d->readq, wait);     /* ★ 注册等待队列 */

    if (d->have_data)
        mask |= EPOLLIN | EPOLLRDNORM;    /* 可读 */
    if (d->space_left)
        mask |= EPOLLOUT | EPOLLWRNORM;   /* 可写 */

    return mask;
}
```

**这就是为什么 `QSocketNotifier` 能工作** —— Qt 底下用 `epoll` 等这个 fd，`poll` 返回可读时就通知你。

**追问·"`ioctl` 和 `sysfs` 怎么选"**：
**调试用 sysfs**（`cat`/`echo` 就能试，不用写程序）；**正式接口用 ioctl**（能传二进制、能做原子多字段操作、不污染 sysfs）。**实践中常常两者都有**：sysfs 保留几个关键参数方便现场调试，正式的数据通路走 ioctl 或 read/write。

---

## 九、Input 子系统

**整体**：Input 子系统解决的是"**各种输入设备（键盘、鼠标、触摸屏、按键、摇杆）如何用同一套模型表达**"的问题。**驱动只需要"上报发生了什么事件"，不用管用户态怎么读。**

**机制·分层结构**：

```
   硬件（按键/触摸屏/编码器）
        │
        ▼
   ┌─────────────────────────┐
   │ Input 驱动（你写的）      │  input_dev + input_report_*
   │ 把硬件事件翻译成标准事件  │
   └───────────┬─────────────┘
               │  input_event 事件流
               ▼
   ┌─────────────────────────┐
   │ Input 核心                │  事件分发
   └───────────┬─────────────┘
               │
   ┌───────────┴─────────────┐
   │ evdev handler            │  → /dev/input/eventX
   └─────────────────────────┘
               │
               ▼
   用户态：read(/dev/input/eventX) 拿到 struct input_event
```

**细节·🔴 一个按键驱动的完整写法**：

```c
#include <linux/input.h>

struct my_input {
    struct input_dev *input;
    int gpio;
    int irq;
};

static irqreturn_t btn_isr(int irq, void *dev_id)
{
    struct my_input *d = dev_id;
    int val = gpiod_get_value(d->gpio);   /* 读当前电平 */

    /* ★ 上报事件：type / code / value */
    input_report_key(d->input, KEY_ENTER, val ? 0 : 1);
    input_sync(d->input);                  /* ★ 必须！表示"这一组事件完了" */
    return IRQ_HANDLED;
}

static int my_probe(struct platform_device *pdev)
{
    struct my_input *d;
    int ret;

    d = devm_kzalloc(&pdev->dev, sizeof(*d), GFP_KERNEL);
    if (!d) return -ENOMEM;

    /* ① 申请 input 设备 */
    d->input = devm_input_allocate_device(&pdev->dev);
    if (!d->input) return -ENOMEM;

    /* ② 描述设备能力（★ 用户态靠这个知道设备能干什么） */
    d->input->name = "my-button";
    d->input->phys = "my/input0";
    d->input->id.bustype = BUS_HOST;

    __set_bit(EV_KEY, d->input->evbit);        /* 我能产生"按键"类事件 */
    __set_bit(KEY_ENTER, d->input->keybit);    /* 我具体能报这些键 */

    /* ③ 注册到 Input 核心 */
    ret = input_register_device(d->input);
    if (ret) return ret;

    /* ④ 申请中断 */
    d->gpio = devm_gpiod_get(&pdev->dev, "button", GPIOD_IN);
    d->irq  = gpiod_to_irq(d->gpio);
    ret = devm_request_irq(&pdev->dev, d->irq, btn_isr,
                           IRQF_TRIGGER_FALLING | IRQF_TRIGGER_RISING,
                           "my-button", d);
    if (ret) return ret;

    platform_set_drvdata(pdev, d);
    return 0;
}
```

**细节·🔴 四种事件类型（`type` 字段）**：

| type | 含义 | 上报函数 |
|---|---|---|
| `EV_KEY` | 按键/按钮（**有值 0/1/2 = 抬起/按下/连发**） | `input_report_key` |
| `EV_REL` | **相对**位移（鼠标、滚轮） | `input_report_rel` |
| `EV_ABS` | **绝对**位置（触摸屏、摇杆） | `input_report_abs` |
| `EV_SYN` | **同步标记**（一组事件结束） | `input_sync` |
| （`EV_MSC`/`EV_LED`/`EV_SND`/`EV_REP`） | 杂项/LED/声音/自动重复 | —— |

**细节·🔴 `input_sync` 为什么必须调**：
一次物理动作可能产生**多个事件**（比如触摸屏一次按下会产生 X 坐标 + Y 坐标 + 按下状态三个事件）。**`input_sync` 是分隔符**，告诉用户态"这一批是同一个时刻的，可以一起处理了"。不调的话，用户态可能**读到一半的状态**（X 更新了 Y 还没更新）。

**细节·用户态怎么读**：

```c
/* 用户态读 /dev/input/eventX，拿到的是 struct input_event 数组 */
struct input_event {
    struct timeval time;    /* 时间戳 */
    __u16 type;             /* EV_KEY / EV_ABS ... */
    __u16 code;             /* KEY_ENTER / ABS_X ... */
    __s32 value;            /* 值 */
};
/* 注意：一次 read 可能返回多个事件，要循环解析！ */
```

**追问·"为什么 Input 子系统比自己写字符设备好"**：
① **自动进 `/dev/input/eventX`**，不用自己建节点；
② **用户态有标准做法**（`libinput`、`evtest` 工具、Qt 的输入插件都认这套）；
③ **自动处理事件同步和缓冲**；
④ **能被上层的输入框架直接使用**（比如 Qt 的 `QPA` 输入插件）。
**自己写字符设备的话，这些都要自己实现，而且用户态没法复用现成的库。**

> **和项目一的关联**：项目一的按键/EXTI 是**直接在 STM32 固件里处理**的（没有 Linux 这一层）；但如果这个巡检机器人的**现场操作终端**要接物理按键，那就应该在 Linux 侧用 Input 子系统做 —— **这样 Qt 能直接收到按键事件，不用自己写驱动+协议**。这个对比能体现你理解"什么时候该用子系统"。

---

## 十、SPI 子系统

**整体**：SPI 子系统把"**控制器驱动**"和"**设备驱动**"分开 —— 你写的是**设备驱动**（针对挂在 SPI 上的那颗芯片），控制器驱动（SPI master）由 SoC 厂商提供。

**机制·分层**：

```
   ┌──────────────────────────────────────────┐
   │  SPI 设备驱动（你写的）                     │
   │   spi_driver + probe + spi_sync           │
   └──────────────┬───────────────────────────┘
                  │ spi_transfer / spi_message
   ┌──────────────┴───────────────────────────┐
   │  SPI 核心（drivers/spi/spi.c）             │
   │   通用逻辑：消息队列、片选管理、模式设置      │
   └──────────────┬───────────────────────────┘
                  │
   ┌──────────────┴───────────────────────────┐
   │  SPI 控制器驱动（SoC 厂商提供）             │
   │   spi_master / spi_controller             │
   │   真正操作硬件寄存器、控制时钟和片选          │
   └──────────────────────────────────────────┘
```

**细节·🔴 两个结构体**：

| 结构 | 谁提供 | 表示 |
|---|---|---|
| **`spi_controller`（旧名 `spi_master`）** | **SoC 厂商** | **一个 SPI 控制器**（一条 SPI 总线） |
| **`spi_device`** | 设备树/板级代码 | **挂在总线上的一个从设备**（含片选、模式、速率） |
| **`spi_driver`** | **你** | 针对**某一类芯片**的驱动 |

**细节·🔴 设备驱动的完整写法**：

```c
#include <linux/spi/spi.h>

static const struct of_device_id my_spi_of_match[] = {
    { .compatible = "vendor,my-spi-chip" },
    { }
};
MODULE_DEVICE_TABLE(of, my_spi_of_match);

/* 旧的 id_table（非设备树平台用） */
static const struct spi_device_id my_spi_id[] = {
    { "my-spi-chip", 0 },
    { }
};
MODULE_DEVICE_TABLE(spi, my_spi_id);

static int my_spi_probe(struct spi_device *spi)
{
    u8 tx[2] = { 0x80, 0x00 };        /* 比如：读寄存器 0x00 */
    u8 rx[2] = { 0 };
    struct spi_transfer xfer = {
        .tx_buf = tx,
        .rx_buf = rx,
        .len    = sizeof(tx),
    };
    struct spi_message msg;
    int ret;

    /* ① 配置 SPI 模式（CPOL/CPHA）和速率——也可以在设备树里配 */
    spi->mode = SPI_MODE_0;
    spi->max_speed_hz = 1000000;      /* 1MHz */
    ret = spi_setup(spi);
    if (ret) return ret;

    /* ② 组一个 message（可以包含多个 transfer）*/
    spi_message_init(&msg);
    spi_message_add_tail(&xfer, &msg);

    /* ③ 同步发送 —— ★ 会睡眠！所以只能在进程上下文调用 */
    ret = spi_sync(spi, &msg);
    if (ret)
        return ret;

    dev_info(&spi->dev, "reg0 = 0x%02x\n", rx[1]);
    return 0;
}

static struct spi_driver my_spi_driver = {
    .probe  = my_spi_probe,
    .remove = my_spi_remove,
    .driver = {
        .name           = "my-spi-chip",
        .of_match_table = my_spi_of_match,
    },
    .id_table = my_spi_id,
};
module_spi_driver(my_spi_driver);
```

**细节·🔴 `spi_transfer` 和 `spi_message` 的关系**（**这题很能区分懂不懂**）：

```
   spi_message
   ├─ spi_transfer ①   ← 可以设置片选控制、延时
   ├─ spi_transfer ②
   └─ spi_transfer ③

   · 一个 message 内的多个 transfer 是【连续】的：
     ★ 中间【不会释放片选】—— 这是关键！
   · 不同 message 之间片选会抬起
```

**所以"先发命令、再读数据、中间片选一直拉着"这种时序，要放在同一个 message 里**（用多个 transfer）。如果分成两次 `spi_sync`，片选会被抬起，很多芯片就会复位状态机。

**细节·🔴 `spi_sync` vs `spi_async`**：

| | `spi_sync` | `spi_async` |
|---|---|---|
| 行为 | **阻塞等待完成** | 异步，完成后回调 |
| 能否睡眠 | ✅ **会睡眠** | ❌ 可在中断里调用 |
| 上下文 | **只能进程上下文** | 中断也可 |
| 用在哪 | 大多数场景（简单） | 中断里发起传输 |

**细节·设备树里怎么描述**：

```dts
&spi1 {
    status = "okay";
    pinctrl-names = "default";
    pinctrl-0 = <&spi1m0_pins>;
    cs-gpios = <&gpio1 RK_PA4 GPIO_ACTIVE_LOW>;

    my_chip: my-spi-chip@0 {
        compatible = "vendor,my-spi-chip";
        reg = <0>;                          /* ★ 片选编号（对应 cs-gpios 的第几个） */
        spi-max-frequency = <1000000>;      /* ★ 这条总线上这个设备的最大时钟 */
        spi-cpol;                           /* 可选：CPOL=1 */
        spi-cpha;                           /* 可选：CPHA=1 */
        interrupt-parent = <&gpio1>;
        interrupts = <RK_PB0 IRQ_TYPE_LEVEL_LOW>;
        vdd-supply = <&vcc_3v3>;            /* 供电（用于电源管理） */
    };
};
```

**追问·"`reg = <0>` 是什么"**：
SPI 不像 I2C 有地址 —— **`reg` 表示这个设备用哪个片选（CS）**。`reg = <0>` 表示用第 0 个片选。**这是 SPI 设备树里最容易搞错的地方**（很多人以为 reg 是地址）。

---

## 十一、I2C 子系统

**整体**：和 SPI 子系统同构 —— **控制器驱动（adapter）由 SoC 厂商提供，你写设备驱动（client driver）**。

**机制·三个结构体**：

| 结构 | 表示 | 谁提供 |
|---|---|---|
| **`i2c_adapter`** | 一个 I2C 控制器（一条总线） | SoC 厂商 |
| **`i2c_client`** | 挂在总线上的一个从设备（**含 7 位地址**） | 设备树/板级代码 |
| **`i2c_driver`** | 针对一类芯片的驱动 | **你** |

**细节·🔴 设备驱动写法**：

```c
#include <linux/i2c.h>

static const struct of_device_id my_i2c_of_match[] = {
    { .compatible = "vendor,my-i2c-chip" },
    { }
};
MODULE_DEVICE_TABLE(of, my_i2c_of_match);

static int my_i2c_probe(struct i2c_client *client)
{
    u8 reg = 0x00, val = 0;
    int ret;

    /* 方式 A：用 i2c_transfer（最通用，能做任意时序） */
    {
        u8 wbuf[1] = { reg };
        struct i2c_msg msgs[2] = {
            { .addr = client->addr, .flags = 0,        /* 写 */
              .len = 1, .buf = wbuf },
            { .addr = client->addr, .flags = I2C_M_RD, /* 读（重复起始） */
              .len = 1, .buf = &val },
        };
        ret = i2c_transfer(client->adapter, msgs, 2);
        if (ret != 2)
            return ret < 0 ? ret : -EIO;
    }

    /* 方式 B：用 smbus 封装（★ 更常用，代码短） */
    ret = i2c_smbus_read_byte_data(client, reg);
    if (ret < 0)
        return ret;
    val = (u8)ret;

    dev_info(&client->dev, "reg0 = 0x%02x\n", val);
    return 0;
}

static struct i2c_driver my_i2c_driver = {
    .probe  = my_i2c_probe,
    .remove = my_i2c_remove,
    .driver = {
        .name           = "my-i2c-chip",
        .of_match_table = my_i2c_of_match,
    },
    .id_table = my_i2c_id,
};
module_i2c_driver(my_i2c_driver);
```

**细节·🔴 `i2c_transfer` vs `i2c_smbus_*` 的取舍**：

| | `i2c_transfer` | `i2c_smbus_*` |
|---|---|---|
| 表达力 | **任意时序**（多段、重复起始、无地址段） | 标准 SMBus 操作 |
| 代码量 | 多（要手填 `i2c_msg` 数组） | 少（一行） |
| 适用 | 特殊时序的芯片 | **绝大多数标准寄存器读写** |

常用的 smbus 封装：
```c
i2c_smbus_read_byte_data(client, reg);              /* 读 1 字节 */
i2c_smbus_write_byte_data(client, reg, val);        /* 写 1 字节 */
i2c_smbus_read_word_data(client, reg);              /* 读 2 字节 */
i2c_smbus_read_i2c_block_data(client, reg, len, buf);/* 读一块 */
```

**细节·🔴 设备树里怎么描述**：

```dts
&i2c1 {
    status = "okay";
    clock-frequency = <100000>;        /* ★ 100kHz（标准模式）或 400000（快速模式） */
    pinctrl-names = "default";
    pinctrl-0 = <&i2c1m0_xfer>;

    /* 从设备：★ reg 就是 7 位 I2C 地址 */
    my_chip: my-i2c-chip@50 {
        compatible = "vendor,my-i2c-chip";
        reg = <0x50>;                  /* 7 位地址 0x50 */
        interrupt-parent = <&gpio1>;
        interrupts = <RK_PB1 IRQ_TYPE_EDGE_FALLING>;
    };
};
```

**细节·⚠️ `reg` 到底是 7 位还是 8 位**（**这是个真实的高频坑**）：
设备树里的 `reg` 是**7 位地址**（比如 `0x50`）。
而**芯片手册上给的地址常常是 8 位形式**（`0xA0` = `0x50 << 1`，最低位是读写位）。
**所以从手册抄地址时要注意是否需要右移一位** —— 这和你写的 `08-总线全解与字节序.md` 里 I2C 那节提到的"地址 7 位 vs 8 位"是同一个坑。

**细节·用户态也能直接访问 I2C**（**这个绕开驱动的方式很实用**）：

```bash
# 需要 i2c-dev 模块
ls /dev/i2c-*
i2cdetect -y 1              # 扫描总线上的设备
i2cget -y 1 0x50 0x00       # 读寄存器
i2cset -y 1 0x50 0x00 0x12  # 写寄存器
```
**调试阶段用 `i2c-tools` 先确认硬件通不通，再写驱动** —— 这是很标准的做法，能省很多时间。

**追问·"SPI 和 I2C 驱动写法有什么不同"**：

| | SPI | I2C |
|---|---|---|
| 设备标识 | **片选编号**（`reg = <0>`） | **7 位地址**（`reg = <0x50>`） |
| 传输单位 | `spi_message` + `spi_transfer` | `i2c_msg` 数组 |
| 速率配置 | **每个设备**（`spi-max-frequency`） | **每条总线**（`clock-frequency`） |
| 时序说明 | 需要 `CPOL/CPHA`（`spi-cpol`/`spi-cpha`） | 固定（起始/地址/ACK/停止） |
| 是否有 ACK | ❌ 无 | ✅ 有（**所以能用 `i2cdetect` 扫地址**） |
| 用户态调试 | 没有通用工具 | ✅ **`i2c-tools` 很成熟** |

---

## 十二、PWM 子系统

**整体**：PWM 子系统把"**产生一个可调频率和占空比的方波**"抽象成统一接口 —— 驱动提供 PWM 通道，**消费者**（背光、电机、蜂鸣器驱动）用它。

**机制·两层**：

```
   ┌──────────────────────────────────────────┐
   │  PWM 消费者（背光驱动 / 电机驱动 / 你的代码）│
   │    pwm_get → pwm_apply_state              │
   └──────────────┬───────────────────────────┘
                  │
   ┌──────────────┴───────────────────────────┐
   │  PWM 核心（drivers/pwm/core.c）            │
   └──────────────┬───────────────────────────┘
                  │
   ┌──────────────┴───────────────────────────┐
   │  PWM 控制器驱动（pwm_chip，SoC 厂商提供）   │
   │    .apply / .config / .enable             │
   └──────────────────────────────────────────┘
```

**细节·🔴 关键概念：周期和占空比都用【纳秒】**：

```c
struct pwm_state {
    u64 period;        /* ★ 周期，单位 ns —— 不是 Hz！ */
    u64 duty_cycle;    /* ★ 占空比，单位 ns —— 不是百分比！ */
    enum pwm_polarity polarity;   /* 正常/反转 */
    bool enabled;
};
```

**所以 1kHz、50% 占空比要这么写**：
```c
period     = 1000000 ns;    /* 1kHz → 周期 1ms = 1,000,000 ns */
duty_cycle =  500000 ns;    /* 50% */
```

**细节·🔴 现代 API 与旧 API**（**内核版本差异，值得提一句**）：

```c
/* ===== 新 API（推荐，内核 4.7+ 逐渐成为标准）===== */
struct pwm_state state;
pwm_init_state(pwm, &state);                  /* 或者 devm_pwm_get 后 pwm_get_state */
state.period     = 1000000;
state.duty_cycle = 500000;
state.polarity   = PWM_POLARITY_NORMAL;
state.enabled    = true;
ret = pwm_apply_state(pwm, &state);
/* 注意：在某些内核版本里这个函数叫 pwm_apply_might_sleep() */

/* ===== 旧 API（已废弃，但老代码里还有）===== */
pwm_config(pwm, 500000, 1000000);    /* (duty, period) 单位 ns */
pwm_enable(pwm);
pwm_disable(pwm);
```

**面试建议**：说"现代内核用 `pwm_apply_state`（`pwm_state` 里放 period/duty，单位纳秒）；老的 `pwm_config`/`pwm_enable` 已废弃但很多老代码还在用" —— **能主动区分新旧 API，说明你不是照抄的**。

**细节·🔴 用 PWM 的完整流程**：

```c
#include <linux/pwm.h>

static int my_probe(struct platform_device *pdev)
{
    struct pwm_device *pwm;
    struct pwm_state state;
    int ret;

    /* ① 获取 PWM 通道（从设备树里按名字找） */
    pwm = devm_pwm_get(&pdev->dev, "my-pwm");     /* 名字对应 pwm-names */
    if (IS_ERR(pwm))
        return PTR_ERR(pwm);

    /* ② 配置 */
    pwm_init_state(pwm, &state);
    state.period     = 1000000;      /* 1ms → 1kHz */
    state.duty_cycle = 500000;       /* 50% */
    state.enabled    = true;
    ret = pwm_apply_state(pwm, &state);
    if (ret)
        return ret;

    return 0;
}
```

**细节·设备树里怎么描述**：

```dts
/* ① 控制器本身（SoC 的 dtsi 里已有，一般只要 status = "okay"） */
&pwm4 {
    status = "okay";
    pinctrl-names = "default";
    pinctrl-0 = <&pwm4m0_pins>;
};

/* ② 消费者引用它 */
my_device: my-device {
    compatible = "vendor,my-device";
    pwms = <&pwm4 0 1000000 0>;
    /*      ↑通道 ↑周期ns ↑极性 */
    pwm-names = "my-pwm";        /* ← 对应 devm_pwm_get(dev, "my-pwm") */
};
```

**细节·🔴 `pwms` 属性里那三个数的含义**：

```
pwms = <&pwm4  0  1000000  0>;
          │    │     │     └─ 极性（PWM_POLARITY_NORMAL=0）
          │    │     └─────── 初始周期（ns）
          │    └───────────── 通道索引（一个控制器可能有多路 PWM 输出）
          └────────────────── phandle（引用哪个 PWM 控制器）
```
**这个顺序很容易记错** —— 周期和通道索引的位置反了的话，会配出一个完全不同的频率。

**细节·用户态调试 PWM（sysfs 接口）**：

```bash
ls /sys/class/pwm/
echo 0 > /sys/class/pwm/pwmchip0/export       # 导出通道 0
ls /sys/class/pwm/pwmchip0/pwm0/

echo 1000000 > /sys/class/pwm/pwmchip0/pwm0/period     # 周期 1ms（ns）
echo  500000 > /sys/class/pwm/pwmchip0/pwm0/duty_cycle # 占空比 50%
echo 1       > /sys/class/pwm/pwmchip0/pwm0/enable     # 使能
```
**这也是调试利器** —— 先用 sysfs 确认 PWM 硬件能出波（示波器看），再写驱动。

**追问·"PWM 的极性和占空比有什么坑"**：
- **占空比不能大于周期** —— 驱动程序一般会检查并返回 `-EINVAL`，但有些老代码不检查，会出诡异波形
- **极性反转（`PWM_POLARITY_INVERSED`）** 表示"占空比描述的是低电平时间" —— 对某些需要"低有效"的驱动电路（比如某些背光）很关键
- **改周期/占空比时最好一次性 `pwm_apply_state`** —— 分两步改（先改周期再改占空比）中间会出现一个瞬时的不正确波形，对电机/背光可能可见

---

## 十三、配套机制（GPIO / pinctrl / regmap / DMA）

### 13.1 GPIO 与 pinctrl

**整体**：**pinctrl 管"引脚复用和电气属性"，GPIO 管"读写电平"**。两者是上下游关系 —— **先 pinctrl 把引脚配成 GPIO 功能，GPIO 才能用**。

**机制·分工**：

```
   pinctrl（引脚控制器）              GPIO（通用输入输出）
   ├─ 复用（MUX）：这个脚当 UART 还是 I2C 还是 GPIO？
   ├─ 电气：上拉/下拉、驱动能力、压摆率
   └─ 由设备树的 pinctrl-0 声明，★ 内核自动应用，驱动一般不用管
                                     ├─ 方向：输入/输出
                                     ├─ 读值 / 写值
                                     └─ 中断（gpiod_to_irq）
```

**细节·设备树里的 pinctrl 写法**：

```dts
/* SoC 的 dtsi 里定义了一堆引脚组 */
&pinctrl {
    mydev_pins: mydev-pins {
        rockchip,pins =
            <1 RK_PA4 RK_FUNC_GPIO &pcfg_pull_up>,   /* 引脚 + 功能 + 电气属性 */
            <1 RK_PB0 RK_FUNC_GPIO &pcfg_pull_down>;
    };
};

/* 设备节点里引用它 —— ★ 名字叫 "default" 的内核会自动应用 */
my_device {
    pinctrl-names = "default", "sleep";
    pinctrl-0 = <&mydev_pins>;          /* default 状态用这组 */
    pinctrl-1 = <&mydev_sleep_pins>;    /* 睡眠状态用这组 */
};
```

**细节·🔴 GPIO 的现代 API（**用 `gpiod_` 前缀的，不要用老的 `gpio_`**）**：

```c
/* 老的（已废弃） */
gpio_request(gpio_num, "name");
gpio_direction_output(gpio_num, 1);
gpio_set_value(gpio_num, 0);
gpio_free(gpio_num);

/* ✅ 新的（推荐） */
struct gpio_desc *gpiod;
gpiod = devm_gpiod_get(&pdev->dev, "reset", GPIOD_OUT_LOW);
/*                                     ↑ 对应设备树里的 reset-gpios */
if (IS_ERR(gpiod))
    return PTR_ERR(gpiod);

gpiod_set_value(gpiod, 1);          /* 置高 */
val = gpiod_get_value(gpiod);       /* 读值 */
/* ★ devm 版不用手动释放 */
```

**细节·为什么从 `gpio_` 换成 `gpiod_`**：
老的 API 用**全局数字**表示 GPIO —— 换个 SoC 号就变了，移植性差。新的用 **`gpio_desc` 描述符**（从设备树按**名字**取），**不依赖具体编号**，这才是设备树时代该有的写法。**面试能说出这个演进的动机，很加分。**

**细节·设备树里的 GPIO 属性命名约定**：

```dts
my_device {
    reset-gpios = <&gpio1 RK_PA4 GPIO_ACTIVE_LOW>;
    /*   ↑                            ↑ 低有效 */
    /*   devm_gpiod_get(dev, "reset", ...) 里的名字就是 reset */
    enable-gpios  = <&gpio1 RK_PB0 GPIO_ACTIVE_HIGH>;
    interrupt-gpios = <&gpio1 RK_PB1 GPIO_ACTIVE_LOW>;
};
```
**约定的后缀是 `-gpios`**（复数），**前面的名字对应 `devm_gpiod_get` 的第二个参数**。`GPIO_ACTIVE_LOW` 表示"逻辑 1 = 物理低电平"，驱动不用关心电平极性 —— **这是很实用的抽象**。

---

### 13.2 regmap（**寄存器访问的统一抽象**）

**整体**：同一个芯片可能通过 I2C / SPI / MMIO 三种方式访问寄存器。**`regmap` 把这些统一成一套 API** —— 驱动代码不关心底层总线。

**细节·🔴 三种初始化**：

```c
/* ① I2C */
struct regmap *map = devm_regmap_init_i2c(client, &my_regmap_config);

/* ② SPI */
struct regmap *map = devm_regmap_init_spi(spi, &my_regmap_config);

/* ③ 内存映射（内核内建外设）*/
struct regmap *map = devm_regmap_init_mmio(&pdev->dev, base, &my_regmap_config);
```

**细节·配置结构**（**关键是寄存器地址/值的位宽**）：

```c
static const struct regmap_config my_regmap_config = {
    .reg_bits   = 8,       /* 寄存器地址 8 位 */
    .val_bits   = 8,       /* 寄存器值 8 位 */
    .max_register = 0x7F,  /* ★ 设了它就能防止越界访问（good practice） */
    .cache_type = REGCACHE_RBTREE,   /* ★ 读缓存：减少总线访问 */
    .volatile_reg = my_volatile_reg, /* 哪些寄存器不缓存（状态寄存器） */
    .readable_reg = my_readable_reg,
    .writeable_reg = my_writeable_reg,
};
```

**细节·🔴 统一后的 API**：

```c
regmap_read(map, reg, &val);
regmap_write(map, reg, val);
regmap_update_bits(map, reg, mask, val);   /* ★ 读-改-写原子完成，最常用 */
regmap_bulk_read(map, reg, buf, count);
regmap_bulk_write(map, reg, buf, count);
```

**`regmap_update_bits` 的价值**：不用自己"先读、改位、再写"（那中间可能有竞态），**一次调用完成，而且内部有锁保护**。**这个函数用得极多。**

**追问·"用 regmap 有什么好处"**：
① **一份驱动代码支持三种总线**（芯片的寄存器逻辑不用改）；
② **自带寄存器缓存**（减少总线访问、省电）；
③ **自带锁**（多线程访问安全）；
④ **`regmap_update_bits` 这类复合操作是原子的**；
⑤ **调试方便** —— `debugfs` 里能 dump 整个寄存器空间（`/sys/kernel/debug/regmap/...`）。
**所以只要芯片是"寄存器式"的，就应该用 regmap。**

---

### 13.3 DMA 引擎（内核里的 DMA）

**整体**：和你在 STM32 上用的 DMA 概念一样，但 Linux 把它抽象成**DMA 引擎框架** —— **驱动不直接操作 DMA 寄存器，而是向 DMA 引擎申请通道、提交传输描述符**。

**细节·🔴 基本流程**：

```c
#include <linux/dmaengine.h>

struct dma_chan *chan;
struct dma_slave_config cfg;
struct dma_async_tx_descriptor *tx;
dma_cookie_t cookie;

/* ① 申请通道（从设备树里按名字找） */
chan = dma_request_chan(&pdev->dev, "rx");     /* 对应 dma-names = "rx" */
if (IS_ERR(chan))
    return PTR_ERR(chan);

/* ② 配置从设备参数（方向、地址、宽度、突发长度） */
memset(&cfg, 0, sizeof(cfg));
cfg.direction    = DMA_DEV_TO_MEM;
cfg.src_addr     = res->start;          /* 外设的 FIFO 地址 */
cfg.src_addr_width = DMA_SLAVE_BUSWIDTH_1_BYTE;
cfg.src_maxburst   = 16;
dmaengine_slave_config(chan, &cfg);

/* ③ 准备传输描述符（scatter-gather 表示内存里的分散缓冲区） */
tx = dmaengine_prep_slave_sg(chan, sgl, sg_len,
                             DMA_DEV_TO_MEM,
                             DMA_PREP_INTERRUPT);
tx->callback = my_dma_done;
tx->callback_param = my_priv;

/* ④ 提交并启动 */
cookie = dmaengine_submit(tx);
dma_async_issue_pending(chan);

/* ⑤ 完成回调里清理 */
dma_unmap_sg(...);
```

**细节·🔴 和 STM32 上裸机 DMA 的区别**：

| | STM32 裸机 | Linux DMA 引擎 |
|---|---|---|
| 配置 | 直接写 DMA 寄存器 | 通过 `dmaengine_slave_config` |
| 缓冲区 | 物理地址直接给 | **要 `dma_map_sg` 做地址映射**（有 IOMMU/缓存一致性要处理） |
| 完成通知 | 中断标志 | **回调函数** |
| 通道管理 | 自己选 | 框架分配（多驱动共享） |

**关键难点是缓存一致性**：CPU 写内存后 DMA 读，或者 DMA 写内存后 CPU 读 —— **中间要做 `dma_sync_single_for_cpu/device`**，否则可能读到 cache 里的旧数据。**这是 Linux DMA 编程最容易错的地方**（裸机上没这个问题，因为没有 cache 一致性管理）。

**追问·"什么时候该用 DMA"**：
**数据量大、且不需要 CPU 参与处理每个字节**的场景。判断标准：**传输开销 > CPU 搬运开销**。所以小块数据用 DMA 反而更慢（配置开销大）。这和你在 STM32 上判断"ADC 要不要用 DMA"是同一个逻辑。

---

## 十四、内核启动与 probe 时机

**整体**：搞清楚"**驱动什么时候被加载、probe 什么时候跑**"，对调试"驱动没生效"这类问题是必需的。

**机制·完整启动链**：

```
① Bootloader（U-Boot）
   · 初始化 DDR、串口
   · 把【内核镜像】和【设备树 dtb】加载到内存
   · 设置启动参数（bootargs）→ 跳转到内核

② 内核解压与早期初始化
   · 解压、建立页表、开启 MMU
   · 解析【设备树】→ 得到内存布局、命令行、CPU 信息

③ 内核子系统初始化（按 initcall 等级顺序）
   各等级用宏标记，★ 等级决定顺序：
     early_initcall → core_initcall → postcore → arch → subsys
     → fs → device → late_initcall
   · ★ 设备树展开成 platform_device 发生在【arch/subsys 阶段】
   · ★ 内核内建的 platform_driver 注册在 device_initcall 阶段
   → 所以：设备先创建，驱动后注册 → 触发 probe

④ 挂载根文件系统 → 启动 init（systemd/busybox init）

⑤ 用户态启动脚本里加载模块（modprobe）
   · udev 根据 modules.alias 自动加载设备对应的驱动
   · ★ 这就是为什么 MODULE_DEVICE_TABLE 很重要
```

**细节·🔴 "驱动没生效"的排查顺序**（**这个流程很实用**）：

```
驱动没生效？
   │
   ├─ ① 模块加载了吗？
   │     lsmod | grep mydev
   │     dmesg | tail          ← 看加载时的报错
   │
   ├─ ② 设备树节点存在吗？（设备没被创建的话驱动永远不会 probe）
   │     ls /proc/device-tree/... | grep mydev
   │     ★ 不在的话：dtb 没更新 / status 不是 okay / 节点位置不对
   │
   ├─ ③ 匹配上了吗？（★ 最常见的问题）
   │     ls /sys/bus/platform/drivers/my-driver/    ← 看有没有设备挂上去
   │     ★ 空的说明没匹配上：
   │        · compatible 字符串和 of_match_table 不一致（大小写/拼写！）
   │        · 缺 MODULE_DEVICE_TABLE
   │
   ├─ ④ probe 跑了吗？
   │     dmesg | grep mydev      ← 看你的 pr_info 有没有打印
   │     ★ 跑了但报错：看返回的 errno
   │
   └─ ⑤ probe 跑了但设备节点没有？
         ls /dev/mydev
         ★ 没有：udev/mdev 没跑，或者 class/device_create 失败
```

**追问·"内核内建驱动（built-in）和模块（ko）的加载顺序不同吗"**：
**内建的**在 `device_initcall` 阶段注册（启动早期），**模块的**在用户态 `insmod`/`modprobe` 时才注册。两者都遵循"**设备先有、驱动注册时遍历设备匹配**"或"**驱动先有、设备创建时遍历驱动匹配**"这个双向机制 —— **所以顺序不影响最终能不能匹配上，只影响 probe 的时机**。
**但有一个例外**：如果驱动是**其他驱动的依赖**（比如你的驱动要调用另一个模块导出的符号），那加载顺序就有影响了。

---

## 十五、驱动调试手段

**整体**：驱动调试**没法用 gdb 单步**（内核崩了整机就完了），所以主要靠**日志 + 观察内核状态**。

**细节·🔴 六个必会的观察点**：

| 手段 | 命令 | 看什么 |
|---|---|---|
| **内核日志** | `dmesg -w` / `dmesg \| tail` | 最重要。`pr_info`/`pr_err` 的输出、驱动报错 |
| **设备注册了吗** | `cat /proc/devices` | 主设备号 → 驱动名 |
| **中断进来了吗** | `cat /proc/interrupts` | **中断计数涨不涨**（不涨=中断没接上） |
| **设备被驱动认领了吗** | `ls /sys/bus/platform/devices/` 和 `ls /sys/bus/platform/drivers/xxx/` | 设备有没有挂到驱动的目录下 |
| **设备树** | `ls /proc/device-tree/` | 节点和属性在不在、值对不对 |
| **模块信息** | `lsmod` / `modinfo xxx.ko` | 依赖、参数、别名 |

**细节·🔴 `printk` 的等级与动态调试**：

```c
/* 等级（数字越小越严重）*/
pr_emerg()   /* 0 系统不可用 */
pr_alert()   /* 1 */
pr_crit()    /* 2 */
pr_err()     /* 3  ★ 常用 */
pr_warn()    /* 4  ★ 常用 */
pr_notice()  /* 5 */
pr_info()    /* 6  ★ 常用 */
pr_debug()   /* 7  ★ 默认不输出，要开动态调试 */
```
```bash
# 看/改控制台日志等级（只有等级 < 这个值才打到控制台）
cat /proc/sys/kernel/printk
echo 8 > /proc/sys/kernel/printk      # 全打出来

# ★ 动态调试：不用重新编译就能打开某个文件的 pr_debug
echo -n "file mydev.c +p" > /sys/kernel/debug/dynamic_debug/control
```

**`pr_debug` 的价值**：**平时不输出（零开销），现场出问题时动态打开** —— 这比到处写 `pr_info` 再重新编译要高明得多。

**细节·oops / panic 怎么读**：

```
[  123.456789] Unable to handle kernel NULL pointer dereference at virtual address 0000000000000010
[  123.456790] pgd = ffffffc0...
[  123.456791] [0000000000000010] *pgd=..., *pte=0000000000000000
[  123.456792] Internal error: Oops: 96000006 [#1] PREEMPT SMP
                                 ↑ 错误码
[  123.456793] CPU: 0 PID: 123 Comm: myapp Not tainted 5.10.0 #1
[  123.456794] Hardware name: My Board (DT)
[  123.456795] PC is at my_driver_probe+0x2c/0x100 [mydev]   ★ 出错位置！
[  123.456796] LR is at ...
[  123.456797] Call trace:                                    ★ 调用栈
[  123.456798]  my_driver_probe+0x2c/0x100 [mydev]
[  123.456799]  platform_drv_probe+0x54/0xa8
```

**看三个地方就够了**：
1. **报错类型**（NULL 解引用 / 非法地址 / 除零）
2. **`PC is at`** —— 出错函数和偏移（**`+0x2c` 这个偏移能对应到源码行**，用 `gdb` 或 `addr2line` 反查）
3. **`Call trace`** —— 调用链，能看出是怎么走到这的

**追问·"怎么在没有 JTAG 的情况下调试驱动"**：
① **`printk` 打点**（最土最有效）；② **`/sys/kernel/debug/` 里的各种接口**（regmap dump、clk summary、gpio 状态）；③ **`trace_printk` + ftrace**（比 printk 开销小得多，适合高频路径）；④ **`kgdb`**（内核级的 gdb 远程调试，需要串口/网口支持）；⑤ **`kprobe`/`kretprobe`**（动态插桩，不用改代码就能看函数入参出参）。**实战里 ①+② 用了九成。**

---

## 十六、交叉编译内核模块

**整体**：内核模块**必须用它要运行的那个内核的源码树编译** —— 因为模块要链接内核符号、而且 ABI 和配置必须完全一致。

**细节·🔴 标准 Makefile**：

```makefile
# 内核模块的 Makefile
obj-m += mydev.o
mydev-objs := mydev_main.o mydev_ops.o     # 多文件组成一个模块

# ★ 两个关键变量：
KDIR    := /path/to/kernel/source            # 目标内核源码树
ARCH    := arm64                             # 目标架构
CROSS   := aarch64-linux-gnu-                # 交叉编译前缀

PWD     := $(shell pwd)

all:
	$(MAKE) -C $(KDIR) M=$(PWD) \
	        ARCH=$(ARCH) CROSS_COMPILE=$(CROSS) modules

clean:
	$(MAKE) -C $(KDIR) M=$(PWD) \
	        ARCH=$(ARCH) CROSS_COMPILE=$(CROSS) clean
```

**细节·🔴 编译前必须准备的两件事**：
```bash
# ① 内核源码要【配置并编译过一次】（否则没有 Module.symvers / include/generated）
cd $KDIR
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- xxx_defconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- modules_prepare   # 或 make -j8

# ② 确认目标内核的版本和你的模块一致
cat $KDIR/include/config/kernel.release
uname -r          # 目标机上
```

**细节·🔴 验证产物**：

```bash
file mydev.ko              # ★ 看架构对不对（应该是 ARM aarch64）
modinfo mydev.ko           # 看 vermagic / license / 依赖
```

`vermagic` 不匹配的症状：
```
insmod: ERROR: could not insert module mydev.ko: Invalid module format
dmesg: mydev: version magic '5.10.0 SMP mod_unload aarch64' 
       should be '5.10.110 SMP mod_unload aarch64'
                                    ↑ 版本号不一致
```

**追问·"能不能在目标机上直接编译"**：
**可以，而且更省事**（如果目标机有 gcc 和内核头文件）。嵌入式产品上常见做法：
① **开发阶段**：交叉编译（开发机性能好）；
② **产线/调试**：把工具链拷到目标机上直接编（避免版本不一致）。
**关键是"一定要用同一个内核源码树"** —— 这是唯一的硬要求。

---

## 十七、面试高频问答

> 这一节是"浓缩版"，**每个问题一句话答**。前十六节是展开，这里是速查。

| # | 问题 | 一句话答案 |
|---|---|---|
| 1 | 驱动模型三要素是什么 | **bus / device / driver** —— 总线负责匹配，设备描述硬件，驱动写操作逻辑 |
| 2 | probe 什么时候被调用 | **驱动注册时遍历已有设备**，或**设备注册时遍历已有驱动** —— 匹配上就调（双向） |
| 3 | 匹配靠什么 | 优先 **设备树 `compatible`**，其次 ACPI、`id_table`、名字 |
| 4 | 为什么需要设备树 | **ARM 硬件不可枚举** —— 把硬件描述从内核代码搬到 bootloader 传的数据，一个内核启动多个板子 |
| 5 | `.dts`/`.dtsi`/`.dtb` 关系 | `.dtsi` 是可 include 的 SoC 公共部分，`.dts` 是板级文件，`dtc` 编译成 `.dtb` |
| 6 | 怎么确认设备树生效了 | `ls /proc/device-tree/`（**运行时的真实设备树**，和 `.dts` 不一致说明 dtb 没更新） |
| 7 | 为什么需要 platform 总线 | SoC 内建外设**没有"插入"这个动作**、无法枚举，得靠设备树声明 |
| 8 | `devm_*` 是什么 | 资源**绑定到 device 生命周期**，驱动卸载时自动逆序释放 —— 大幅简化错误处理 |
| 9 | 字符设备注册的现代做法 | `alloc_chrdev_region` + `cdev_init` + `cdev_add` + `class_create` + `device_create` |
| 10 | 设备节点是怎么自动出现的 | 驱动 `device_create` → 内核发 **uevent** → **udev/mdev** 收到并 `mknod` |
| 11 | 为什么必须 `copy_to_user` | 用户和内核**地址空间不同**、要做**合法性检查**、架构差异；这是唯一合法通道 |
| 12 | `read` 返回 0 是什么意思 | **EOF，不是错误**（错误要返回负的 errno） |
| 13 | `file_operations.owner` 为什么必须填 | **`THIS_MODULE`** —— 防止模块在使用中被卸载（open 期间模块引用计数不为 0） |
| 14 | 中断里能睡眠吗 | **绝对不能** —— 所以中断里只能用 `spinlock`，不能用 `mutex`；不能用 `copy_to_user` |
| 15 | `spin_lock_irqsave` 为什么要关中断 | 自旋锁是忙等 —— 如果中断进来抢同一把锁会**永远自旋**（持锁的进程跑不到） |
| 16 | 上半部下半部分工 | 上半部：清标志、读数据、投递；下半部：处理、可能睡眠的活 |
| 17 | 下半部选哪个 | **要睡眠** → workqueue 或 `request_threaded_irq`；**不睡眠且短** → tasklet。**现代推荐线程化中断** |
| 18 | `IRQF_ONESHOT` 干什么 | 线程化中断里保证**下半部跑完前不重新使能中断**（避免丢数据/重复唤醒） |
| 19 | 四种用户态接口怎么选 | **调试用 sysfs、正式接口用 ioctl、纯调试用 debugfs、别用 procfs** |
| 20 | sysfs 的 `store` 返回什么 | **返回 `count`**（不是 0/1），否则 `echo` 报错；且**必须做参数校验** |
| 21 | Input 子系统为什么好 | 自动建 `/dev/input/eventX`、用户态有标准做法（libinput/evtest/Qt 插件）、自动同步 |
| 22 | `input_sync` 为什么必须调 | 一次物理动作产生多个事件，**它是分隔符** —— 不调用户态会读到半套状态 |
| 23 | SPI 里 `reg` 是什么 | **片选编号**（不是地址！） |
| 24 | `spi_transfer` 和 `spi_message` | message 含多个 transfer，**同一 message 内片选不释放** —— 需要连续时序就放一个 message |
| 25 | I2C 里 `reg` 是什么 | **7 位设备地址**（手册常给 8 位形式，要右移一位） |
| 26 | PWM 的周期占空比单位 | **纳秒（ns）**，不是 Hz/百分比 |
| 27 | PWM 新老 API | 新：`pwm_apply_state` + `struct pwm_state`；老：`pwm_config`/`pwm_enable`（已废弃） |
| 28 | pinctrl 和 GPIO 的关系 | pinctrl 管**引脚复用和电气属性**（设备树声明，内核自动应用）；GPIO 管**读写电平和中断** |
| 29 | `gpiod_` 为什么替代 `gpio_` | 老的用**全局编号**（移植性差），新的用**描述符**（从设备树按名字取） |
| 30 | regmap 的价值 | 一份驱动支持 I2C/SPI/MMIO；自带缓存、锁、原子 `update_bits`、debugfs dump |
| 31 | Linux DMA 最难的是什么 | **缓存一致性** —— CPU 和 DMA 共享内存时要 `dma_sync_single_for_cpu/device` |
| 32 | 驱动没生效怎么查 | 按顺序：`lsmod` → `/proc/device-tree` → `ls /sys/bus/.../drivers/xxx/` → `dmesg` |
| 33 | `/proc/interrupts` 看什么 | **中断计数涨不涨** —— 不涨说明中断没接上 |
| 34 | `pr_debug` 为什么好 | 默认不输出（**零开销**），现场出问题用**动态调试**打开，不用重编译 |
| 35 | 模块必须有的宏 | **`MODULE_LICENSE`**（否则污染内核 + 用不了 GPL-only 符号） |
| 36 | `MODULE_DEVICE_TABLE` 的作用 | 把匹配表导出，`depmod` 生成 `modules.alias` —— **设备出现时 udev 能自动 modprobe** |
| 37 | 模块编译的硬要求 | **必须用目标内核的源码树** —— 否则 vermagic 不匹配，`Invalid module format` |

---

## 附：和项目的对应关系 + 边界口径

**✅ 可以放心讲的（框架层面）**：

| 项目里的东西 | 你能讲到什么程度 |
|---|---|
| `/dev/misc_sandblasting` | **它是 misc 字符设备**（主设备号固定 10）、`misc_register` 自动建节点、**阻塞 read 靠等待队列实现** |
| 应用侧的 read 路径 | 从 `open` → VFS 查 cdev → `file_operations` → 驱动的 `read` → `wake_up_interruptible` → 用户态拿到数据 |
| 为什么用字符设备 | **简单单向数据流** —— 设备有数据就 read，不需要协议协商。ioctl 适合配置、netlink 适合结构化双向 |
| 驱动模型 / 设备树 / platform | **能画出三层关系、能说清匹配和 probe 时机** |

**⚠️ 边界口径（被问到要如实说）**：

> "**驱动的本体是团队提供的，我写的是应用层基于 read 接口的接收逻辑** ——
> 事件接入（`QSocketNotifier`）、长度前缀拆帧、脏数据重同步、异常处理这些是我的。
> 内核驱动本身**我没有从零写过**，但因为我天天在用这个驱动，
> 它的注册方式（misc 设备）、read 的实现机制（等待队列 + `wake_up`）、
> 以及从用户态 open 到驱动 read 的完整路径我是清楚的。
> 简历上写的'了解'也是这个程度 —— **框架能讲清、代码能写骨架，复杂驱动没做过**。"

**这个口径的好处**：既不越界（不谎称写过驱动），**又展示了"我理解自己用的东西在整个系统里的位置"** —— 后者对做应用层的人来说**恰恰是最该具备的**。

---

**最后更新**：2026-09-24
**配套**：`基础问答-项目2.md`（Linux 应用/Qt/并发/网络）、`09-项目二-TCP与Socket详解.md`、`08-总线全解与字节序.md`（SPI/I2C 时序）、`10-真实项目场景还原.md`（misc 设备那一节）
**口径**：简历写的是"**了解**" —— 框架讲清、代码写骨架、**不越界**。
