# Knowledge

嵌入式 Linux / 驱动开发个人知识库，围绕 **C 语言 → Linux → 内核驱动** 的面试与工程主线整理的学习笔记。

## 目录

| 文件 | 大小 | 内容概览 |
|------|------|----------|
| [c基础.md](./c基础.md) | 25 KB | C 语言基础与面试题：数据类型、volatile、指针（函数指针 / 数组指针 / 指针数组等）、const、浮点数存储；链表、常见排序算法（选择/插入/希尔/冒泡/快速）、hash 算法（构造、冲突解决、常用函数） |
| [linux.md](./linux.md) | 46 KB | Linux 系统使用与开发：关键命令（关机、查看文本、mount、dmesg、grep、find、lsusb、lsof）；进程间通讯、内存申请、gcc 编译过程、文件系统、进程状态、系统调用、fork、僵尸进程、/proc 目录等软件开发知识点；驱动开发、makefile、shell 相关 |
| [嵌入式Linux驱动知识点.md](./嵌入式Linux驱动知识点.md) | 96 KB | 内核驱动**源码级**知识点（面试主打文档）：整体分层、内核模块、bus/device/driver 设备模型、设备树、platform 总线、字符设备、并发与竞态、中断、用户态接口，以及 Input / SPI / I2C / PWM 子系统、配套机制（GPIO/pinctrl/regmap/DMA）、驱动调试、交叉编译、面试高频问答，共 17 章 |
| [tcpip协议栈.md](./tcpip协议栈.md) | 31 KB | TCP/IP 网络：分层模型（OSI 与 Linux 模型对照）、以太网协议、ARP 地址解析、TCP 协议等 |
| [shell.md](./shell.md) | 11 KB | Shell 脚本：shell 种类、基本语法（变量、if/else、条件测试）、常用命令 |
| [makefile.md](./makefile.md) | 7 KB | Makefile 语法：简单示例、赋值方式等 |
| [git.md](./git.md) | 2 KB | Git 常用命令操作笔记（add / rm / commit / push / branch 等） |
| [uboot.md](./uboot.md) | 1 KB | U-Boot 源码目录结构及各目录作用、启动流程（stage1 / stage2） |

## 建议阅读顺序

按从基础到驱动的递进关系：

1. **C 基础**：`c基础.md`（语言功底，面试必考）
2. **系统操作**：`linux.md`（命令 + 进程/文件系统）、`shell.md`、`makefile.md`
3. **网络协议**：`tcpip协议栈.md`
4. **系统启动**：`uboot.md`
5. **核心面试文档**：`嵌入式Linux驱动知识点.md`（含设备树、platform 总线、各子系统，可与简历"专业技能 6"对应）
6. **版本管理**：`git.md`

## 说明

- `test.txt` 为测试文件，可忽略。
- 各文档为长期维护的个人笔记，新增知识点时保持现有 Markdown 层级结构即可。
