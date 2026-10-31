# Qt

Qt是一个功能十分强大的**跨平台**的**用户界面框架（GUI）**，主要用于前端开发，且主要用于B端开发。在嵌入式领域中，主要用于上位机开发。

Qt由于其跨平台的特性，使其成为了最广泛应用的GUI之一。其主要使用C++进行开发，也支持 `python` 、 `qml` 等多种开发形式。

## Qt Creator

`Qt Creator` 是一个Qt专属的 `IDE` ，专门用于Qt应用程序开发，其除了具备基础IDE需要具备的编辑器、编译器、运行套件外，还集成了 `Qt Designer` 、 `Qt Assistant` 、 `Qt Linguist` 等工具，可以非常方便的完成Qt开发。

### 创建项目

🌟尽量不要在开发路径中包含中文

文件→新建项目（`New Project`）然后选择 `Application(Qt)` → `Qt Widgets Application` （第一个的第三个）

#### 构建系统

`Qt` 中支持多种构建工具，一般在 `Qt6` 中主要使用 CMake 和原生的 `qmake` ，之后课件中统一使用 `qmake` 。使用 `qmake` 创建好项目后，会有一个 `.pro` 文件来管理项目。如果是 `CMake` ，则为一个 `.txt` 。

#### Details

这里选择的是窗体的默认风格，目前先选择 `QWigdets` 作为基础风格，其他的之后说。

#### 项目目录框架

```c
test 		//项目文件夹
├ test.pro 	//项目管理文件
├ 头文件
│ 	└ widget.h 		//类声明的头文件，需要用户修改
├ 源文件
│ 	├ main.cpp 		//主函数，通常不修改
│ 	└ widget.cpp 	//类实现，需要用户修改
└ 界面文件
	└ widget.ui 	//界面文件，双击打开Qt Designer进行窗口的可视化编辑
```

## Qt中的控件

Qt中提供的控件大部分都和其他GUI工具提供的控件没有区别，所以掌握了任意一种GUI工具都可以进行类比学习。

### 布局控件（Layouts）

布局控件负责盛放其他控件并保证这些控件可以直接对齐。

布局控件一共有四种：垂直布局、水平布局、网格布局、表格布局

使用布局控件时，可以先拖出布局，然后再往里面放控件；也可以先拖出控件，然后用上边栏的布局按钮将选中的控件直接整理成布局。

一旦控件进入布局，用户就无法再对这些控件进行微调了，但是可以通过布局的 `stretch` 属性来调整各个部分的占比，如果仍有微调需求，那么就只能分拆布局了（上边栏就有分拆布局的按钮），然后再对控件的大小和位置做微调。微调的方法可以是直接用鼠标拖动或拖放，也可以在右下角的属性编辑器中修改，也可以使用快捷键：方向键可以以网格为单位，对控件做位置上的微调； `Ctrl + 方向键` 可以以像素为单位对位置做微调； `Shift + 方向键` 可以以网格为单位，对控件做大小上的微调； `Ctrl + Shift + 方向键` 可以以像素为单位，对控件做大小上的微调。

🌟窗体 `Widget` （和一些其他的控件）也可以设置为布局，这样做的话，窗体拖放时，控件的大小和位置也会跟着变。

🌟控件在进入布局时，其表现根据其水平策略和垂直策略的不同，表现也会有所不同，如果其策略不符合用户的要求，通常可以通过修改水平策略和垂直策略实现。如如果希望按钮在网格中的高度也可以随着网格的大小变动而变动，则可以将垂直策略设置为 `MinimumExpanding` ，从而达成这一效果。

### 占位器（Spaces）

占位器没有任何作用，它只用在布局中，用来调整控件的位置和占比，在布局外，其没有用途。

占位器也可以通过 `stretch` 属性来调整所占比例。

### 按钮控件（Buttons）

按钮控件一共有三种常用的：

`push button` ：一般按钮

`check box` ：复选框

`radio button` ：单选按钮

#### QWidget中的属性和函数

`enable` ：控件是否使能（对应函数 `isEnable` 、 `setEnable` ）

`visible` ：控件是否可见（对应函数 `isVisible` 、 `setVisible` ），这个属性在属性编辑器中找不到

**常用函数**

`pos` ：获取控件位置，用 `x` 和 `y` 两个函数就可以获取控件的坐标（控件左上角相对界面左上角的相对坐标）

`size` ：获取控件大小，用 `width` 和 `height` 两个函数可以获取控件的宽度和高度

`move` ：传入横纵坐标，将控件移动到指定位置

#### QRadioButton

**关键属性和函数**

**QAbstractButton中的属性（和其他按钮控件共享的属性）**

`autoExclusive` ：是否和其他单选按钮互斥（对应函数 `isAutoExclusive` 、 `setAutoExclusive` ）

`checked` ：是否选中（对应函数 `isChecked` 、 `setChecked` ）

**关键信号**

`toggled` ：当按钮的选中状态发生改变时触发（无论是选中还是去选中都会触发，参数表示当前的选中状态）

#### QCheckBox

**关键属性和函数**

`tristate` ：设置按钮的三态属性（对应函数 `isTristate` 、 `setTristate` ）

`checkState` ：设置按钮的三态状态（对应函数 `checkState` 、 `setCheckState` ），这个属性在属性编辑器中找不到

**关键信号**

`checkStateChanged` ：当按钮的选中状态发生改变时触发，如果设置了三态，则三态发生变化时都会触发。三态基于一个枚举：

```c++
enum CheckState {
	Unchecked, 			//未选中
	PartiallyChecked, 	//半选中
	Checked 			//选中
};
```

`stateChanged` ：Qt6.7之前的老版信号，触发时机和 `checkStateChanged` 一致，参数是整型。

🌟不使用三态的复选框也可以使用 `toggled` 信号

##### 三态复选框的demo

```c++
void Widget::on_checkBox_1_checkStateChanged(const Qt::CheckState &arg1) {
    switch (arg1) {
    case Qt::Unchecked:
        ui->checkBox_1_1->setChecked(false);
        ui->checkBox_1_2->setChecked(false);
        break;
    case Qt::Checked:
        ui->checkBox_1_1->setChecked(true);
        ui->checkBox_1_2->setChecked(true);
        break;
    case Qt::PartiallyChecked:
        break;
    }
}

void Widget::on_checkBox_1_1_toggled(bool checked) {
    if (checked) {
        if (ui->checkBox_1_2->isChecked()) {
            ui->checkBox_1->setCheckState(Qt::Checked);
        } else {
            ui->checkBox_1->setCheckState(Qt::PartiallyChecked);
        }
    } else {
        if (ui->checkBox_1_2->isChecked()) {
            ui->checkBox_1->setCheckState(Qt::PartiallyChecked);
        } else {
            ui->checkBox_1->setCheckState(Qt::Unchecked);
        }
    }
}

void Widget::on_checkBox_1_2_toggled(bool checked) {
    if (checked) {
        if (ui->checkBox_1_1->isChecked()) {
            ui->checkBox_1->setCheckState(Qt::Checked);
        } else {
            ui->checkBox_1->setCheckState(Qt::PartiallyChecked);
        }
    } else {
        if (ui->checkBox_1_1->isChecked()) {
            ui->checkBox_1->setCheckState(Qt::PartiallyChecked);
        } else {
            ui->checkBox_1->setCheckState(Qt::Unchecked);
        }
    }
}
```

### View控件和Widget控件

这两类控件使用起来相对复杂，所以在这里不做过多介绍，后续有专门的章节来使用这两类控件。

这两类空间中有三种最为常见的视图：

`list view` ：列表视图

`table view` ：表格视图

`tree view` ：树状视图

不同视图展示数据的方式不同，而 `Widget` 控件和 `View` 控件是一个道理

### 容器控件（Containers）

容器控件用于整理、盛放其他控件，其会建立起一个独立环境（相当于又开了一个子窗体嵌入到了当前窗体中）。所以容器控件会把一个窗体切分成若干个部分，各自为政。

容器控件大部分也有布局属性，可以设置为布局。

部分容器控件可以分页展示内容，如 `tab` 、`widget` ，相当于扩大了当前页面的面积。

### 输入控件（Input Widgets）和输出控件（Display Widgets）

这两类控件主要用户和用户发生直接的数据交互，是最常见的种类，所以大部分控件只要拖出来就知道是干啥的了。故不做特别介绍。

### 对象查看器

这里可以查看每一个控件对象的对象名和类名，其中对象名可以在代码中通过 `ui-> 对象名` 的方式直接调用对应的对象，双击直接就可以修改对象名。而类名通常可以通过提升的手段修改。

在某些对象前会有一个布局的图标，表示这个对象可以设置为布局，例如 `QWidget` 、 `QTabWidget` 等，都有这个功能，如果将该控件设置为了布局，则布局的图标会发生变化。

### 属性编辑器

在 `Qt Designer` 界面，右下角有一个属性编辑器，当你选中某个控件时，就会显示该控件的大部分属性，通过更改这些属性的值就可以更改控件的一些默认的行为。

属性编辑器中的属性列其实就是函数名，通过在其前面加上 `set` 就可以在代码中直接调用对应函数修改对应属性，而直接用属性的名字或者 `is + 属性的名字`（用于 `bool` 类型的），通常就可以获取该属性的值。

除此之外，这里还将属性划分成了各个类中的属性，即可以通过这里查看当前控件的每一个祖先类。

## 信号与槽

信号与槽是Qt的核心机制，Qt中各种控件和库都有可能发射信号。当信号发生时，表示某个事件发生了，此时如果存在和该信号对应的槽函数，则该函数就会被调用。

信号和槽函数间并没有一一对应的关系，即一个信号可以绑定多个槽函数，信号一旦发生，**所有的槽函数都会被调用**。如果将一个槽函数多次绑定到同一个信号上，则信号发生时，该槽函数也会被调用多次。反过来一个槽函数也可以被多个信号绑定，这样无论这些信号中是谁发生了，该函数都会被调用。

🌟槽函数归根结底也只是一个普通的成员函数，其成为槽函数依赖这种“绑定”机制，即只有被绑定到了具体的信号上，他才会在信号发生时就直接调用。

### 槽函数的自动绑定

在为控件所发出的信号绑定槽函数时，可以直接在 `Qt Designer` 界面上右键控件，选择“转到槽”，然后选择具体信号，点击确定即可。此时， `Qt Creator` 会自动生成一个函数的声明和实现，这个函数名的格式为 `on_控件名_信号名` ，如： `on_pushButton_0_clicked` ，同时对应声明也会出现在头文件的 `private slots` 标签后。随后QC会自动跳转到对应函数处，等待用户编写代码。

自动创建的槽函数名其实也暗示了自动绑定的规则，即只要在 `private slots` 标签后声明格式为 `on_控件名_信号名` 的函数，则该函数就会自动绑定到指定控件的制定信号上，所以用户也可以手动定制这样的函数，让其自动绑定。

### 槽函数的手动绑定

手动绑定槽函数需要用到一个非常重要的Qt函数： `connect` 。如下是一个绑定槽函数的例子：

总得来说，这四个参数可以概括为：**谁发的，发的啥，谁接收，咋处理**。

```c
connect(ui->pushButton, 			//信号的发送者
		&QPushButton::clicked, 		//发的什么信号
		this, 						//信号的接收者，通常为this
		&MainWindow::clickedSlot); 	//处理信号的槽函数
```

🌟如果手动绑定了一个已经被自动绑定了的函数，则信号触发时，该函数会被调用两次。

### 自定义信号

Qt允许用户自定义信号，定义时，需要在头文件中插入 `signals` 标签，如：

```c
class MainWindow : public QMainWindow
{
	Q_OBJECT
public:
	MainWindow(QWidget *parent = nullptr);
	~MainWindow();
signals:
	void testSig(); //信号函数
private:
	Ui::MainWindow *ui;
};
```

`testSig` 就是一个信号函数，这个函数**不需要**实现，调用它就相当于发射它。发射时通常还会使用

`emit` 关键字：

```c
emit testSig();
```

自定义信号可以有参数，但槽函数的参数不一定要和信号函数一一对应，如果一一对应，则发射信号时传入的参数会被槽函数获取。

### 案例

简易计算器

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>

QT_BEGIN_NAMESPACE
namespace Ui { class Widget; }
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    Widget(QWidget *parent = nullptr);
    ~Widget();

    enum Sign { ADD, SUB, MUL, DIV };

private slots:
    // 数字
    void on_pushButton_0_clicked();
    void on_pushButton_1_clicked();
    void on_pushButton_2_clicked();
    void on_pushButton_3_clicked();
    void on_pushButton_4_clicked();
    void on_pushButton_5_clicked();
    void on_pushButton_6_clicked();
    void on_pushButton_7_clicked();
    void on_pushButton_8_clicked();
    void on_pushButton_9_clicked();

    // 小数点
    void on_pushButton_dian_clicked();

    // 运算符
    void on_pushButton_ADD_clicked();
    void on_pushButton_SUB_clicked();
    void on_pushButton_MUL_clicked();
    void on_pushButton_DIV_clicked();

    // 功能
    void on_pushButton_back_clicked();
    void on_pushButton_C_clicked();
    void on_pushButton_CE_clicked();
    void on_pushButton_MP_clicked();
    void on_pushButton_EQ_clicked();

private:
    Ui::Widget *ui;

    // 运算数据
    double num1;          // 当前操作数1
    double num2;          // 当前操作数2
    double origNum1;      // 本次运算的原始操作数1（用于显示）
    double lastOperand;   // 上次运算的操作数2（用于连续等于）
    Sign sign;            // 当前运算符
    Sign lastSign;        // 上次运算符（用于连续等于）
    bool signFlag;        // 是否已输入运算符（true表示正在输入第二个数）
    bool eqFlag;          // 是否刚完成计算

    // 小数支持
    bool decimalFlag;     // 当前输入的数字是否包含小数点
    double decimalFactor; // 下一位小数的权重（10, 100, 1000...）

    // 辅助函数
    void updateDisplay();                         // 统一更新两个显示框
    QString getSignChar(Sign s) const;            // 获取运算符字符
    double performOperation(double a, double b, Sign op); // 执行运算
    void performCalculation();                    // 连续运算符时完成上次计算
    void numButtonClicked(int n);                 // 数字处理
    void signButtonClicked(Sign s);               // 运算符处理
    void resetDecimalState();                     // 重置小数状态
    void parseNumberFromFormula(const QString& formula, bool isNum2); // 从公式字符串解析数字并更新对应变量
};

#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"
#include <QDebug>
#include <cmath>
#include <QIcon> // 包含头文件

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , num1(0.0)
    , num2(0.0)
    , origNum1(0.0)
    , lastOperand(0.0)
    , sign(ADD)
    , lastSign(ADD)
    , signFlag(false)
    , eqFlag(false)
    , decimalFlag(false)
    , decimalFactor(10.0)
{
    ui->setupUi(this);
    this->setWindowIcon(QIcon(":/calculator-icon.ico"));
    this->setFixedSize(500, 600);
    updateDisplay();
}

Widget::~Widget()
{
    delete ui;
}

// ==================== 辅助函数 ====================

void Widget::resetDecimalState()
{
    decimalFlag = false;
    decimalFactor = 10.0;
}

void Widget::parseNumberFromFormula(const QString& formula, bool isNum2)
{
    // 解析公式中的数字部分（第一个数或第二个数）
    QString numStr;
    if (isNum2) {
        int pos = formula.lastIndexOf(' ');
        if (pos != -1) {
            numStr = formula.mid(pos + 1);
        } else {
            numStr = "0";
        }
    } else {
        // 第一个数可能包含符号（如 '-'），但 formula 以数字开头
        numStr = formula;
        // 如果公式包含运算符，则截取运算符前的部分
        int opPos = formula.indexOf(' ');
        if (opPos != -1) {
            numStr = formula.left(opPos);
        }
    }

    bool ok;
    double val = numStr.toDouble(&ok);
    if (!ok) val = 0.0;

    // 更新对应的变量
    if (isNum2) {
        num2 = val;
    } else {
        num1 = val;
        origNum1 = val; // 同步原始值
    }

    // 检测是否包含小数点，更新小数状态
    if (numStr.contains('.')) {
        decimalFlag = true;
        // 计算小数位数，设置 decimalFactor
        int dotPos = numStr.indexOf('.');
        int digitsAfterDot = numStr.length() - dotPos - 1;
        decimalFactor = std::pow(10.0, digitsAfterDot + 1); // 下一位的权重
    } else {
        resetDecimalState();
    }
}

void Widget::updateDisplay()
{
    QString formula;
    QString resultText;

    if (eqFlag) {
        // 等于状态：显示完整等式
        QString op = getSignChar(lastSign);
        formula = QString::number(origNum1) + " " + op + " " +
                  QString::number(lastOperand) + " =";
        resultText = QString::number(num1);
    } else if (signFlag) {
        // 输入运算符后，正在输入第二个数
        QString op = getSignChar(sign);
        // 使用 num1 显示第一个数，第二个数显示当前输入的字符串（通过 num2 转换，但可能丢失末尾零）
        // 为了保留末尾零，我们可以单独存储字符串，但为了简洁，这里使用数值显示
        formula = QString::number(num1) + " " + op + " " + QString::number(num2);
        resultText = QString::number(num2);
    } else {
        // 只输入了第一个数（或初始）
        formula = QString::number(num1);
        resultText = QString::number(num1);
    }

    ui->LineEdit_1->setText(formula);
    ui->LineEdit->setText(resultText);
}

QString Widget::getSignChar(Sign s) const
{
    switch (s) {
    case ADD: return "+";
    case SUB: return "-";
    case MUL: return "×";
    case DIV: return "÷";
    default: return "";
    }
}

double Widget::performOperation(double a, double b, Sign op)
{
    switch (op) {
    case ADD: return a + b;
    case SUB: return a - b;
    case MUL: return a * b;
    case DIV:
        if (b == 0.0) {
            ui->LineEdit->setText("Error");
            return 0.0;
        }
        return a / b;
    default: return 0.0;
    }
}

// 在 eqFlag=true 时，完成上次运算，并将结果作为新运算的第一个数
void Widget::performCalculation()
{
    if (!eqFlag) return;
    double result = performOperation(num1, lastOperand, lastSign);
    num1 = result;
    origNum1 = num1;   // 结果作为新运算的起始操作数
    eqFlag = false;
    resetDecimalState(); // 重置小数状态
}

// ==================== 数字按键 ====================

void Widget::numButtonClicked(int n)
{
    if (eqFlag) {
        // 刚计算完，按数字重新开始
        on_pushButton_C_clicked();
    }

    if (signFlag) {
        // 正在输入第二个数
        if (decimalFlag) {
            num2 += n / decimalFactor;
            decimalFactor *= 10.0;
        } else {
            num2 = num2 * 10 + n;
        }
        updateDisplay();
    } else {
        // 输入第一个数
        if (decimalFlag) {
            num1 += n / decimalFactor;
            decimalFactor *= 10.0;
        } else {
            num1 = num1 * 10 + n;
        }
        origNum1 = num1;   // 同步更新原始操作数
        updateDisplay();
    }
}

void Widget::on_pushButton_0_clicked(){ numButtonClicked(0); }
void Widget::on_pushButton_1_clicked(){ numButtonClicked(1); }
void Widget::on_pushButton_2_clicked(){ numButtonClicked(2); }
void Widget::on_pushButton_3_clicked(){ numButtonClicked(3); }
void Widget::on_pushButton_4_clicked(){ numButtonClicked(4); }
void Widget::on_pushButton_5_clicked(){ numButtonClicked(5); }
void Widget::on_pushButton_6_clicked(){ numButtonClicked(6); }
void Widget::on_pushButton_7_clicked(){ numButtonClicked(7); }
void Widget::on_pushButton_8_clicked(){ numButtonClicked(8); }
void Widget::on_pushButton_9_clicked(){ numButtonClicked(9); }

// ==================== 小数点 ====================

void Widget::on_pushButton_dian_clicked()
{
    if (eqFlag) {
        // 刚计算完，按小数点开始输入小数（相当于重新开始）
        on_pushButton_C_clicked();
    }

    if (decimalFlag) {
        // 已有小数点，忽略
        return;
    }

    decimalFlag = true;
    decimalFactor = 10.0;

    // 如果当前没有数字，自动补0
    if (signFlag) {
        if (num2 == 0.0) {
            // 显示"0."，但我们的数值还是0，显示会变成"0"，但加小数点后显示应为"0."
            // 因此我们需要在显示中体现小数点，可以使用显示字符串，但为了保持简单，我们允许显示为"0"
            // 但用户期望看到"0."，所以我们可以在显示时特殊处理。
            // 更简单的方法：使用字符串输入方式。鉴于代码已定，我们保持数值，显示会丢失末尾小数点。
            // 我们可以临时在updateDisplay中判断，但这会复杂化。
            // 一个折中：在显示时，如果decimalFlag为true且数字为0，则显示为"0."。
            // 在updateDisplay中，如果signFlag且num2==0且decimalFlag，则显示"0."。
        }
    } else {
        if (num1 == 0.0) {
            // 类似处理
        }
    }
    updateDisplay();
}

// ==================== 运算符 ====================

void Widget::signButtonClicked(Sign s)
{
    if (eqFlag) {
        // 等号后按运算符：以当前结果作为新运算的第一个数，直接开始
        eqFlag = false;
        origNum1 = num1;          // 更新为当前结果
        // 不要调用 performCalculation()
    } else if (signFlag) {
        // 已有运算符且正在输入第二个数：先完成本次运算
        double result = performOperation(num1, num2, sign);
        num1 = result;
        origNum1 = num1;
        lastSign = sign;
        lastOperand = num2;
    }

    // 设置新运算符
    sign = s;
    signFlag = true;
    num2 = 0.0;
    resetDecimalState();
    updateDisplay();
}

void Widget::on_pushButton_ADD_clicked(){ signButtonClicked(ADD); }
void Widget::on_pushButton_SUB_clicked(){ signButtonClicked(SUB); }
void Widget::on_pushButton_MUL_clicked(){ signButtonClicked(MUL); }
void Widget::on_pushButton_DIV_clicked(){ signButtonClicked(DIV); }

// ==================== 功能键 ====================

// 退格
void Widget::on_pushButton_back_clicked()
{
    if (eqFlag) {
        on_pushButton_C_clicked();
        return;
    }

    QString formula = ui->LineEdit_1->text();
    if (formula.isEmpty() || formula == "0") return;

    formula.chop(1);
    if (formula.isEmpty() || formula == "-") {
        // 清空变为0
        if (signFlag) {
            num2 = 0.0;
        } else {
            num1 = 0.0;
            origNum1 = 0.0;
        }
        resetDecimalState();
        updateDisplay();
        return;
    }

    // 重新解析数字并更新变量
    if (signFlag) {
        parseNumberFromFormula(formula, true);
    } else {
        parseNumberFromFormula(formula, false);
    }
    ui->LineEdit_1->setText(formula);
    updateDisplay();
}

// 全部清除
void Widget::on_pushButton_C_clicked()
{
    num1 = 0.0;
    num2 = 0.0;
    origNum1 = 0.0;
    lastOperand = 0.0;
    signFlag = false;
    eqFlag = false;
    sign = ADD;
    lastSign = ADD;
    resetDecimalState();
    updateDisplay();
}

// 清除当前输入
void Widget::on_pushButton_CE_clicked()
{
    if (eqFlag) {
        on_pushButton_C_clicked();
        return;
    }

    if (signFlag) {
        num2 = 0.0;
    } else {
        num1 = 0.0;
        origNum1 = 0.0;
    }
    resetDecimalState();
    updateDisplay();
}

// 正负号切换
void Widget::on_pushButton_MP_clicked()
{
    if (eqFlag) {
        num1 = -num1;
        origNum1 = num1;
        updateDisplay();
        return;
    }

    if (signFlag) {
        num2 = -num2;
    } else {
        num1 = -num1;
        origNum1 = num1;
    }
    updateDisplay();
}

// 等于
void Widget::on_pushButton_EQ_clicked()
{
    if (eqFlag) {
        // 连续等于
        double result = performOperation(num1, lastOperand, lastSign);
        origNum1 = num1;     // 保存本次运算的第一个操作数（即旧结果）
        num1 = result;
        updateDisplay();
        return;
    }

    if (!signFlag) {
        // 没有运算符，直接标记等于状态
        eqFlag = true;
        lastSign = sign;
        lastOperand = num2;
        origNum1 = num1;
        resetDecimalState();
        updateDisplay();
        return;
    }

    // 执行运算
    double result = performOperation(num1, num2, sign);
    lastSign = sign;
    lastOperand = num2;
    origNum1 = num1;
    num1 = result;
    signFlag = false;
    eqFlag = true;
    num2 = 0.0;
    resetDecimalState();
    updateDisplay();
}
```

## Qt程序的发布

在发布Qt程序时，通常要发布 `Release` 版本，而要发布 `Release` 版本，就需要先配置 `Release` 版本。

过程为：

1、点击左边栏的“项目”，在构建设置（默认选项下）点击上方的“添加”的下拉按钮，选择 `Release` ，然后确定。

2、直接编译整个工程，此时得到的结果就是 `Release` 版本的了。

此时项目目录的 `build` 文件夹中就有 `Release` 版本的结果了，然后就可以将它转移到一个空目录下，随后打开Qt提供的命令提示符（搜索Qt版本号即可），将其中的目录切换到刚刚准备好的空目录下。随后键入命令：

```bash
windeployqt 可执行文件名
```

命令执行完毕后，工具会将所有依赖的文件全部都拉取到当前路径下，之后打包发布即可。

## Qt中的“容器”

Qt中存在一套类似STL容器的类，如 `QVector` 、`QList` 、`QString` ，其使用方式和STL容器大差不差，但多了很多实用的功能，如昨天用的 `QString::number` 就是一个很实用的功能。所以这些容器不做特殊讲解，只需要类比记忆即可，此处只强调和STL的不同之处。

### 和STL容器的区别

1、STL中顺序表容器在发生拷贝时，采用的方式是深拷贝，而在Qt中类似的容器在发生拷贝时使用的是**隐式共享**（写时拷贝）。

2、Qt中的顺序表容器存在两种空状态，一种和STL容器一样是没有元素的空状态，一种是指针为 `NULL` 的状态。

### Qstring

#### 常用函数

由于大部分行为都和 `std::string` 一致，所以这里只介绍不同的地方：

`mid` ：就是 `substr` ，完全一致

`trimmed` ：去掉当前字符串中首尾的空白字符

`to` 系列函数：这个系列中的函数非常多，大体分为：

1、数字相关的： `toInt` 、 `toDouble` 等，这系列函数的返回值和转换的内容相关

2、字符串相关的： `toStdString` 、 `toUpper` 、 `toLower` 等，这系列函数的返回值和转换的内容相关

3、字符集相关的： `toLatin1` 、 `toUtf8` 等，这系列函数的返回值是 `QByteArray` 类型。

`split` ：传入分隔符，切分字符串，切分好的字符串会以 `QStringList` 的方式返回

🌟上面的函数都不会修改原字符串。

`replace` ：查找替换，传入两个字符串，找到所有参数1的子串，替换为参数2。它会直接修改原字符串。

`number` ：静态函数，传入数字和一个进制数，将数字处理成该进制下的 `QString` 。进制数可以不传，默认十进制。

#### QString的特殊构造方式

```c++
QString str = QString("%1 is %2")
					.arg("kunkun")
					.arg(0, 5, 10, 'x'); //带宽度的写法
					 //数字 宽度 进制 填充字符
```

这种写法类似 `sprintf` ，可以使用包含占位符的字符串来构造一个字符串，每个占位符后面对应一个 `arg` 。

### QByteArray

这个类型字节数组，其整体的使用思想和常用函数都可以参考 `QString` ，但是 `QString` 更偏向字符串处理，而这个类型主要用来存储二进制数据。所以这个类型通常用户读取文件、收发信息等等。

`QByteArray` 类型可以转换为 `QString` ，但反之不行，要将 `QString` 对象给 `QByteArray` 对象的话需要用到 `to` 系列函数中的字符集转换函数。如：

```c++
QByteArray qba = "黄海波";
QString str(qba);
```

```c++
QString str = "黄海波";
QByteArray qba(str.toUtf8());
```

用 `qDebug` 的打印结果为：

```
"黄海波"
"\xE9\xBB\x84\xE6\xB5\xB7\xE6\xB3\xA2
```

### QStringList

这个类型其实就是 `QList<QString>` ，所以其完全可以对标 `vector<string>` 来记。因此其中的函数其实是 `QList` 中的函数。

`append` ：可以插入一个列表的 `push_back`

`prepend` ：可以插入一个列表的 `push_front`

`removeAt` ：下标版本的 `erase`

`removeOne` ：传入一个 `QString` ，查找并删除第一个找到的该元素。

`removeAll` ：传入一个 `QString` ，查找并删除所有找到的该元素。

`join` ：传入分隔符，将所有的元素用分隔符连接起来后返回成一个 `QString`

## 事件


事件是一些已经由Qt预设好的来自外部的”信号”（无源”信号”），这些“信号”会被**事件函数**捕获并处理。这些事件函数已经在平时常用的父类（如 `QWidget`）预设好了，如果要修改或充值其功能，只需要重写函数即可。

事件函数包罗万象，包括键盘的按下、鼠标的各种操作、窗口的关闭、重绘等等，这些都是外源性的、没有指定发射方的“信号”，而事件函数就可以视为是这种“信号”的“槽函数”。

🌟事件一经发生，就会由当前的活动控件所捕获，此时，就会触发该控件的事件函数。如果重写了，调用的当然是重写后的函数，否则事件就会被交由父窗口处理。在Qt中，面板（`QWidget`）上的控件会将当前面板视为父控件。

### 常用事件

#### QMouseEvent

鼠标事件，用于响应鼠标的各种操作。

##### QMouseEvent的事件函数

`mousePressEvent` ：鼠标按键按下事件，任意鼠标按键按下都会触发这个事件。

`mouseReleaseEvent` ：鼠标按键弹起事件，任意鼠标按键弹起都会触发这个事件。

`mouseDoubleClickEvent` ：鼠标按键双击事件，任意鼠标按键双击都会触发这个事件。

`mouseMoveEvent` ：鼠标移动事件，任意鼠标按键拖动都会触发这个事件。如果需要让其在自然移动时

就触发，则需要设置控件 `mouseTracking` 属性。

> `wheelEvent` ：滚轮滚动事件，当滚轮滚动时触发，其隶属于 `QWheelEvent` 。

##### QMouseEvent的函数

`button` ：记录当前按下的鼠标按键

`x / y` ：返回当前鼠标位置的横纵坐标

##### QMouseEvent的示例

```c++
void Widget::mousePressEvent(QMouseEvent* event)
{
    switch(event->button())
    {
    case Qt::LeftButton    : ui->label_1->setText("左键按下"); break;
    case Qt::RightButton   : ui->label_1->setText("右键按下"); break;
    case Qt::MiddleButton  : ui->label_1->setText("滚轮按下"); break;
    case Qt::BackButton    : ui->label_1->setText("后腿按下"); break;
    case Qt::ForwardButton : ui->label_1->setText("前进按下"); break;
    default:;
    }
}

void Widget::mouseReleaseEvent(QMouseEvent* event)
{
    switch(event->button())
    {
    case Qt::LeftButton    : ui->label_2->setText("左键弹起"); break;
    case Qt::RightButton   : ui->label_2->setText("右键弹起"); break;
    case Qt::MiddleButton  : ui->label_2->setText("滚轮弹起"); break;
    case Qt::BackButton    : ui->label_2->setText("后腿弹起"); break;
    case Qt::ForwardButton : ui->label_2->setText("前进弹起"); break;
    default:;
    }
}

void Widget::mouseDoubleClickEvent(QMouseEvent* event)
{
    switch(event->button())
    {
    case Qt::LeftButton    : ui->label_3->setText("左键双击"); break;
    case Qt::RightButton   : ui->label_3->setText("右键双击"); break;
    case Qt::MiddleButton  : ui->label_3->setText("滚轮双击"); break;
    case Qt::BackButton    : ui->label_3->setText("后腿双击"); break;
    case Qt::ForwardButton : ui->label_3->setText("前进双击"); break;
    default:;
    }
}

void Widget::mouseMoveEvent(QMouseEvent* event)
{
    QString str = QString("(%1,%2)").arg(event->position().x()).arg(event->position().y());
    ui->label_4->setText(str);
}
```

#### QKeyEvent

键盘事件，用于响应键盘上任意按键的按下和弹起操作。

##### QKeyEvent的事件函数

`keyPressEvent` ：键盘按键按下事件，任意键盘按键按下都会触发这个事件。

`keyReleaseEvent` ：键盘按键弹起事件，任意键盘按键弹起都会触发这个事件。

##### QKeyEvent的函数

`key` ：记录当前按下的键盘按键

`modifiers` ：记录当前按下的控制按键（ `ctrl shift alt` 等）

`isAutoRepeat` ：当按键长按时， `Press` 和 `Release` 事件会被频繁触发，此时，如果是长按导致的频发触发，则这个函数会返回 `true` 。通过判断其是否是 `true` ，就可以知道是否是长按导致的频繁触发，可以根据实际情况决定是否要屏蔽这种频繁触发。

##### QKeyEvent的示例

```c++
void Widget::keyPressEvent(QKeyEvent* event)
{
    if(event->isAutoRepeat())
    {
        return;
    }
    if(event->modifiers() == Qt::NoModifier)
    {
        switch(event->key())
        {
        case Qt::Key_W : ui->label_1->setText("W键按下"); break;
        case Qt::Key_A : ui->label_1->setText("A键按下"); break;
        case Qt::Key_S : ui->label_1->setText("S键按下"); break;
        case Qt::Key_D : ui->label_1->setText("D键按下"); break;
        default:;
        }
    }
    else if(event->modifiers() == Qt::ControlModifier)
    {
        switch(event->key())
        {
        case Qt::Key_W : ui->label_2->setText("Ctrl + W键按下"); break;
        case Qt::Key_A : ui->label_2->setText("Ctrl + A键按下"); break;
        case Qt::Key_S : ui->label_2->setText("Ctrl + S键按下"); break;
        case Qt::Key_D : ui->label_2->setText("Ctrl + D键按下"); break;
        default:;
        }
    }
    else if(event->modifiers() == (Qt::ControlModifier | Qt::ShiftModifier))
    {
        switch(event->key())
        {
        case Qt::Key_W : ui->label_3->setText("Ctrl + Shift + W键按下"); break;
        case Qt::Key_A : ui->label_3->setText("Ctrl + Shift + A键按下"); break;
        case Qt::Key_S : ui->label_3->setText("Ctrl + Shift + S键按下"); break;
        case Qt::Key_D : ui->label_3->setText("Ctrl + Shift + D键按下"); break;
        default:;
        }
    }
}

void Widget::keyReleaseEvent(QKeyEvent* event)
{
    if(event->isAutoRepeat())
    {
        return;
    }
    if(event->modifiers() == Qt::NoModifier)
    {
        switch(event->key())
        {
        case Qt::Key_W : ui->label_1->setText("W键弹起"); break;
        case Qt::Key_A : ui->label_1->setText("A键弹起"); break;
        case Qt::Key_S : ui->label_1->setText("S键弹起"); break;
        case Qt::Key_D : ui->label_1->setText("D键弹起"); break;
        default:;
        }
    }
    else if(event->modifiers() == Qt::ControlModifier)
    {
        switch(event->key())
        {
        case Qt::Key_W : ui->label_2->setText("Ctrl + W键弹起"); break;
        case Qt::Key_A : ui->label_2->setText("Ctrl + A键弹起"); break;
        case Qt::Key_S : ui->label_2->setText("Ctrl + S键弹起"); break;
        case Qt::Key_D : ui->label_2->setText("Ctrl + D键弹起"); break;
        default:;
        }
    }
    else if(event->modifiers() == (Qt::ControlModifier | Qt::ShiftModifier))
    {
        switch(event->key())
        {
        case Qt::Key_W : ui->label_3->setText("Ctrl + Shift + W键弹起"); break;
        case Qt::Key_A : ui->label_3->setText("Ctrl + Shift + A键弹起"); break;
        case Qt::Key_S : ui->label_3->setText("Ctrl + Shift + S键弹起"); break;
        case Qt::Key_D : ui->label_3->setText("Ctrl + Shift + D键弹起"); break;
        default:;
        }
    }
}
```

#### QCloseEvent

窗体关闭事件，当窗体正常关闭时触发。无论是什么原因导致的正常关闭，都会触发

##### QCloseEvent的事件函数

`closeEvent` ：窗体关闭事件，当窗体正常关闭时触发

##### QCloseEvent的函数

`ignore` ：忽视此次关闭事件

##### QCloseEvent的示例

```c++
void Widget::closeEvent(QCloseEvent* event)
{
	auto ret = QMessageBox::question(this, "询问", "是否要关闭窗口？");
	if (ret == QMessageBox::No)
	{
		event->ignore();
	}
}
```

#### QTimerEvent

定时器事件，可以每隔一段时间触发一次。

##### QTimerEvent的事件函数

`timerEvent` ：定时器事件，只要时间到就触发。

##### QTimerEvent的函数

`timerId` ：定时器触发时可以获取是谁触发的定时器

**相关函数**

`startTimer` ：启动定时器，传入一个毫秒数，返回 `timerId` 。

`killTimer` ：停止定时器，传入 `timerId` ，停止该定时器。

#### 课堂案例

简易的图片浏览器

功能细节：

1、可以用上一张下一张循环切换图片列表中的图片，可以用快捷键A D切换上一张和下一张

2、可以点击自动播放和停止播放来自动阅览图片，切换间隔2秒。在自动播放期间，禁止功能1中的切

换手段，且自动播放期间关闭窗口时需要询问用户

3、让自动播放和停止播放按钮间互斥

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QPixmap>
#include <QKeyEvent>
#include <QCloseEvent>
#include <QTimerEvent>
#include <QMessageBox>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

    void timerEvent(QTimerEvent* event) override;
    void keyPressEvent(QKeyEvent* event) override;
    void closeEvent(QCloseEvent* event) override;

private slots:
    void on_pushButton_prev_clicked();
    void on_pushButton_next_clicked();
    void on_pushButton_auto_clicked();
    void on_pushButton_stop_clicked();

private:
    Ui::Widget *ui;

    int timerID;
    QStringList playList;
    size_t pos;
    bool playFlag = false;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , playList({"D:/Qt_project/tmp/img/1.png",
                "D:/Qt_project/tmp/img/2.png",
                "D:/Qt_project/tmp/img/3.png",
                "D:/Qt_project/tmp/img/4.png",
                "D:/Qt_project/tmp/img/5.png"
      })
    , pos(0)
{
    ui->setupUi(this);
    this->setFixedSize(890, 590);
    QPixmap pmap;
    pmap.load(playList[pos]);
    ui->label->setPixmap(pmap);

    ui->pushButton_stop->setVisible(false);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::timerEvent(QTimerEvent* event)
{
    if (event->timerId() == timerID)
    {
        on_pushButton_next_clicked();
    }
}

void Widget::keyPressEvent(QKeyEvent* event)
{
    if(event->isAutoRepeat() || playFlag)
    {
        return;
    }

    if(event->modifiers() == Qt::NoModifier)
    {
        if(event->key() == Qt::Key_Left)
        {
            on_pushButton_prev_clicked();
        }
        else if(event->key() == Qt::Key_Right)
        {
            on_pushButton_next_clicked();
        }
    }
}

void Widget::closeEvent(QCloseEvent* event)
{
    if(playFlag)
    {
        auto ret = QMessageBox::question(this, "大胆", "谁给你的胆子关闭的");
        if(ret == QMessageBox::No)
        {
            event->ignore();
        }
    }
}

void Widget::on_pushButton_prev_clicked()
{
    pos = (pos - 1 + playList.size()) % playList.size();

    QPixmap pmap;
    pmap.load(playList[pos]);
    ui->label->setPixmap(pmap);
}

void Widget::on_pushButton_next_clicked()
{
    pos = (pos + 1) % playList.size();

    QPixmap pmap;
    pmap.load(playList[pos]);
    ui->label->setPixmap(pmap);
}

void Widget::on_pushButton_auto_clicked()
{
    timerID = startTimer(2000);

    playFlag = true;

    ui->pushButton_auto->setVisible(false);
    ui->pushButton_stop->setVisible(true);

    ui->pushButton_next->setEnabled(false);
    ui->pushButton_prev->setEnabled(false);
}

void Widget::on_pushButton_stop_clicked()
{
    killTimer(timerID);

    playFlag = false;

    ui->pushButton_stop->setVisible(false);
    ui->pushButton_auto->setVisible(true);

    ui->pushButton_next->setEnabled(true);
    ui->pushButton_prev->setEnabled(true);
}
```

### 事件过滤器

事件过滤器可以让指定控件（事实上不止控件）响应指定事件，它需要由用户指定控件类型和事件类型，然后编写该控件触发该事件后要执行的功能，最后将写好的事件过滤器安装到对应控件上即可。

事件过滤器函数是 `QObject` 类中的一个虚函数，有两个参数，分别是控件和事件，控件用 `QObject*` 传入，时间用 `QEvent*` 传入。返回值是一个 `bool` 值，如果是 `QObject` 类中的默认函数处理的，则一定会返回 `false` ，如果重写了，则要求将过滤掉的功能执行完毕后返回 `true` 。

#### Qt中创建事件过滤器的方法

以下创建事件过滤器的过程也适用于一般类的创建

1、右键项目→添加新文件

2、选 C/C++ 下的 C/C++ Class （第一个的第一个）

3、填写类名，并选择基类。（在事件过滤器中，基类选择 `QObject` ）

之后下一步，完成后，项目中就会多出一对 `.h` 和 `.cpp` 文件。

然后在头文件的类中添加事件过滤器函数的声明：

```c++
bool eventFilter(QObject* obj, QEvent* event);
```

这样，一个事件过滤器就创建好了。

##### 示例

```c++
#ifndef FOXEVENTFILTER_H
#define FOXEVENTFILTER_H

#include <QObject>
#include <QEvent>
#include <QPushButton>

class foxEventFilter : public QObject
{
    Q_OBJECT
public:
    explicit foxEventFilter(QObject *parent = nullptr);

    bool evenFilter(QObject* obj, QEvent* event);
signals:
    void moveSig(QPushButton* pbutton);
};

#endif // FOXEVENTFILTER_H
```

```c++
#include "foxeventfilter.h"

foxEventFilter::foxEventFilter(QObject *parent)
    : QObject{parent}
{}

bool foxEventFilter::evenFilter(QObject* obj, QEvent* event){
    //判断事件类型
    if (event->type() == QEvent::MouseMove)
    {
        //判断控件类型，可以用dynamic_cast，也可以用Qt提供的qobject_cast
        QPushButton* pbutton = dynamic_cast<QPushButton*>(obj);
        if (pbutton) //判断成功
        {
            emit moveSig(pbutton); //执行功能
            return true; //返回true表示过滤掉了
        }
    }
    return QObject::eventFilter(obj, event); //其余情况调用父类函数处理
}
```

#### 案例

使用事件过滤器完成“点不到的按钮”

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QmessageBox>
#include "foxeventfilter.h"
#include <cstdlib>
#include <ctime>
#include <cmath>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

private slots:
    void on_pushButton_clicked();

    void moveSigSlot(QPushButton* pbutton);

private:
    Ui::Widget *ui;

    foxEventFilter* fFilter;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , fFilter(new foxEventFilter)
{
    ui->setupUi(this);

    srand(time(NULL));
    ui->pushButton->installEventFilter(fFilter);

    connect(fFilter, &foxEventFilter::moveSig, this, &Widget::moveSigSlot);
}

Widget::~Widget()
{
    delete fFilter;
    delete ui;
}

void Widget::on_pushButton_clicked()
{
    QMessageBox::information(this, "哎呀", "居然被点到了！");
}

void Widget::moveSigSlot(QPushButton* pbutton)
{
    int widht = size().height();
    int butht = pbutton->size().height();

    int widwd = size().height();
    int butwd = pbutton->size().height();

    int x = pbutton->pos().x();
    int y = pbutton->pos().y();

    int randx, randy;
    int i;
    for(i = 0;i < 10;i++)
    {
        randx = rand() % (widwd - butwd);
        randy = rand() % (widht - butht);
        if(!(abs(randx - x) < butwd && abs(randy - y) < butht))
        {
            break;
        }
    }
    pbutton->move(randx, randy);
}
```

## 控件的提升

除了事件过滤器外，还有一种方式可以让控件响应具体事件，这种方式就是对控件做提升。即用一个子类继承现有控件类，然后在子类中添加一些功能（例如让其相应某事件），这种做法更加普遍。

提升本质上就是在现有控件类的基础上继承了个新的控件类，用户可以为其添加任意功能，Qt允许用户将已有控件提升为自己定义的控件。

### 自定义控件的创建

1、右键项目→添加新文件

2、选 C/C++ 下的 C/C++ Class （第一个的第一个）

3、填写类名，并选择基类。（在提升控件的过程中，基类选择 `QWidget` ）

之后下一步，完成后，项目中就会多出一对 `.h` 和 `.cpp` 文件。生成的文件中要改三个地方：

```c++
#ifndef FOXBUTTON_H
#define FOXBUTTON_H

#include <QPushButton> // 1

class foxButton : public QPushButton // 2
{
	Q_OBJECT
public:
	explicit foxButton(QWidget *parent = nullptr);
    
signals:
};

#endif // FOXBUTTON_H
```

```c++
#include "foxbutton.h"

foxButton::foxButton(QWidget *parent)
	: QPushButton{parent} // 3
{}
```

将这三个地方改成要继承的具体控件的类名即可。

之后，就可以完成功能补充了：

### 具体功能的补充

```c++
#ifndef FOXBUTTON_H
#define FOXBUTTON_H

#include <QPushButton>

class foxButton : public QPushButton
{
    Q_OBJECT
public:
    explicit foxButton(QWidget *parent = nullptr);

    void mouseMoveEvent(QMouseEvent* event);
signals:
    void moveSig(foxButton* pbutton);
};

#endif // FOXBUTTON_H
```

```c++
#include "foxbutton.h"

foxButton::foxButton(QWidget *parent)
    : QPushButton{parent}
{
    setMouseTracking(true); //  直接用代码设置mouseTracking
}

void foxButton::mouseMoveEvent(QMouseEvent* event)
{
    Q_UNUSED(event); //消除未使用参数的警告
    emit moveSig(this); // 触发事件直接发射信号
}
```

### 案例

用控件提升完成“点不到的按钮”

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QmessageBox>
#include "foxbutton.h"
#include <cstdlib>
#include <ctime>
#include <cmath>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

private slots:
    void on_pushButton_clicked();

    void moveSigSlot(foxButton* pbutton);

private:
    Ui::Widget *ui;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    connect(ui->pushButton, &foxButton::moveSig, this, &Widget::moveSigSlot);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::on_pushButton_clicked()
{
    QMessageBox::information(this, "哎呀", "居然被点到了！");
}

void Widget::moveSigSlot(foxButton* pbutton)
{
    int widht = size().height();
    int butht = pbutton->size().height();

    int widwd = size().height();
    int butwd = pbutton->size().height();

    int x = pbutton->pos().x();
    int y = pbutton->pos().y();

    int randx, randy;
    int i;
    for(i = 0;i < 10;i++)
    {
        randx = rand() % (widwd - butwd);
        randy = rand() % (widht - butht);
        if(!(abs(randx - x) < butwd && abs(randy - y) < butht))
        {
            break;
        }
    }
    pbutton->move(randx, randy);
}
```

## QMainWindow

创建 `QMainwindow` 的窗体后，一共会有三个部件，分别是：

1、中心 `widget` ，负责窗体主体

2、上方的菜单栏，可以承载下拉菜单

3、下方的状态栏，可以用于显示应用的一些信息

### 菜单栏

菜单栏（`QMenuBar`）上可以放置多个 `QMenu` 对象，每个 `QMenu` 对象都可以承载一个菜单树，即 `QMenu` 项中可以包含其他 `QMenu` 项或者 `QAction` 项（即 `QMenu` 是非叶子结点，`QAction` 是叶子结点）。在定义 `QMenu` 项或者 `QAction` 项时，可以加上一个 `& + 字符` 的格式定义默认快捷键，如加上 `&F` ，表示这个项可以用 `Alt+F` 快捷选取。

鼠标操作时，在 `QAction` 项右侧有一个带加号的图标，点击该图标，则可以编辑下级菜单，一旦添加了新的子项，则该 `QAction` 会升级为 `QMenu` 。子菜单中的 `QAction` 项还可以继续拥有子菜单。

🌟`QAction` 的名字在编辑时存在BUG，无法输入汉字，此时需要在别处编辑好后再粘贴进去。

`QAction` 无法通过右键转到槽的方式直接自动绑定槽函数，所以需要手动绑定，其点击时会触发 `triggered` 信号。所以给该信号绑定槽函数即可正常触发。

### 状态栏

底边的状态栏主要用来显示一些补充信息，如果不需要，右键直接移除即可。如果需要使用的话，可以让其显示一些信息。其特征是右下角的由六个小正方形构成的图标，其目的是扩大拖放窗口的鼠标变形范围。

## Qt中的预制窗口

### QMessageBox

`QMessageBox` 是Qt中最常用的预制窗体，平时主要使用的是 `QMessageBox` 中的静态函数，通过这些函数直接创建指定类型的窗体。如：`QMessageBox::information(this，"标题"，"正文")`，这样就可以直接通过一行代码创建一个窗体。

弹窗的基本格式：

|    函数名     |        图标        |  默认按钮  |
| :-----------: | :----------------: | :--------: |
|  `question`   |      蓝圈问号      | `Yes | No` |
| `information` | 蓝圈 i /蓝圈感叹号 |    `Ok`    |
|   `warning`   |    黄三角感叹号    |    `Ok`    |
|  `critical`   |      红圈叉号      |    `Ok`    |

#### 参数列表

1、父窗口，通常为 `this`

2、弹出窗体的标题

3、弹出窗体的正文

🌟以下参数可缺省

4、按钮的种类，如果要给多种按钮，则用位或连接。

5、设置弹出窗体时默认选中的按钮。

#### 返回值

窗体上任意按钮被点击时，窗口关闭，返回被点击的按钮。如果点 `x` 关闭窗口，则返回按钮中最靠右的那个。

🌟按钮在窗体上的顺序是Qt定好的，不会因为参数4中按钮的顺序变化而变化。

### QFileDialog

这是一个获取文件打开路径或保存路径的预制窗体，其目的是帮助用户获取一个已存在文件/目录的路径或者要保存的新文件的路径。

|         函数名         |      默认按钮       |               返回值                |
| :--------------------: | :-----------------: | :---------------------------------: |
|   `getOpenFileName`    |    `打开 | 取消`    |      `Qstring`，选中文件的路径      |
|   `getOpenFileNames`   |    `打开 | 取消`    | `QstringList`，所有选中的文件的路径 |
|   `getSaveFileName`    |    `保存 | 取消`    |    `Qstring`，要保存的文件的路径    |
| `getExistingDirectory` | `打开文件夹 | 取消` |      `Qstring`，选中目录的路径      |

#### 参数列表

1、父窗口，通常为 `this` 

2、弹出窗体的标题

🌟以下参数可缺省

3、打开窗体时默认打开的路径，默认是空字符串，表示当前路（即 `exe` 所在路径）

4、过滤器，选择只显示哪些后缀的文件。（`getExistingDirectory` 没有这个参数）

#### 返回值

正常则参考表中的返回值，如果取消或关闭窗口，则返回空字符串/空 `QstringList`

## QFile

`QFile` 是Qt中用来读写文件的库，其中包括了常规的打开、关闭、读写等操作，除此之外，Qt还提供了上面提到的 `QFileDialog` 来方便我们获取文件路径。

### 文件的打开、关闭、读写操作

#### 打开和关闭

```c++
//使用getOpenFileName获取一个已存在的文件的文件名
QString tmp = QFileDialog::getOpenFileName(this, "打开", "C:/Users/Clairad/Desktop");
if (tmp.isEmpty()) //如果获取失败，直接结束
{
	return;
}

QFile qf(tmp); //先加载这个文件

//然后打开这个文件
bool ret = qf.open(QIODevice::ReadOnly);
if (!ret)
{
	QMessageBox::critical(this, "记事本", "打开文件出错！");
	return;
}

qf.close();
```

##### open函数的参数和返回值

参数是打开方式，常见的打开方式包括：

`QIODevice::ReadOnly` ：只读

`QIODevice::WriteOnly` ：只写

`QIODevice::ReadWrite` ：读写

`QIODevice::Append` ：存在即追加，不存在则创建

`QIODevice::Truncate` ：存在即覆盖，不存在则创建

返回值是一个 `bool` ，打开成功则返回 `true` ，失败返回 `false`

#### 读写

`read` ：传入一个字节读取上限，至多读取这么多个字符。

`readLine` ：读取文本文件时，读取一整行。

`readAll` ：读出全部文件数据。

这三个函数会返回一个 `QByteArray` ，表示读出的内容。

`write` ：传入一个 `QByteArray` ，将其内容写入文件。

### 案例

简易文本编辑器

```c++
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QMessageBox>
#include <QFileDialog>
#include <QFile>
#include <QCloseEvent>
#include <QKeyEvent>

QT_BEGIN_NAMESPACE
namespace Ui {
class MainWindow;
}
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = nullptr);
    ~MainWindow() override;

    void closeEvent(QCloseEvent* event) override;
    void keyPressEvent(QKeyEvent* event) override;

    void saveFile();
    bool isSaved();

private:
    void Action_N_slot();
    void Action_O_slot();
    void Action_Q_slot();
    void Action_S_slot();
    void Action_Shift_S_slot();
    void Action_M_slot();

private slots:
    void on_textEdit_textChanged();

private:
    Ui::MainWindow *ui;

    QString openName;
    bool changeFlag = false;
};
#endif // MAINWINDOW_H
```

```c++
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    connect(ui->action_N, &QAction::triggered, this, &MainWindow::Action_N_slot);
    connect(ui->action_M, &QAction::triggered, this, &MainWindow::Action_M_slot);
    connect(ui->action_O, &QAction::triggered, this, &MainWindow::Action_O_slot);
    connect(ui->action_Q, &QAction::triggered, this, &MainWindow::Action_Q_slot);
    connect(ui->action_S, &QAction::triggered, this, &MainWindow::Action_S_slot);
    connect(ui->action_Shift_S, &QAction::triggered, this, &MainWindow::Action_Shift_S_slot);
}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::closeEvent(QCloseEvent* event)
{
    if(changeFlag && !isSaved())
    {
        event->ignore();
    }
}

void MainWindow::keyPressEvent(QKeyEvent* event)
{
    if(event->isAutoRepeat())
    {
        return;
    }

    if(event->modifiers() == Qt::ControlModifier)
    {
        switch(event->key())
        {
        case Qt::Key_N : Action_N_slot(); break;
        case Qt::Key_O : Action_O_slot(); break;
        case Qt::Key_S : Action_S_slot(); break;
        case Qt::Key_Q : Action_Q_slot(); break;
        case Qt::Key_M : Action_M_slot(); break;
        default:;
        }
    }

    if(event->modifiers() == (Qt::ControlModifier | Qt::ShiftModifier))
    {
        switch(event->key())
        {
        case Qt::Key_S : Action_Shift_S_slot(); break;
        default:;
        }
    }
}

bool MainWindow::isSaved()
{
    auto ret = QMessageBox::question(this, "记事本", "文件已修改，是否要保存文件",
                    QMessageBox::Yes | QMessageBox::No | QMessageBox::Cancel);
    switch(ret)
    {
    case QMessageBox::Yes :
        Action_S_slot();
        if(openName.isEmpty())
        {
            return false;
        }
        else
        {
            return true;
        }
    case QMessageBox::No : return true;
    case QMessageBox::Cancel : return false;
    default:;
    }
    return false;
}

void MainWindow::Action_N_slot()
{
    if(changeFlag && !isSaved())
    {
        return;
    }

    openName.clear();
    ui->textEdit->clear();
    changeFlag = false;
}

void MainWindow::Action_O_slot()
{
    if(changeFlag && !isSaved())
    {
        return;
    }

    QString tmp = QFileDialog::getOpenFileName(this, "打开", "D:/");
    if(tmp.isEmpty())
    {
        return;
    }
    openName = tmp;

    QFile qf(openName);

    bool ret = qf.open(QIODevice::ReadOnly);
    if(!ret)
    {
        QMessageBox::critical(this, "记事本", "打开文件出错！");
        return;
    }
    ui->textEdit->clear();
    ui->textEdit->setText(qf.readAll());

    qf.close();
    changeFlag = false;
}

void MainWindow::Action_Q_slot()
{
    close();
}

void MainWindow::saveFile()
{
    QFile qf(openName);

    bool ret = qf.open(QIODevice::WriteOnly);
    if(!ret)
    {
        QMessageBox::critical(this, "记事本", "保存文件出错！");
        return;
    }

    qf.write(ui->textEdit->toPlainText().toUtf8());

    qf.close();
}

void MainWindow::Action_S_slot()
{
    if(openName.isEmpty())
    {
        QString tmp = QFileDialog::getSaveFileName(this, "保存文件", "D:/");
        if(tmp.isEmpty())
        {
            return;
        }

        openName = tmp;
    }

    saveFile();
    changeFlag = false;
}

void MainWindow::Action_Shift_S_slot()
{
    QString tmp = QFileDialog::getSaveFileName(this, "另存为", "D:/");
    if(tmp.isEmpty())
    {
        return;
    }

    openName = tmp;

    saveFile();
    changeFlag = false;
}

void MainWindow::Action_M_slot()
{
    QMessageBox::information(this, "关于", "一个文本编辑软件");
}

void MainWindow::on_textEdit_textChanged()
{
    changeFlag = true;
}
```

## Item Views / Item Widgets

视图类控件（就是这两类控件）其实就是前端常用的架构 `MCV(Model-controller-View)` 。

### Item Views

`Views` 控件是`Model-Based` 的，它的底层依赖一个 `Model` 为其提供数据，而 `controller` 已经由Qt完成，无需关心，用户只需要给 `Model` 合适的数据即可。Qt提供了各种各样的 `Model` 类型，可以根据具体 `views` 的不同，选择对应的 `Model` 。其中 `QstandardItemModel` 是可以适配所有 `views` 的。

#### 示例

一个 `ListView` 的使用示例，完成一个每点击一次按钮，就将 `LineEdit` 中的数据提交到 `ListView` 中的 `demo`

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QStringListModel>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

private slots:
    void on_pushButton_clicked();

private:
    Ui::Widget *ui;

    QStringList data;
    QStringListModel* model;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , model(new QStringListModel)
{
    ui->setupUi(this);

    ui->listView->setModel(model);
}

Widget::~Widget()
{
    delete model;
    delete ui;
}

void Widget::on_pushButton_clicked()
{
    data.append(ui->lineEdit->text());
    model->setStringList(data);
    ui->lineEdit->clear();
}
```

如果不希望 `View` 中的数据可以被双击编辑，那么可以设置 `editTrigger` 属性，去掉所有的勾后，它就成只读的了。

### Item Widgets

这类控件是 `Item Views` 控件的简化版本，它们不再需要由用户指定 `Model` 类型，而是固定了 `Model` 类型，只能直接通过控件给其中的 `Item` 赋值，所以灵活性大打折扣，但是易用性直线上升。如用同样的规则实现刚刚的 `demo`

#### 示例

```c++
#include "widget.h"
#include "ui_widget.h"
Widget::Widget(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Widget)
{
	ui->setupUi(this);
}

Widget::~Widget()
{
	delete ui;
}

void Widget::on_pushButton_clicked()
{
	ui->listWidget->addItem(ui->lineEdit->text());
	ui->lineEdit->clear();
}
```

明显可以看出代码简单了不少，也能看出如果使用的是 `Item Views` 控件，则用户操作的实际上是 `Model` ，而如果使用的是 `Item Widgets` 控件，操作的往往就是控件本身。

### 案例

将之前的文本编辑器扩展为CSV编辑器

```c++
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QMessageBox>
#include <QFileDialog>
#include <QFile>
#include <QCloseEvent>
#include <QKeyEvent>
#include <QStandardItem>
#include <QStandardItemModel>

QT_BEGIN_NAMESPACE
namespace Ui {
class MainWindow;
}
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit MainWindow(QWidget *parent = nullptr);
    ~MainWindow() override;

    void closeEvent(QCloseEvent* event) override;
    void keyPressEvent(QKeyEvent* event) override;

    void saveFile();
    bool isSaved();

private:
    void Action_N_slot();
    void Action_O_slot();
    void Action_Q_slot();
    void Action_S_slot();
    void Action_Shift_S_slot();
    void Action_M_slot();

    void Action_R_slot();
    void Action_C_slot();

    void Action_Rrow_slot();
    void Action_Rcol_slot();

    void Action_Down_slot();
    void Action_Up_slot();

    void Action_Adapt_slot();
private slots:
    void on_tableView_doubleClicked(const QModelIndex &index);

private:
    Ui::MainWindow *ui;

    QString openName;
    bool changeFlag = false;

    QStandardItemModel* model;
};
#endif // MAINWINDOW_H
```

```c++
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
    , model(new QStandardItemModel)
{
    ui->setupUi(this);

    ui->tableView->setModel(model);

    connect(ui->action_N, &QAction::triggered, this, &MainWindow::Action_N_slot);
    connect(ui->action_M, &QAction::triggered, this, &MainWindow::Action_M_slot);
    connect(ui->action_O, &QAction::triggered, this, &MainWindow::Action_O_slot);
    connect(ui->action_Q, &QAction::triggered, this, &MainWindow::Action_Q_slot);
    connect(ui->action_S, &QAction::triggered, this, &MainWindow::Action_S_slot);
    connect(ui->action_Shift_S, &QAction::triggered, this, &MainWindow::Action_Shift_S_slot);

    connect(ui->action_R, &QAction::triggered, this, &MainWindow::Action_R_slot);
    connect(ui->action_C, &QAction::triggered, this, &MainWindow::Action_C_slot);

    connect(ui->action_Rrow, &QAction::triggered, this, &MainWindow::Action_Rrow_slot);
    connect(ui->action_Rcol, &QAction::triggered, this, &MainWindow::Action_Rcol_slot);

    connect(ui->action_Down, &QAction::triggered, this, &MainWindow::Action_Down_slot);
    connect(ui->action_Up, &QAction::triggered, this, &MainWindow::Action_Up_slot);

    connect(ui->action_Adapt, &QAction::triggered, this, &MainWindow::Action_Adapt_slot);
}

MainWindow::~MainWindow()
{
    delete model;
    delete ui;
}

void MainWindow::closeEvent(QCloseEvent* event)
{
    if(changeFlag && !isSaved())
    {
        event->ignore();
    }
}

void MainWindow::keyPressEvent(QKeyEvent* event)
{
    if(event->isAutoRepeat() && event->key() != Qt::Key_D && event->key() != Qt::Key_U)
    {
        return;
    }

    if(event->modifiers() == Qt::ControlModifier)
    {
        switch(event->key())
        {
        case Qt::Key_N : Action_N_slot(); break;
        case Qt::Key_O : Action_O_slot(); break;
        case Qt::Key_S : Action_S_slot(); break;
        case Qt::Key_Q : Action_Q_slot(); break;
        case Qt::Key_M : Action_M_slot(); break;
        default:;
        }
    }

    if(event->modifiers() == (Qt::ControlModifier | Qt::ShiftModifier))
    {
        switch(event->key())
        {
        case Qt::Key_S : Action_Shift_S_slot(); break;
        case Qt::Key_R : Action_R_slot(); break;
        case Qt::Key_C : Action_C_slot(); break;
        case Qt::Key_D : Action_Down_slot(); break;
        case Qt::Key_U : Action_Up_slot(); break;
        default:;
        }
    }
}

bool MainWindow::isSaved()
{
    auto ret = QMessageBox::question(this, "CSV编辑器", "文件已修改，是否要保存文件",
                                     QMessageBox::Yes | QMessageBox::No | QMessageBox::Cancel);
    switch(ret)
    {
    case QMessageBox::Yes :
        Action_S_slot();
        if(openName.isEmpty())
        {
            return false;
        }
        else
        {
            return true;
        }
    case QMessageBox::No : return true;
    case QMessageBox::Cancel : return false;
    default:;
    }
    return false;
}

void MainWindow::Action_N_slot()
{
    if(changeFlag && !isSaved())
    {
        return;
    }

    openName.clear();
    model->clear();
    changeFlag = false;
}

void MainWindow::Action_O_slot()
{
    if(changeFlag && !isSaved())
    {
        return;
    }

    QString tmp = QFileDialog::getOpenFileName(this, "打开", "D:/");
    if(tmp.isEmpty())
    {
        return;
    }
    openName = tmp;

    QFile qf(openName);

    bool ret = qf.open(QIODevice::ReadOnly);
    if(!ret)
    {
        QMessageBox::critical(this, "CSV编辑器", "打开文件出错！");
        return;
    }

    model->clear();
    QString line = qf.readLine();
    QStringList lineData;
    int maxColCount = 0;
    int i, j;
    for(i = 0; !line.isEmpty(); i++)
    {
        lineData = line.trimmed().split(",");
        if(lineData.size() > maxColCount)
        {
            maxColCount = lineData.size();
            model->setColumnCount(maxColCount);
        }

        for(j = 0; j < lineData.size(); j++)
        {
            model->setItem(i, j, new QStandardItem(lineData[j]));
        }
        line = qf.readLine();
    }

    qf.close();
    changeFlag = false;
    ui->tableView->resizeColumnsToContents();
    ui->tableView->resizeRowsToContents();
}

void MainWindow::Action_Q_slot()
{
    close();
}

void MainWindow::saveFile()
{
    QFile qf(openName);

    bool ret = qf.open(QIODevice::WriteOnly);
    if(!ret)
    {
        QMessageBox::critical(this, "CSV编辑器", "保存文件出错！");
        return;
    }

    QStringList strl;
    int i, j;
    for(i = 0; i < model->rowCount(); i++)
    {
        for(j = 0; j < model->columnCount(); j++)
        {
            QStandardItem* item = model->item(i, j);
            if(item)
            {
                strl.append(item->text());
            }
            else
            {
                strl.append("");
            }
        }
        qf.write((strl.join(",") + "\r\n").toUtf8());
        strl.clear();
    }
    qf.close();
}

void MainWindow::Action_S_slot()
{
    if(openName.isEmpty())
    {
        QString tmp = QFileDialog::getSaveFileName(this, "保存文件", "D:/");
        if(tmp.isEmpty())
        {
            return;
        }

        openName = tmp;
    }

    saveFile();
    changeFlag = false;
}

void MainWindow::Action_Shift_S_slot()
{
    QString tmp = QFileDialog::getSaveFileName(this, "保存文件", "D:/");
    if(tmp.isEmpty())
    {
        return;
    }

    openName = tmp;

    saveFile();
    changeFlag = false;
}

void MainWindow::Action_M_slot()
{
    QMessageBox::information(this, "关于", "一个CSV编辑软件");
}

void MainWindow::Action_R_slot()
{
    model->appendRow(NULL);
}

void MainWindow::Action_C_slot()
{
    model->setColumnCount(model->columnCount() + 1);
}

void MainWindow::Action_Rrow_slot()
{
    if (!model) return;

    int rowCount = model->rowCount();
    if (rowCount <= 0) return; // 没有行可删

    int lastRow = rowCount - 1;
    int colCount = model->columnCount();

    bool hasContent = false;
    for(int col = 0;col < colCount; col++)
    {
        QStandardItem* item = model->item(lastRow, col);
        if (item) {
            QString text = item->text().trimmed();
            if (!text.isEmpty()) {
                hasContent = true;
                break;
            }
        }
    }
    if(hasContent)
    {
        auto ret = QMessageBox::question(this, "询问", "尾行有数据，确定要删除吗");
        if(ret != QMessageBox::Yes)
        {
            return;
        }
    }

    model->removeRow(lastRow);
}

void MainWindow::Action_Rcol_slot()
{
    if (!model) return;

    int colCount = model->columnCount();
    if (colCount <= 0) return; // 没有列可删

    int lastCol = colCount - 1;
    int rowCount = model->rowCount();

    bool hasContent = false;
    for(int row = 0; row < rowCount; row++)
    {
        QStandardItem* item = model->item(row, lastCol);
        if (item) {
            QString text = item->text().trimmed();
            if (!text.isEmpty()) {
                hasContent = true;
                break;
            }
        }
    }
    if(hasContent)
    {
        auto ret = QMessageBox::question(this, "询问", "尾列有数据，确定要删除吗");
        if(ret != QMessageBox::Yes)
        {
            return;
        }
    }
    model->removeColumn(lastCol);
}

void MainWindow::on_tableView_doubleClicked(const QModelIndex &index)
{
    Q_UNUSED(index);
    changeFlag = true;
}

void MainWindow::Action_Up_slot()   // 增大表格字体
{
    QFont font = ui->tableView->font();
    int newSize = font.pointSize() + 1;
    if (newSize > 72) newSize = 72;
    font.setPointSize(newSize);
    ui->tableView->setFont(font);
    ui->tableView->resizeColumnsToContents();
    ui->tableView->resizeRowsToContents();
}

void MainWindow::Action_Down_slot()     // 减小表格字体
{
    QFont font = ui->tableView->font();
    int newSize = font.pointSize() - 1;
    if (newSize < 6) newSize = 6;
    font.setPointSize(newSize);
    ui->tableView->setFont(font);
    ui->tableView->resizeColumnsToContents();
    ui->tableView->resizeRowsToContents();
}

void MainWindow::Action_Adapt_slot()
{
    ui->tableView->resizeColumnsToContents();
    ui->tableView->resizeRowsToContents();
}
```

## Qt中的内置拓展库

Qt中提供了大量的内置功能库，可以让Qt方便的调用各种常见功能的接口。这些库存放在 `Qt根目录/Qt版本号/编译器名/include` 目录下。在这里，每一个库都以一个独立文件夹的形式存在。

```c++
QT 		+= core gui
greaterThan(QT_MAJOR_VERSION, 4): QT += widget
```

这些库都以 Qt 开头，去掉 Qt 后，剩余的就是库名。在 `qmake` 的管理文件 `.pro` 中，可以用 `Qt += 库名`的方式加载这些库。而Qt程序默认包含三个库：

即 `core gui widgets` ，这三个库是默认包含的，他们分别负责底层支撑、前台显示和各类控件，都是Qt应用程序必不可少的组件，而其他组件可以选择性加入。

例如如果要播放音视频，则需要引入多媒体库，其文件夹的名字为 `QtMultimedia` ，所以其库名就是 `multimedia` ，故在 `.pro` 文件中，需要加上这个库名：

```c++
QT 		+= core gui multimedia
```

之后， `QtMultimedia` 下的头文件就可以正常引用了。

🌟即使不加库，其实也可以临时引用，形式为 `#include <QtMultimedia/QMediaPlayer>` ，但这个用法非常不方便，所以很少使用。

### 多媒体库

之前学习控件的思路可以套用到学习库上，毕竟控件本身也是一种库。所以学习时，也是从属性和函数、信号两个方面学习。

#### QMediaPlayer库

##### 常用属性

`source` ：播放器加载的媒体，参数是一个 `QUrl` 类的参数

`position` ：播放器当前播放的位置，单位为 `ms`

`audioOutput` ：音频输出类，要设置一个 `QAudioOutput` 类为音频输出设备

##### 其他函数

`duration` ：已加载的媒体文件的总长度，单位为 `ms`

`play / pause / stop` ：播放/暂停/停止

##### 常用信号

`playbackStateChanged(QMediaPlayer::PlaybackState)`

当播放状态发生改变时触发，参数是一个枚举值，其对应的枚举为：

```c++
enum PlaybackState
{
	StoppedState,
	PlayingState,
	PausedState
};
```

当媒体状态发生改变时触发，参数是一个枚举值，其对应的枚举为：

```c++
enum MediaStatus
{
	NoMedia,
	LoadingMedia,
	LoadedMedia,
	StalledMedia,
	BufferingMedia,
	BufferedMedia,
	EndOfMedia,
	InvalidMedia
};
```

具体含义：

|     枚举名称      |                       描述                       |                         典型处理建议                         |
| :---------------: | :----------------------------------------------: | :----------------------------------------------------------: |
|    `NoMedia `     |                  未加载任何媒体                  |            提示用户选择媒体文件，禁用播放控制按钮            |
|  `LoadingMedia`   |        正在加载媒体（网络流或本地大文件）        |        显示加载动画/进度条，禁用播放按钮直到加载完成         |
|   `LoadedMedia`   |              媒体已加载但未开始播放              |                    启用播放按钮，准备播放                    |
|  `StalledMedia`   |        播放过程中数据流中断（如网络波动）        |           显示缓冲提示，尝试重新连接或等待数据恢复           |
| `BufferingMedia`  |       正在缓冲媒体数据（网络流播放时常见）       |             显示缓冲进度条，暂停播放直到缓冲完成             |
|  `BufferedMedia`  |           媒体数据已缓冲完成，准备就绪           |                  隐藏缓冲提示，启用播放按钮                  |
| **`EndOfMedia `** |                 媒体播放到达末尾                 | 自动停止播放，提示"播放完成"，可连接`playbackFinished()` 信号处理循环播放逻辑 |
|  `InvalidMedia`   | 媒体文件无效或无法解析（格式不支持、文件损坏等） |     显示错误提示"无法播放此文件"，提供重新选择媒体的选项     |

`durationChanged(qint64)`

媒体播放的总时长发生变化时触发，参数是新媒体的总时长。

通常这个信号会出现在切换播放文件时，参数的单位是毫秒。

`positionChanged(qint64)`

媒体播放的位置发生变化时触发，参数是当前的播放位置。

通常用于表示进度条，参数的单位是毫秒。

#### QMediaPlayer库

##### 常用属性

`volume` ：音量，取值范围是 `[0,1]` 之间的浮点数

`muted` ：静音， `bool` 值

`device` ：音频输出设备，如耳机、音箱等

##### 常用信号

三个属性都有对应的信号，即 `volumeChanged` 、 `mutedChanged` 、 `deviceChanged` ，和属性一一对应。今天只会用到 `mutedChanged`

##### 测试代码

测试音频是否畅通的代码：

```c++
audio->setVolume(1);
player->setAudioOutput(audio);
player->setSource(QUrl("D:/1.mp3"));
player->play();
```

#### 课堂案例

MP3播放器

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QAudioOutput>
#include <QMediaPlayer>
#include <QMessageBox>
#include <QFile>
#include <QFileDialog>
#include <QStringListModel>
#include <QCloseEvent>
#include <ctime>
#include <cstdlib>
#include <QMediaDevices>
#include <QAudioDevice>

#define DFLPATH "D:/音乐/music"
#define SETTINGFILE "setting.dat"

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

    enum PLAT_MODE{LIST_PLAY, LIST_LOOP, SING_LOOP, RAND_PLAY};
private:
    QString timeStr(qint64 num);
    void playSong();

    void saveList(const QString& path);
    void readList(const QString& path);

    void closeEvent(QCloseEvent* event) override;

    void handleAudioOutputsChanged();
    void makeFile();

private slots:
    void on_verticalSlider_sliderMoved(int position);
    void on_verticalSlider_sliderReleased();
    void on_horizontalSlider_sliderMoved(int position);
    void on_horizontalSlider_sliderReleased();

    void positionChangedSlot(qint64 pos);
    void durationChangedSlot(qint64 dur);

    void on_pushButtonPlayOrPause_clicked();
    void on_pushButtonStop_clicked();

    void on_pushButtonMute_clicked();

    void mutedChangedSlot(bool isMute);
    void playbackStateChangedSlot(QMediaPlayer::PlaybackState state);

    void mediaStatusChangedSlot(QMediaPlayer::MediaStatus status);

    void on_pushButtonBack_clicked();
    void on_pushButtonFront_clicked();

    void on_pushButtonAdd_clicked();
    void on_pushButtonDel_clicked();

    void on_listView_doubleClicked(const QModelIndex &index);

    void on_pushButtonPrev_clicked();
    void on_pushButtonNext_clicked();

    void on_pushButtonUp_clicked();
    void on_pushButtonDown_clicked();

    void on_pushButtonImport_clicked();
    void on_pushButtonExport_clicked();

    void on_pushButtonPosing_clicked();
    void on_pushButtonClear_clicked();

private:
    Ui::Widget *ui;

    QMediaPlayer* player;
    QAudioOutput* audio;

    int pos = -1;
    int currentPos = -1;
    bool isPlaying = false;
    bool isMuteing = false;
    int currentVolume = 100;
    QStringList songData;
    QStringListModel* model;

    QMediaDevices mediaDevices;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , player(new QMediaPlayer)
    , audio(new QAudioOutput)
    , model(new QStringListModel)
{
    ui->setupUi(this);
    makeFile();
    connect(player, & QMediaPlayer::positionChanged, this, &Widget::positionChangedSlot);
    connect(player, & QMediaPlayer::durationChanged, this, &Widget::durationChangedSlot);
    connect(audio, & QAudioOutput::mutedChanged, this, &Widget::mutedChangedSlot);
    connect(player, & QMediaPlayer::playbackStateChanged, this, &Widget::playbackStateChangedSlot);
    connect(player, &QMediaPlayer::mediaStatusChanged, this, &Widget::mediaStatusChangedSlot);
    connect(&mediaDevices, &QMediaDevices::audioOutputsChanged, this, &Widget::handleAudioOutputsChanged);

    ui->listView->setModel(model);
    QStringList modes = {"顺序播放", "列表循环", "单曲循环", "随机播放"};
    for (int i = 0; i < modes.size(); ++i) {
        ui->comboBox->addItem(modes[i]);
        ui->comboBox->setItemData(i, Qt::AlignCenter, Qt::TextAlignmentRole);
    }
    readList(SETTINGFILE);

    srand(time(NULL));
    audio->setVolume(currentVolume / 100.0);
    ui->verticalSlider->setSliderPosition(currentVolume);
    ui->labelVolume->setText(QString::number(currentVolume));

    player->setAudioOutput(audio);
}

Widget::~Widget()
{
    delete model;
    delete audio;
    delete player;
    delete ui;
}

void Widget::makeFile()
{
    QFile file(SETTINGFILE);
    if (!file.exists()) {
        if (file.open(QIODevice::WriteOnly)) {
            file.close();
        }
    }
}

QString Widget::timeStr(qint64 num)
{
    num /= 1000;
    return QString("%1:%2").arg(num / 60, 2, 10, '0').arg(num % 60, 2, 10, '0');
}

void Widget::playSong()
{
    if(pos != currentPos)
    {
        player->setSource(QUrl(songData[pos]));
        currentPos = pos;
    }
    player->play();
    isPlaying = true;
    ui->listView->setCurrentIndex(model->index(pos));
}

void Widget::on_verticalSlider_sliderMoved(int position)
{
    audio->setVolume(position / 100.0);
    ui->labelVolume->setText(QString::number(position));
    audio->setMuted(false);
    isMuteing = false;
    currentVolume = pos;
}

void Widget::on_verticalSlider_sliderReleased()
{
    int pos = ui->verticalSlider->value();
    audio->setVolume(pos / 100.0);
    ui->labelVolume->setText(QString::number(pos));
    audio->setMuted(false);
    isMuteing = false;
    currentVolume = pos;
}

void Widget::on_horizontalSlider_sliderMoved(int position)
{
    ui->labelCurPos->setText(timeStr(position));
}

void Widget::on_horizontalSlider_sliderReleased()
{
    int pos = ui->horizontalSlider->value();
    player->setPosition(pos);
}

void Widget::positionChangedSlot(qint64 pos)
{
    if (!ui->horizontalSlider->isSliderDown())
    {
        ui->horizontalSlider->setValue(pos);
        ui->labelCurPos->setText(timeStr(pos));
    }
}

void Widget::durationChangedSlot(qint64 dur)
{
    ui->labelAllPos->setText(timeStr(dur));
    ui->horizontalSlider->setMaximum(dur);
}

void Widget::on_pushButtonPlayOrPause_clicked()
{
    if(isPlaying == false)
    {
        if(pos < 0)
        {
            QMessageBox::warning(this, "警告", "播放列表为空");
            return;
        }
        playSong();
    }
    else{
        player->pause();
        isPlaying = false;
    }
}

void Widget::on_pushButtonStop_clicked()
{
    player->stop();
    isPlaying = false;
}

void Widget::on_pushButtonMute_clicked()
{
    if(isMuteing == true)
    {
        audio->setMuted(false);
        isMuteing = false;
    }
    else
    {
        audio->setMuted(true);
        isMuteing = true;
    }
}

void Widget::mutedChangedSlot(bool isMute)
{
    if(isMute)
    {
        ui->pushButtonMute->setText("解除静音");

        ui->labelVolume->setText("静音");
    }
    else
    {
        ui->pushButtonMute->setText("静音");

        ui->labelVolume->setText(QString::number(currentVolume));
    }
}

void Widget::playbackStateChangedSlot(QMediaPlayer::PlaybackState state)
{
    switch (state)
    {
    case QMediaPlayer::StoppedState:
    case QMediaPlayer::PausedState:
        ui->pushButtonPlayOrPause->setText("播放");
        break;
    case QMediaPlayer::PlayingState:
        ui->pushButtonPlayOrPause->setText("暂停");
        break;
    }
}

void Widget::on_pushButtonBack_clicked()
{
    qint64 pos = player->position();
    pos += 5000;
    if(pos > player->duration())
    {
        pos = player->duration();
    }
    player->setPosition(pos);
}

void Widget::on_pushButtonFront_clicked()
{
    qint64 pos = player->position();
    pos -= 5000;
    if(pos < 0)
    {
        pos = 0;
    }
    player->setPosition(pos);
}

void Widget::on_listView_doubleClicked(const QModelIndex &index)
{
    pos = index.row();
    if(pos == currentPos && isPlaying == true)
    {
        player->pause();
        isPlaying = false;
    }
    else
    {
        pos = index.row();
        playSong();
    }
}

void Widget::on_pushButtonPrev_clicked()
{
    switch(ui->comboBox->currentIndex())
    {
    case LIST_PLAY:
    case SING_LOOP:
        if(pos == 0)
        {
            QMessageBox::critical(this, "MP3播放器", "已经是第一首了");
            return;
        }
        pos--;
        playSong();
        break;
    case LIST_LOOP:
        pos = (pos - 1 + songData.size()) % songData.size();
        playSong();
        break;
    case RAND_PLAY:
        pos = rand() % songData.size();
        playSong();
        break;
    default:;
    }
}

void Widget::on_pushButtonNext_clicked()
{
    switch(ui->comboBox->currentIndex())
    {
    case LIST_PLAY:
    case SING_LOOP:
        if(pos == songData.size() - 1)
        {
            QMessageBox::critical(this, "MP3播放器", "已经是最后一首了");
            return;
        }
        pos++;
        playSong();
        break;
    case LIST_LOOP:
        pos = (pos + 1) % songData.size();
        playSong();
        break;
    case RAND_PLAY:
        pos = rand() % songData.size();
        playSong();
        break;
    default:;
    }
}

void Widget::on_pushButtonAdd_clicked()
{
    bool flag = false;
    QModelIndex tmp;
    if(songData.isEmpty())
    {
        flag = true;
    }
    else
    {
        tmp = ui->listView->currentIndex();
    }

    QStringList res = QFileDialog::getOpenFileNames(this, "添加音乐文件", DFLPATH);
    if(res.isEmpty())
    {
        return;
    }

    songData.append(res);
    model->setStringList(songData);
    if(flag)
    {
        pos = 0;
        ui->listView->setCurrentIndex(model->index(0));
    }
    else
    {
        ui->listView->setCurrentIndex(tmp);
    }
}

void Widget::mediaStatusChangedSlot(QMediaPlayer::MediaStatus status)
{
    if(status == QMediaPlayer::EndOfMedia)
    {
        if(ui->comboBox->currentIndex() == SING_LOOP)
        {
            player->play();
            return;
        }
        on_pushButtonNext_clicked();
    }
}

void Widget::on_pushButtonDel_clicked()
{
    if(songData.isEmpty())
    {
        QMessageBox::critical(this, "MP3播放器", "列表中没有歌曲，无法删除！");
        return;
    }

    int del = ui->listView->currentIndex().row();

    if(del < pos)
    {
        pos--;
    }
    else if(del == pos)
    {
        if(pos == songData.size() - 1 && player->playbackState() == QMediaPlayer::StoppedState)
        {
            pos--;
        }
        else if(player->playbackState() == QMediaPlayer::PlayingState
                || player->playbackState() == QMediaPlayer::PausedState)
        {
            QMessageBox::critical(this, "MP3播放器", "歌曲正在播放，无法删除");
            return;
        }
    }
    songData.remove(del);
    model->setStringList(songData);

    if(del == songData.size())
    {
        del--;
    }

    if(songData.isEmpty())
    {
        pos = -1;
        ui->labelAllPos->setText("00:00");
    }
    else
    {
        ui->listView->setCurrentIndex(model->index(del));
    }
}

void Widget::on_pushButtonUp_clicked()
{
    int mov = ui->listView->currentIndex().row();
    if(mov == 0)
    {
        QMessageBox::critical(this, "MP3播放器", "已经是第一首了");
        return;
    }

    if(mov == pos)
    {
        pos--;
    }
    else if(mov - 1 == pos)
    {
        pos++;
    }

    songData[mov].swap(songData[mov - 1]);
    model->setStringList(songData);
    ui->listView->setCurrentIndex(model->index(mov - 1));
}

void Widget::on_pushButtonDown_clicked()
{
    int mov = ui->listView->currentIndex().row();
    if(mov == songData.size() - 1)
    {
        QMessageBox::critical(this, "MP3播放器", "已经是最后一首了");
        return;
    }

    if(mov == pos)
    {
        pos++;
    }
    else if(mov + 1 == pos)
    {
        pos--;
    }

    songData[mov].swap(songData[mov + 1]);
    model->setStringList(songData);
    ui->listView->setCurrentIndex(model->index(mov + 1));
}

void Widget::readList(const QString& path)
{
    QFile qf(path);

    bool ret = qf.open(QIODevice::ReadOnly);
    if(!ret)
    {
        QMessageBox::critical(this, "MP3播放器", "打开播放列表出错");
        return;
    }
    QString volumeStr = qf.readLine();
    currentVolume = volumeStr.toInt();


    QString str = qf.readLine();
    while(!str.isEmpty())
    {
        songData.append(str.trimmed());
        str = qf.readLine();
    }

    model->setStringList(songData);
    ui->listView->setCurrentIndex(model->index(0));
    pos = 0;
    qf.close();
}

void Widget::saveList(const QString& path)
{
    QFile qf(path);

    bool ret = qf.open(QIODevice::WriteOnly | QIODevice::Truncate);
    if(!ret)
    {
        QMessageBox::critical(this, "MP3播放器", "保存播放列表出错");
        return;
    }
    qf.write(QString::number(currentVolume).toUtf8() + "\r\n");
    for(auto& e : songData)
    {
        qf.write((e + "\r\n").toUtf8());
    }

    qf.close();
}

void Widget::on_pushButtonImport_clicked()
{
    QString lst = QFileDialog::getOpenFileName(this, "打开播放列表", DFLPATH, "*.lst");
    if(lst.isEmpty())
    {
        return;
    }

    readList(lst);
}


void Widget::on_pushButtonExport_clicked()
{
    QString lst = QFileDialog::getSaveFileName(this, "保存播放列表", DFLPATH, "*.lst");
    if(lst.isEmpty())
    {
        return;
    }

    saveList(lst);
}

void Widget::closeEvent(QCloseEvent* event)
{
    Q_UNUSED(event);
    saveList(SETTINGFILE);
}

void Widget::on_pushButtonClear_clicked()
{
    player->stop();
    isPlaying = false;
    songData.clear();
    model->setStringList(songData);

    pos = -1;
    ui->horizontalSlider->setValue(0);
    ui->labelCurPos->setText("00:00");
    ui->labelAllPos->setText("00:00");

    ui->pushButtonPlayOrPause->setText("播放");
}

void Widget::on_pushButtonPosing_clicked()
{
    if (pos >= 0 && pos < songData.size()) {
        QModelIndex index = model->index(pos);
        ui->listView->setCurrentIndex(index);
        ui->listView->scrollTo(index, QAbstractItemView::PositionAtCenter);
    } else {
        QMessageBox::information(this, "MP3播放器", "当前没有正在播放的歌曲");
    }
}

void Widget::handleAudioOutputsChanged()
{
    // 获取当前默认输出设备
    QAudioDevice defaultDevice = QMediaDevices::defaultAudioOutput();

    // 如果当前设备与之前不同，则重新设置
    if (audio->device() != defaultDevice) {
        // 记录当前播放位置（可选，用于恢复进度）
        qint64 position = player->position();

        // 重新设置输出设备
        audio->setDevice(defaultDevice);

        // 如果是播放状态，重启播放以应用新设备
        if (player->playbackState() == QMediaPlayer::PlayingState) {
            player->setPosition(position);
            player->play();
        }
    }
}
```

## 多窗口Qt应用

如果Qt中需要添加一个新窗体，则需要单独为这个窗体创建一套文件（ `.h` 、 `.cpp` 、 `.ui` ）

1、项目右键→添加新文件

2、选择 `Qt -> Qt Widgets Designer Form Class` （第四个的第二个，最长的那个）

3、根据需求，选择不同的窗体类型（通常建议选择 `Dialog` 而非 `Widget` ），起名并完成

此时，一套文件就自动加载进了当前项目。

新的这套文件对应着一个新的独立窗体，如果要默认显示这个窗体（一打开Qt就是这个窗体），则可以选择更改 `main` 函数。如：

```c++
#include "form.h" //更改头文件
#include <QApplication>
int main(int argc, char *argv[])
{
	QApplication a(argc, argv);
	Form w; //更改默认打开的窗体类型
	w.show();
	return a.exec();
}
```

可以看到这里使用 `show` 显示了窗体，实际上有三个基本操作：

`show` ：展示窗体

`hide` ：隐藏窗体

`close` ：关闭窗体

三种动作都有对应的 `event` 。

🌟这里的 `close` 也可以理解为一种会将窗体重制的 `hide` ，本质上都是让窗体隐藏。而Qt中只要没有窗体处于 `show` 状态（即所有窗体都隐藏了），则Qt应用程序就会退出。

### 窗体的类型

在选择窗体类型时，一般有三种五个选项，忽略三种不同的 Dialog 带来的重复，本质上也是`MainWindow` 、 `Dialog` 、 `Widget` 三种。其中只有 `Widget` 比较特殊。

`QWidget` 本质上是一个控件，所以如果某个其他窗体在探出时将其设置为了自己的子窗体，此时`QWidget` 并不会弹出为一个独立窗体，而是直接将自己的控件嵌入到父窗体中，所以一般建议如果需要窗体弹出，则一定要使用 `QDialog` 。

当然，也可以通过 `setWindowFlag` 函数来更改窗体的类型，如：

```c++
Widget* pw = new Widget(this); //Widget窗体默认是QWidget类型的，认了父类则不会弹出
pw->setWindowFlag(Qt::Window); //在这里改成Window类型即可正常弹出
pw->show();
```

### 窗体的模态

窗体的模态可以直接在属性编辑器中设置，属性名为 WindowModality ，一共有三种模态：

```c++
enum WindowModality {
	NonModal, 			//无模态，不阻塞任何其他窗口
	WindowModal, 		//窗体模态，阻塞所有祖先窗口
	ApplicationModal 	//应用模态，阻塞所有其他窗口
};
```

新建的窗体默认无模态，而Qt的预制窗体都是应用模态，在平时使用时可以根据具体需求来调整窗体的模态。

### 案例

让两个无关窗体间完成如下关联：

（1）点击A窗体的按钮，弹出B窗体。

（2）当A、B窗体的任意一个关闭时，另一个也关闭。

🌟要求，A窗体只能弹出一个B窗体，且B窗体不能有模态。

窗体A `Form` ：

```c++
#ifndef FORM_H
#define FORM_H

#include <QWidget>
#include <QCloseEvent>
#include "widget.h"

namespace Ui {
class Form;
}
class Form : public QWidget
{
	Q_OBJECT
        
public:
	explicit Form(QWidget *parent = nullptr);
	~Form();
	void closeEvent(QCloseEvent* event) override;
private slots:
	void on_pushButton_clicked();
    
    void closeSigSlot();
private:
	Ui::Form *ui;
    
	Widget* pw = nullptr;
};
#endif // FORM_H
```

```c++
#include "form.h"
#include "ui_form.h"

Form::Form(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Form)
{
	ui->setupUi(this);
}

Form::~Form()
{
	delete ui;
}

void Form::closeEvent(QCloseEvent* event)
{
	if (pw)
	{
		pw->close();
		delete pw;
	}
}

void Form::on_pushButton_clicked()
{
	if (pw)
	{
		//pw->show();
		return;
	}
	pw = new Widget;
	connect(pw, &Widget::closeSig, this, &Form::closeSigSlot);
	pw->setWindowFlag(Qt::Window);
	pw->show();
}

void Form::closeSigSlot()
{
	close();
}
```

窗体B `Widget` ：

```c++
#ifndef WIDGET_H
#define WIDGET_H
#include <QWidget>
#include <QCloseEvent>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE
    
class Widget : public QWidget
{
	Q_OBJECT
        
public:
	Widget(QWidget *parent = nullptr);
	~Widget();
    
	void closeEvent(QCloseEvent* event) override;
signals:
	void closeSig();
    
private:
	Ui::Widget *ui;
};

#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"
Widget::Widget(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Widget)
{
	ui->setupUi(this);
}

Widget::~Widget()
{
	delete ui;
}

void Widget::closeEvent(QCloseEvent* event)
	{
	emit closeSig();
}
```

基本思路：

1、A窗体在创建B窗体时，采用单例模式中的懒汉模式思想，即第一次调用时创建，之后调用直接返回，A窗体在关闭时，直接检查B窗体是否还在，如果还在，直接关闭。

2、B窗体在关闭时给A窗体发信号，A窗体收到信号后也关掉自己。

🌟去掉思路2，只保留思路1的情况下是一个常见的帮助窗体的写法，即弹出帮助窗体，帮助用户边查阅信息边操作面板。

## Qt中的TCP库

在Qt中使用网络相关库时，需要在 `.pro` 文件中加入 `network` 。

```c++
QT += widgets network
```

和TCP相关的库有两个，即 `QTcpSocket` 和 `QTcpServer`

### 常用函数

`listen` ： `QTcpServer` 的函数，相当于绑定+监听，需要传入 `IP` 和端口号。

`nexPendingConnection` ： `QTcpServer` 的函数，相当于 `accept` 。这个函数通常用于 `newConnection` 信号的槽函数中，返回一个 `QTcpSocket` 的对象，用于和指定客户端通讯。



`connectToHost` ： `QTcpSocket` 的函数，传入点分十进制的字符串和端口号。

`waitForConnect` ： `QTcpSocket` 的函数，传入一个毫秒数（可以省略，默认为30000），表示连接时的等待时间，在此过程中会不断尝试连接，如果时间到后连接仍未建立，则在时间到后返回 `false` 。只要连接建立，则函数立即返回 `true` 。

🌟和Linux类似，通讯时使用的 `QTcpSocket` 的函数就是之前学过的 `QFile` 中的那些函数，关闭连接也依然是 `close` 。

### 常用信号

`newConnection` ： `QTcpServer` 的信号，当有客户端发起连接请求时触发。



`connected` ： `QTcpSocket` 的信号，当连接建立时触发。

`disconnected` ： `QTcpSocket` 的信号，当连接断开时触发。

`readyRead` ： `QTcpSocket` 的信号，当信道中有待读取数据时触发。

### 示例

完成一个基础的C/S框架

客户端：

```c++
#ifndef WIDGET_H
#define WIDGET_H
#include <QWidget>
#include <QTcpSocket>
#include <QMessageBox>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE
    
class Widget : public QWidget
{
	Q_OBJECT
        
public:
	Widget(QWidget *parent = nullptr);
	~Widget();

private slots:
	void on_pushButton_clicked();
    
private:
	Ui::Widget *ui;
    
	QTcpSocket* socket;
};
#endif // WIDGET
```

```c++
#include "widget.h"
#include "ui_widget.h"
Widget::Widget(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Widget)
	, socket(new QTcpSocket)
{
	ui->setupUi(this);
}

Widget::~Widget()
{
	delete ui;
}

void Widget::on_pushButton_clicked()
{
	socket->connectToHost(ui->lineEdit_IP->text(), ui->lineEdit_PORT->text().toUShort());
	bool ret = socket->waitForConnected(5000);
	if (!ret)
	{
		QMessageBox::critical(this, "错误", "连接失败！");
		return;
	}
	QMessageBox::information(this, "提示", "连接成功！");
}
```

服务器：

```c++
#ifndef WIDGET_H
#define WIDGET_H
#include <QWidget>
#include <QTcpServer>
#include <QTcpSocket>
#include <QMessageBox>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE
    
class Widget : public QWidget
{
	Q_OBJECT
        
public:
	Widget(QWidget *parent = nullptr);
	~Widget();
    
private:
	void newConnectionSlot();
    
private:
	Ui::Widget *ui;
    
	QTcpServer* server;
};
#endif // WIDGET_H

```

```c++
#include "widget.h"
#include "ui_widget.h"
Widget::Widget(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Widget)
	, server(new QTcpServer)
{
	ui->setupUi(this);
        
	server->listen(QHostAddress::Any, 10086);
        
	connect(server, &QTcpServer::newConnection, this, &Widget::newConnectionSlot);
}

Widget::~Widget()
{
	delete ui;
}

void Widget::newConnectionSlot()
{
	QTcpSocket* socket = server->nextPendingConnection();
	if (socket == nullptr)
    {
		QMessageBox::critical(this, "错误", "连接失败！");
		return;
	}
	QMessageBox::information(this, "提示", "连接成功！");
}
```

### 客户端服务应答项目

完成一个客户端服务应答系统，这个系统的功能包括：

1、客户端有两个界面，没有父子关系，默认弹出登录框，登录框上有IP和端口用于连接服务器。登录成功后，登录框会隐藏在后台，聊天窗口显示。聊天窗口关闭时，程序关闭。

2、服务器同样有两个界面，没有父子关系，默认一个等待的窗口挂在前台，一旦有客户端连接，则前台弹出一个和指定客户端通讯的窗口。

3、两侧的聊天窗体有任何一侧关闭，另一侧都将得知，并决定是要关闭窗体还是继续浏览聊天记录，此时无法发送消息。

#### 服务器

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QTcpServer>
#include <QTcpSocket>
#include <QMessageBox>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

private:
    void newConnectionSlot();

private:
    Ui::Widget *ui;

    QTcpServer* server;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"
#include "chatdialog.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , server(new QTcpServer)
{
    ui->setupUi(this);

    server->listen(QHostAddress::Any, 10086);

    connect(server, &QTcpServer::newConnection, this, &Widget::newConnectionSlot);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::newConnectionSlot()
{
    QTcpSocket* socket = server->nextPendingConnection();
    if(socket == nullptr)
    {
        QMessageBox::critical(this, "错误", "连接超时");
        return;
    }

    chatDialog* chat = new chatDialog(socket);
    chat->show();
}
```

#### 客户端

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QTcpSocket>
#include <QMessageBox>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

private slots:
    void on_pushButton_clicked();

private:
    Ui::Widget *ui;

    QTcpSocket* socket;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"
#include "chatdialog.h"
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , socket(new QTcpSocket)
{
    ui->setupUi(this);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::on_pushButton_clicked()
{
    socket->connectToHost(ui->lineEdit_IP->text(), ui->lineEdit_PORT->text().toUShort());
    bool ret = socket->waitForConnected(5000);
    if (!ret)
    {
        QMessageBox::critical(this, "错误", "连接失败！");
        return;
    }
    chatDialog* chat = new chatDialog(socket);
    chat->show();
    hide();
}
```

#### 聊天窗体

```c++
#ifndef CHATDIALOG_H
#define CHATDIALOG_H

#include <QDialog>
#include <QTcpSocket>
#include <QCloseEvent>
#include <QMEssageBox>

namespace Ui {
class chatDialog;
}

class chatDialog : public QDialog
{
    Q_OBJECT

public:
    explicit chatDialog(QTcpSocket* socket, QWidget *parent = nullptr);
    ~chatDialog();

    void closeEvent(QCloseEvent* event);

private slots:
    void on_pushButtonClear_clicked();
    void on_pushButtonSend_clicked();

private:
    void readyReadSlot();
    void disconnectSlot();

private:
    Ui::chatDialog *ui;

    QTcpSocket* socket;

    bool closeFlag = false;
};

#endif // CHATDIALOG_H
```

```c++
#include "chatdialog.h"
#include "ui_chatdialog.h"

chatDialog::chatDialog(QTcpSocket* sock, QWidget *parent)
    : QDialog(parent)
    , ui(new Ui::chatDialog)
    , socket(sock)
{
    ui->setupUi(this);

    setWindowTitle(socket->peerAddress().toString());

    connect(socket, &QTcpSocket::readyRead, this, &chatDialog::readyReadSlot);
    connect(socket, &QTcpSocket::disconnected, this, &chatDialog::disconnectSlot);
    connect(ui->textEdit, &chatTextEdit::sendSig, this, &chatDialog::on_pushButtonSend_clicked);
}

chatDialog::~chatDialog()
{
    delete ui;
}

void chatDialog::closeEvent(QCloseEvent* event)
{
    Q_UNUSED(event);
    closeFlag = true;
    socket->close();
}

void chatDialog::readyReadSlot()
{
    ui->listWidget->addItem("客户端：");
    ui->listWidget->addItem(socket->readAll());
    ui->listWidget->scrollToBottom();
}

void chatDialog::disconnectSlot()
{
    if(closeFlag)
    {
        return;
    }

    auto ret = QMessageBox::question(this, "询问", "对方已断开连接，是否关闭窗口？");
    if(ret == QMessageBox::Yes)
    {
        close();
        return;
    }

    closeFlag = true;
}

void chatDialog::on_pushButtonClear_clicked()
{
    ui->textEdit->clear();
}


void chatDialog::on_pushButtonSend_clicked()
{
    if(closeFlag)
    {
        QMessageBox::warning(this, "警告", "连接已断开，无法发送数据！");
        return;
    }

    QString data = ui->textEdit->toPlainText();
    socket->write(data.toUtf8());
    ui->listWidget->addItem("服务器：");
    ui->listWidget->addItem(data);
    ui->listWidget->scrollToBottom();
    ui->textEdit->clear();
}
```

#### 提升的TextEdit

```c++
#ifndef CHATTEXTEDIT_H
#define CHATTEXTEDIT_H

#include <QTextEdit>
#include <QKeyEvent>

class chatTextEdit : public QTextEdit
{
    Q_OBJECT
public:
    explicit chatTextEdit(QWidget *parent = nullptr);

    void keyPressEvent(QKeyEvent* event);
signals:
    void sendSig();
};

#endif // CHATTEXTEDIT_H
```

```c++
#include "chattextedit.h"

chatTextEdit::chatTextEdit(QWidget *parent)
    : QTextEdit{parent}
{}

void chatTextEdit::keyPressEvent(QKeyEvent* event)
{
    if(event->isAutoRepeat())
    {
        return;
    }

    if(event->modifiers() == Qt::NoModifier)
    {
        if(event->key() == Qt::Key_Return)
        {
            emit sendSig();
            return;
        }
    }
    else if(event->modifiers() == Qt::ControlModifier)
    {
        if(event->key() == Qt::Key_Return)
        {
            append("");
            return;
        }
    }
    QTextEdit::keyPressEvent(event);
}
```

## Qt中的一些其他类

### QTimer

#### 相关函数

`start` ：传入一个毫秒数，开启一个定时器，没过这么多毫秒触发一次 `timeout` 信号

`stop` ：停止定时器

#### 相关信号

`timeout` ：当定时器数到时触发。

### QThread

`QThread` 库在具体使用时是要提升的，在提升过程中，必须重写 `run` 函数，这个函数是线程执行函数。`run` 函数没有参数也没有返回值，但既然是成员函数，则有 `this` ，所有的参数和返回值都通过 `this` 带进带出。

#### 常用函数

`start` ：用 `run` 函数开启一个线程

`wait` ：等待线程执行完毕（ `join` ）

#### 课堂案例

用 `QThread` 完成一个生产者消费者模型

提升的线程类

```c++
#ifndef PROCONTHREAD_H
#define PROCONTHREAD_H

#include <QThread>
#include <QMutex>

class ProductorThread : public QThread
{
    Q_OBJECT
public:
    explicit ProductorThread(QMutex& m, int& s, int n, QObject *parent = nullptr);
    void run();
signals:

private:
    QMutex& mutex;
    int& src;
    int num;
};

class ConsumerThread : public QThread
{
    Q_OBJECT
public:
    explicit ConsumerThread(QMutex& m, int& s, QObject *parent = nullptr);
    void run();
    int getCount();
signals:

private:
    QMutex& mutex;
    int& src;
    int count = 0;
};
#endif // PROCONTHREAD_H
```

```c++
#include "proconthread.h"

ProductorThread::ProductorThread(QMutex& m, int& s, int n, QObject *parent)
    : QThread{parent}
    , mutex(m)
    , src(s)
    , num(n)
{}

void ProductorThread::run()
{
    mutex.lock();
    src += num;
    mutex.unlock();
}

ConsumerThread::ConsumerThread(QMutex& m, int& s, QObject *parent)
    : QThread{parent}
    , mutex(m)
    , src(s)
{}

void ConsumerThread::run()
{
    count = 0;
    while(src > 0)
    {
        mutex.lock();
        if(src > 0)
        {
            src--;
            count++;
        }
        mutex.unlock();
    }
}

int ConsumerThread::getCount()
{
    return count;
}
```

触发产者消费者模型的主面板

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QVector>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

private slots:
    void on_pushButton_clicked();

private:
    Ui::Widget *ui;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"
#include "proconthread.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::on_pushButton_clicked()
{
    QMutex mutex;
    int src = 0;
    ProductorThread pth(mutex, src, ui->spinBox_Source->value());
    pth.start();
    int threadnum = ui->spinBox_Count->value();
    QVector<ConsumerThread*> cth(threadnum);
    pth.wait();
    for (auto& e : cth)
    {
        e = new ConsumerThread(mutex, src);
        e->start();
    }
    for (int i = 0; i < cth.size(); i++)
    {
        cth[i]->wait();
        ui->listWidget->addItem(QString("%1 : %2").arg(i, 2).arg(cth[i]->getCount()));
    }
    ui->listWidget->addItem("");
    ui->listWidget->scrollToBottom();
}
```

## SQLite

`SQLite` 是一个轻量级的数据库，和之前提到的 `mysql` 等数据库有着很大的差别。虽然它也是关系型数据库，也是库-表二级结构，但其没有服务器和客户端，就是一个单纯的应用程序，它使用不同的文件管理不同的库，每个库中存放若干表。即DDL语句中关于库的增删查改语句在 `SQLite` 中就不存在了。

`SQLite` 的体量非常小，通常只有MB级，所以在一些有一定硬件资源的嵌入式环境上可以用它来管理数据。

使用 `SQLite` 时，无需配置任何环境，打开对应的工具（终端或者可视化工具，此处介绍终端）即可使用，在 `SQLite` 上，要使用 `SQLite` 提供的一些语句来操作库：

`.help` ：可以查看所有的 `SQLite` 指令，类似 `mysql` 中的 `\h`

`.open` ：后跟一个数据库名，可以打开一个数据库，如果不存在就创建它

`.table` ：可以查看当前库中的所有表，相当于 `SHOW TABLES`

`SQLite` 中虽然不会用 SQL 语句管理库，但表的语句依然成立，如 `CREATE TABLE` 等，但语句中存在少量的不同，例如创建表时，主键不再默认非空，且不支持 `AUTO_INCREMENT` 、 `INSERT` 语句不能省略`INTO` 等。但实际上也只有这些细节上的差别， SQL 语句在使用上的区别并不算大。

Qt中默认支持 `SQLite` 数据库，且无需任何配置

## Qt中的数据库

`QtSql` 库是Qt中提供的数据库相关的结构的库，使用前要在 `.pro` 文件中加上 sql ：

```c++
QT += sql
```

这个库中，默认提供了多种数据库的接口，但只有 `SQLite` 不需要做任何配置直接就可以使用。

Qt的 `sql` 库中主要包含三个类，这三个类由于只会响应用户的指令，而不会主动做事，所以没有常用的信号。

### QSqlDatabase

这个库完成的事数据库的宏观操作，如连接、断开、选择数据库等。

`addDatabase` ：静态函数，创建一个新的数据库连接，需要传入一个字符串，表示要连接的数据库类型，如  `QSQLITE` 、 `QMYSQL` 等。

`setDatabaseName` ：设置库名，传入一个字符串，表示数据库的库名。

`open` ：打开设置好的数据库

`close` ：关闭数据库

`isOpen` ：检查数据库的打开状态

`lastError` ：以 QSqlError 的形式返回最后一次的错误信息

### QSqlQuery

这个函数主要用于处理各类和 SQL 语句相关的功能，如执行语句、获取语句执行结果等，其执行的主要是 DDL 和 DML 语句， DCL 语句一般由独立的函数处理。

这个类在定义对象时，只要有一个已经打开的数据库，就会自动在该数据库下执行语句，而不需要手动连接或定。

`prepare` ：在执行 DML 语句时，可以用这个函数来预处理语句，然后再执行，这样可以有效规避 SQL 注入

`bindValue` ：服务于 `prepare` 函数的，为 `prepare` 函数中要填入的具体位置提供数据

`exec` ：执行语句，可以传入一个参数，表示要执行这个参数对应的 SQL 语句。如果不传参，表示执行准备好的语句。

`lastError` ：以 `QSqlError` 的形式返回最后一次的错误信息

🌟当执行的语句是查询语句时，可以用如下函数来获取查询结果

`first` ：返回结果集中第一行的结果，返回 `false` 则表示结果集为空

`next` ：返回结果集中下一行的结果，返回 `false` 则表示结果集已经访问完毕

`value` ：传入一个下标，返回这一行的第几个数据。返回值类型为 `QVariant` ，需要用 `toString` 等函数转换其类型

🌟关于 DCL 语句，以事务为例， `trasaction` 函数表示开启事务， `rollback` 函数表示回滚事务，`commit` 函数表示提交事务，这些都以函数形式实现，而非调用 `exec`

### QSqlError

这个库用于表示数据库处理中的错误信息。

`text` ：返回错误的文本信息

`type` ：返回错误码

### 案例

一个数据 `demo` ，功能包括：

1、可以通过输入 `id` 和 `name` 向数据库中插入数据

2、可以执行一条用户输入的 SQL 语句

3、可以将数据表中的数据实时展示到 `tableWidget` 中

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QSqlDatabase>
#include <QSqlQuery>
#include <QSqlError>
#include <QMessageBox>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

    void refreshData();

private slots:
    void on_pushButton_add_clicked();

    void on_pushButton_exe_clicked();

private:
    Ui::Widget *ui;

    QSqlDatabase db;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
    , db(QSqlDatabase::addDatabase("QSQLITE"))
{
    ui->setupUi(this);

    db.setDatabaseName("test.db");
    bool ret = db.open();
    if(!ret)
    {
        QMessageBox::critical(this, "错误", "数据库打开失败：" + db.lastError().text());
        return;
    }

    QSqlQuery query;
    ret = query.exec("CREATE TABLE IF NOT EXISTS `student`("
                    "`id` INT UNSIGNED PRIMARY KEY NOT NULL,"
                     "`name` VARCHAR(128) NOT NULL);");
    if(!ret)
    {
        QMessageBox::critical(this, "错误", "创建数据表失败：" + query.lastError().text());
        close();
        return;
    }

    ui->tableWidget->setColumnCount(2);
    ui->tableWidget->setHorizontalHeaderLabels({"id", "name"});

    refreshData();
}

Widget::~Widget()
{
    delete ui;
}

void Widget::refreshData()
{
    QSqlQuery query;
    bool ret = query.exec("SELECT * FROM `student`;");
    if(!ret)
    {
        QMessageBox::critical(this, "错误", "查询数据失败：" + query.lastError().text());
        return;
    }

    int i;
    int maxid = -1;
    ret = query.first();
    for(i = 0; ret; i++)
    {
        ui->tableWidget->setRowCount(i + 1);

        ui->tableWidget->setItem(i, 0, new QTableWidgetItem(query.value(0).toString()));
        ui->tableWidget->setItem(i, 1, new QTableWidgetItem(query.value(1).toString()));

        maxid = query.value(0).toInt();
        ret = query.next();
    }
    ui->spinBox->setValue(maxid + 1);
}

void Widget::on_pushButton_add_clicked()
{
    if(ui->lineEdit->text().isEmpty())
    {
        QMessageBox::critical(this, "错误", "请输入姓名！");
        return;
    }

    QSqlQuery query;
    query.prepare("INSERT INTO `student` VALUES( :id, :name);");
    query.bindValue(":id", ui->spinBox->value());
    query.bindValue(":name", ui->lineEdit->text());

    bool ret = query.exec();
    if(!ret)
    {
        QMessageBox::critical(this, "错误", "插入数据失败：" + query.lastError().text());
        return;
    }

    ui->lineEdit->clear();
    refreshData();
}

void Widget::on_pushButton_exe_clicked()
{
    if(ui->textEdit->toPlainText().isEmpty())
    {
        QMessageBox::critical(this, "错误", "请输入语句！");
        return;
    }

    QSqlQuery query;
    bool ret = query.exec(ui->textEdit->toPlainText());
    if(!ret)
    {
        QMessageBox::critical(this, "错误", "插入数据失败：" + query.lastError().text());
        return;
    }
    refreshData();
}
```

## Qt中的资源文件

Qt中可以添加资源文件，资源文件可以用来管理图片、图标等外部资源。添加资源文件的方法是：

1、右键项目→添加新文件→ Qt下的 `Qt Resource Flie` （第四个的第四个）

2、填写资源文件的名字后一路下一步即可

此时项目中会多出一个叫“资源”的目录，里面会出现一个 `.qrc` 文件，这个文件就是资源文件。

右键这个 `.qrc` 文件，选择添加现有文件，将打算作为资源的文件添加进去。

🌟强烈建议将这些文件以文件夹的形式存放好放在当前应用程序的目录下，来保证相对路径不变。

添加好后， `.qrc` 下会出现一个根目录，其为当前项目的所在路径，文件的路径都是以这个路径为参考的相对路径。

添加好 `.qrc` 文件和其中的资源文件后，就可以用 `":/1.png"` 的方式来访问资源文件中已经添加好的文件了。

如果当前路径下有专门的资源文件夹，全部添加后，可以以当前路径为参照物来确定文件的路径，例如当前目录下有一个 `a` 目录， `a` 目录下有 `1.png` ，则引入的方式就成了 `:/a/1.png` 。

🌟尽量不要在上一级目录或者更上面的目录下放资源文件。

如下是在资源文件中放入了项目路径下的 `wheel.png` 和 `test.ico` 的情况下，给 `label` 设置图片和给当前窗体设置图标的代码：

```c++
QPixmap pm(":/wheel.png");
ui->label->setPixmap(pm);

setWindowIcon(QIcon(":/test.ico"));
```

## QPaint

`QPaint` 是Qt提供的画图工具，这个画图依赖于Qt的 `Paint` 事件，即 `paintEvent` 。只需要用户重写`paintEvent` ，并在这个函数中绘图，即可完成画图功能。

在 `QPaint` 中包含三种基础类：

### QPainter

这个类指的是“画家类”。这个类负责最终做画的工作，相当于一个拿着画笔和画刷的画家，所有的做画动作都是由它完成的。

`setRenderHints` ：开启画家的一些特性，如抗锯齿、图片平滑等，通常这么写：

```c++
painter.setRenderHints(QPainter::Antialiasing |
					   QPainter::TextAntialiasing |
					   QPainter::SmoothPixmapTransform);
```

注意，有一个 `setRenderHint` 函数，不带 `s` ，这个函数只能设1个特性，而带了 `s` 的才能用 `|` 连接多个特性。

`translate` ：传入横纵坐标，让当前的默认坐标加上这两个数值后确定新的横纵坐标。一开始默认坐标是 `(0,0)`

`restore` ：将当前的默认坐标恢复到 (0,0)

`setPen` ：设置画笔

`setBrush` ：设置画刷（只有在封闭图形时，画刷才会生效）

`draw` 系列函数：这系列函数可以绘制出各种各样形状的图形，如 `drawEllipse` 是绘制椭圆，给出圆心和横纵半径即可绘制，如 `drawRect` 是绘制矩形，给出左上角坐标和宽高即可绘制等。其他的还有`drawPath` ，表示绘制一个 `QPainterPath` 对象中已经设置好的图案， `drawText` 表示绘制文本等等。

#### QPainterPath

这个类可以提前预制好一些线条和图形，然后将其直接放到画布上。

`moveTo` ：让当前准备绘制的线条的起始位置移动到某个坐标上。

`lineTo` ：从当前位置绘制一条到达目标位置的线段，同时当前位置切换。

`closeSubpath` ：让当前绘制的线段连接成为一个封闭图形

### QPen

这个类是“画笔类”。可以用来设置线条的粗细、画笔的颜色等等

`setColor` ：设置颜色，颜色用RGB数值表示，如 `QColor(255, 0, 0)` 来表示红色

`setWidth` ：设置线条宽度等

### QBrush

这个类是“画刷类”。主要用来填充封闭图形。也可以设置渐变颜色。

`setColor` ：设置颜色，颜色用RGB数值表示，如 `QColor(255, 0, 0)` 来表示红色

#### 其他函数

`update` ：触发重绘事件，可以选择在某个时间点触发。

`repaint` ：触发重绘事件，但不建议使用，因为这个函数会强行打断别的事件来执行重绘时间

#### 案例

1、绘制出一个可以随着 `Dial` 旋转的死亡圣器图案

```c++
void Widget::paintEvent(QPaintEvent *event)
{
    Q_UNUSED(event);

    QPainter painter(this);

    painter.setRenderHints(QPainter::Antialiasing |
                           QPainter::TextAntialiasing |
                           QPainter::SmoothPixmapTransform);

    QPixmap pm(":/1.png");  //设置背景图片
                    //前四个参数给定了一个矩形，图片会在这个矩形范围内缩放
    painter.drawPixmap(0, 0, width(), height(), pm);

    QPen textPenrect;
    textPenrect.setColor(QColor(214, 173, 214));
    painter.setPen(textPenrect);

    QFont font("经典繁方篆", 100);
    painter.setFont(font);
    QRect textRect(0, 0, width(), height() / 3); //设置文字矩形框
    painter.drawText(textRect, Qt::AlignCenter, "国之印信在此，还不速速跪下");

    //调整默认位置
    painter.translate(width() / 2, height() / 2);
    //调整坐标系角度
    painter.rotate(angle);

    //设置正三角形的各种属性
    double side = qMin(height(), width()) * 0.8;
    double half = side / 2.0;
    double height = half * sqrt(3);
    double radius = height / 3;

    QPainterPath trianglePath; //绘制正三角形
    trianglePath.moveTo(0, -radius * 2);
    trianglePath.lineTo(-half, radius);
    trianglePath.lineTo(half, radius);
    trianglePath.closeSubpath();

    QPen pentri(QColor(214, 173, 214));
    pentri.setWidth(5);

    QBrush brushtri(QColor(199, 235, 250));

    painter.setPen(pentri);
    painter.setBrush(brushtri);

    painter.drawPath(trianglePath); //画三角

    QPen pencir(QColor(0, 255, 0));
    pencir.setWidth(5);

    QBrush brushcir(QColor(255, 128, 255));
    painter.setPen(pencir);
    painter.setBrush(brushcir);

    painter.drawEllipse(QPointF(0, 0), radius, radius); //画圆

    QPen penrect(QColor(0, 255, 0));
    penrect.setWidth(5);

    painter.setPen(penrect);
    painter.drawRect(0, -radius * 2, 0, height); //用一个宽度为0的矩形来表示直线
}

void Widget::on_dial_valueChanged(int value)
{
    angle = value;
    update(); //用一个Dial让图片跟着Dial转
}
```

2、绘制一个动态变化的正弦波

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QPainterPath>
#include <QPainter>
#include <QPixMap>
#include <Qvector>
#include <cmath>
#include <QTimerEvent>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = nullptr);
    ~Widget() override;

    void timerEvent(QTimerEvent* event) override;
    void paintEvent(QPaintEvent* event) override;

private:
    Ui::Widget *ui;

    QVector<double> data;
    double val = 0;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    startTimer(50);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::timerEvent(QTimerEvent* event)
{
    Q_UNUSED(event);

    double newValue = 200 + 100 * std::sin(val);
    data.append(newValue);
    val += 0.2;

    if(data.size() >= 50)
    {
        data.removeFirst();
    }

    update();
}

void Widget::paintEvent(QPaintEvent* event)
{
    Q_UNUSED(event);

    if(data.isEmpty())
    {
        return;
    }

    QPainter painter(this);
    painter.setRenderHint(QPainter::Antialiasing);

    //绘制辅助虚线
    painter.setPen(QPen(Qt::lightGray, 1, Qt::DashLine));
    int i;
    for(i = 1; i <= height() / 100; i++)
    {
        painter.drawLine(0, i * 100, width(), i * 100);
    }

    QPainterPath path;

    //绘制正弦曲线
    int stepx = width() / 50;
    int x = 0, y = height() - (int)data[0];
    path.moveTo(x, y);
    for(i = 1;i < data.size();i++)
    {
        x = i * stepx;
        y = height() - (int)data[i];
        path.lineTo(x, y);
    }
    QPen pen(QColor(0, 255, 0));
    pen.setWidth(5);
    pen.setCapStyle(Qt::RoundCap);   //在线段两端加上一个半圆使其圆润
    pen.setJoinStyle(Qt::RoundJoin); //在拐角处磨掉棱角使其圆润
    painter.setPen(pen);
    painter.setBrush(Qt::NoBrush);
    painter.drawPath(path);

    //绘制末端的小圆点来表示当前数值
    int lastx = (data.size() - 1) * stepx;
    int lasty = height() - (int)data.last();
    painter.setBrush(QColor(255, 0, 0));
    painter.setPen(Qt::NoPen);
    painter.drawEllipse(QPointF(lastx, lasty), 5, 5);
}
```

## QGraphicsView

`QGraphicsView` 是Qt中用于管理和交互大量的自定义2D图形对象的框架，它提供的是可视化的显示图形对象的视图控件。

`QGraphicsView` 依赖场景（ `QGraphicsScene` ）存在，而场景中存在若干图元（ `QGraphicsItem` ），打个比方就是 `View` 是是一个摄像机，只显示舞台上的部分场景，而场景就是整个舞台，图元就是舞台上的演员和布景。本质上，用户就是导演，指挥各个图元在 `View` 的范围内做出指定的动作。

一个 `Scene` 不一定只能对应一个 `View` ，可以让多个 `View` 同时显示一个 `Scene` ，每个 `View` 显示 `Scene`的不同部分。理论上， `Scene` 没有大小限制

### QGraphicsScene

这个类提供 `View` 的底层支持，用于管理大量的图元（2D图形项），它本身不具备显示功能，其必须依赖 `View` 来做显示。

#### 常用函数

`addItem` ：向场景中添加一个图元

`add` 系列函数：由于图元的种类非常多，例如椭圆图元、直线图元、路径图元、图片图元等等，这些图元都可以通过 `add` 系列函数来添加。

`selectedItems` ：图元可以被用户选中，甚至可以通过 `Ctrl` 多选图元，所以这个函数可以返回所有被用户选中的图元，其返回值类型为 `QList<QGraphicsItem*>`

### QGraphicsItem

这个类实际上是一个父类，下面继承了各式各样的 `Item` ，如 `QGraphicsPixmapItem` 、
`QGraphicsEllipseItem` 、 `QGraphicsLineItem` 等等，每种图元的用法都不尽相同，这里只介绍一些通用的函数。

#### 常用函数

`setPos` ：设置图元在场景中的位置

`setScale` ：设置图元的缩放比例

`setZValue` ：设置图元的Z轴值

`setRotation` ：设置图元的旋转角度

### QGraphicsView

这是最后展示用的窗口套件，它可以显示场景的局部（或者全部），用来完成最后的“拍摄”工作。

#### 常用函数

`setScene` ：设置场景

`setRenderHint` ：设置渲染提示（如抗锯齿等）

`scale` ：传入一个坐标值，缩放视图

`rotate` ：旋转视图

### 课堂案例

在一个 `view` 上添加若干图元，让其可以被用户拖动，并且用户可以通过按钮调整图元的图层（Z轴值）

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QGraphicsScene>
#include <QPixmap>
#include <QGraphicsPixmapItem>
#include <QGraphicsEllipseItem>
#include <QGraphicsLineItem>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE

class Widget : public QWidget
{
	Q_OBJECT
        
public:
	Widget(QWidget *parent = nullptr);
	~Widget();
    
private slots:
	void on_pushButton_up_clicked();
	void on_pushButton_down_clicked();
    
private:
	Ui::Widget *ui;
	QGraphicsScene* scene;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Widget)
	, scene(new QGraphicsScene)
{
	ui->setupUi(this);
	ui->graphicsView->setScene(scene);
	scene->setSceneRect(0, 0, 400, 500);
        
	QPixmap pm(":/1.png");
	QPixmap scaled = pm.scaled(300, 200, Qt::KeepAspectRatio,
	Qt::SmoothTransformation);
        
	QGraphicsPixmapItem* pixItem = new QGraphicsPixmapItem(scaled);
	pixItem->setPos(100, 100);
	pixItem->setFlags(QGraphicsItem::ItemIsMovable |
	QGraphicsItem::ItemIsSelectable);
	scene->addItem(pixItem);
        
	QGraphicsEllipseItem* ellItem = new QGraphicsEllipseItem(200, 200, 100,
	100);
	ellItem->setBrush(QColor(Qt::red));
	ellItem->setFlags(QGraphicsItem::ItemIsMovable |
	QGraphicsItem::ItemIsSelectable);
	scene->addItem(ellItem);
        
	QGraphicsLineItem* lineItem = new QGraphicsLineItem(100, 100, 400, 400);
	lineItem->setPen(QPen(Qt::green, 5));
	lineItem->setFlags(QGraphicsItem::ItemIsMovable |
	QGraphicsItem::ItemIsSelectable);
	scene->addItem(lineItem);
}

Widget::~Widget()
{
	delete ui;
}

void Widget::on_pushButton_up_clicked()
{
	QList<QGraphicsItem*> selected = scene->selectedItems();
	if (selected.isEmpty())
	{
		return;
	}
	QGraphicsItem* item = selected.first();
	item->setZValue(item->zValue() + 1);
}

void Widget::on_pushButton_down_clicked()
{
	QList<QGraphicsItem*> selected = scene->selectedItems();
	if (selected.isEmpty())
	{
		return;
	}
	QGraphicsItem* item = selected.first();
    item->setZValue(item->zValue() - 1);
}
```

### QGraphicsItemGroup（图元组）

这个类是一个特殊的复合项，里面可以存放很多图元，然后将这些图元视为一个整体。即图元组本身可以视为一个图元，只是内部包含别的图元而已。

#### 常用函数

在很多时候需要将很多图元组合起来时，图元组并不方便，而提升图元是另一种解决方案，即用户自定义一个图元，然后直接将他添加到场景中。

然而图元的父类 `QGraphicsItem` 并不是 `QObject` 的子类，所以直接继承这个类的话，没办法使用信号与槽机制和事件机制，此时，需要多重继承 `QGraphicsItem` 和 `QObject` ，所以Qt中提供了一个专门用于提升的类，即： `QGraphicsObject` ，这个类本身已经多重继承了 `QGraphicsItem` 和 `QObject` ，所以用户直接继承它即可。

此时，提升时要改的地方就多了两个：

```c++
#ifndef DEATHLYHOLLOWITEM_H
#define DEATHLYHOLLOWITEM_H

#include <QGraphicsObject> //1

class DeathlyHollowItem : public QGraphicsObject //2
{
	Q_OBJECT
        
public:
							//额外需要更改构造函数里的父类类型
	explicit DeathlyHollowItem(QGraphicsObject *parent = nullptr);
    
signals:
};

#endif // DEATHLYHOLLOWITEM_H
```

```c++
#include "deathlyhollowitem.h"
									//所以这里也要跟着改
DeathlyHollowItem::DeathlyHollowItem(QGraphicsObject *parent)
	: QGraphicsObject{parent} //3
{}
```

完成这些后，还有一个要做的工作， `QGraphicsObject` 中存在两个纯虚函数：

`QRectF boundingRect() const` ：这个函数返回图元占用的矩形区域，用于碰撞检测和视图更新等。

`void paint(QPainter* painter, const QStyleOptionGraphicsItem* option, QWidget* widget)` ：这个函数就是图元的样貌，表示图元应该如何绘图，通常只需要告诉 `painter` 如何绘图即可。后两个参数一般用不到。

只要这两个函数没有重写，那么你的图元类就是抽象类，无法定义对象。

### 课堂案例

用路径图元、椭圆图元、直线图元、文字图元完成一个死亡圣器图案+文字，并将他们放到一个图元组中，随后用Dial驱动它们转动。

`QGraphicsView` 中设置了样式表： `border:none;` ，隐藏了边框

方案1：使用图元组完成

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QPainter>
#include <QGraphicsScene>
#include <QGraphicsItemGroup>
#include <QGraphicsPathItem>
#include <QGraphicsEllipseItem>
#include <QPainterPath>

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE
    
class Widget : public QWidget
{
	Q_OBJECT
        
public:
	Widget(QWidget *parent = nullptr);
	~Widget();
    
	void setGroup();
    
private slots:
	void on_dial_valueChanged(int value);
    
private:
	Ui::Widget *ui;
    
	QGraphicsScene* scene;
	QGraphicsItemGroup* group;
};
#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"
Widget::Widget(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Widget)
	, scene(new QGraphicsScene)
	, group(new QGraphicsItemGroup)
{
	ui->setupUi(this);
        
	ui->graphicsView->setAttribute(Qt::WA_TransparentForMouseEvents, true);
        
	ui->graphicsView->setScene(scene);
        
	scene->setSceneRect(-200, -250, 400, 500);
        
	ui->graphicsView->setRenderHint(QPainter::Antialiasing);
	ui->graphicsView->setDragMode(QGraphicsView::NoDrag);
	//240 240 240是默认面板的背景色，配合无边框设置，可以让背景和外部融为一体
	ui->graphicsView->setBackgroundBrush(QColor(240, 240, 240));
        
	setGroup();
        
	scene->addItem(group);
}

Widget::~Widget()
{
	delete ui;
}

void Widget::setGroup()
{
	group->setPos(0, 0);
    
	double side = 400;
	double half = side / 2.0;
	double hght = half * sqrt(3);
	double radius = hght / 3;
    
	QPainterPath trianglePath;
	trianglePath.moveTo(0, -radius * 2);
	trianglePath.lineTo(-half, radius);
	trianglePath.lineTo(half, radius);
	trianglePath.closeSubpath();
    
	QGraphicsPathItem* triangle = new QGraphicsPathItem(trianglePath);
	triangle->setPen(QPen(Qt::red, 5));
	triangle->setBrush(QBrush(Qt::yellow));
	group->addToGroup(triangle);
    
	QGraphicsEllipseItem* circle = new QGraphicsEllipseItem(-radius, -radius,
	radius * 2, radius * 2);
	circle->setPen(QPen(Qt::green, 5));
	circle->setBrush(QBrush(Qt::cyan));
	group->addToGroup(circle);
    
	QGraphicsLineItem* line = new QGraphicsLineItem(0, -radius * 2, 0, radius);
	line->setPen(QPen(Qt::black, 5));
	group->addToGroup(line);
    
	QGraphicsTextItem* text = new QGraphicsTextItem("△隐形衣 ○复活石 |老魔杖");
	text->setFont(QFont("Consolas", 20));
	text->setPos(-half + 30, radius + 10);
	group->addToGroup(text);
}

void Widget::on_dial_valueChanged(int value)
{
	group->setRotation(value);
}
```

方案2：提升Item来完成

提升的Item：

```c++
#ifndef DEATHLYHOLLOWITEM_H
#define DEATHLYHOLLOWITEM_H

#include <QGraphicsObject>
#include <QPainter>

class DeathlyHollowItem : public QGraphicsObject
{
	Q_OBJECT
        
public:
	explicit DeathlyHollowItem(QGraphicsObject *parent = nullptr);
	//这两个函数是纯虚函数，必须重写
	QRectF boundingRect() const;
	void paint(QPainter* painter, const QStyleOptionGraphicsItem *option, QWidget *widget);
    
signals:
};

#endif // DEATHLYHOLLOWITEM_H
```

```c++
#include "deathlyhollowitem.h"

DeathlyHollowItem::DeathlyHollowItem(QGraphicsObject *parent)
	: QGraphicsObject{parent}
{}

QRectF DeathlyHollowItem::boundingRect() const
{
	double side = 400;
	double half = side / 2.0;
	double hght = half * sqrt(3);
	double radius = hght / 3;
	return QRectF(-half, -radius * 2, side, hght + 50);
}

void DeathlyHollowItem::paint(QPainter* painter, const QStyleOptionGraphicsItem
*option, QWidget *widget)
{
	Q_UNUSED(option);
	painter->setRenderHints(QPainter::Antialiasing |
							QPainter::TextAntialiasing |
							QPainter::SmoothPixmapTransform);
	//设置正三角形的各种属性
	double side = 400;
	double half = side / 2.0;
	double hght = half * sqrt(3);
	double radius = hght / 3;
    
	QPainterPath trianglePath; //绘制正三角形
	trianglePath.moveTo(0, -radius * 2);
	trianglePath.lineTo(-half, radius);
	trianglePath.lineTo(half, radius);
	trianglePath.closeSubpath();
    
	QPen pentri(QColor(255, 0, 0));
	pentri.setWidth(5);
    
	QBrush brushtri(QColor(255, 255, 0));
    
	painter->setPen(pentri);
	painter->setBrush(brushtri);
    
	painter->drawPath(trianglePath); //画三角
    
	QPen pencir(QColor(0, 255, 0));
	pencir.setWidth(5);
    
	QBrush brushcir(QColor(255, 128, 255));
    
	painter->setPen(pencir);
	painter->setBrush(brushcir);
	painter->drawEllipse(QPointF(0, 0), radius, radius); //画圆
    
	QPen penrect(QColor(0, 0, 0));
	penrect.setWidth(5);
    
	painter->setPen(penrect);
	painter->drawRect(0, -radius * 2, 0, hght); //用一个宽度为0的矩形来表示直线
    
	penrect.setColor(QColor(0, 0, 0));
	painter->setPen(penrect);
    
	QFont font("Consolas", 20);
	painter->setFont(font);
	QRect textRect(-300, radius + 10, 600, 40); //设置文字矩形框
	painter->drawText(textRect, Qt::AlignCenter, "△隐形衣 ○复活石 |老魔杖");
}
```

主面板：

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QGraphicsScene>
#include "deathlyhollowitem.h"

QT_BEGIN_NAMESPACE
namespace Ui {
class Widget;
}
QT_END_NAMESPACE
    
class Widget : public QWidget
{
	Q_OBJECT
        
public:
	Widget(QWidget *parent = nullptr);
	~Widget();
    
private slots:
	void on_dial_valueChanged(int value);
    
private:
	Ui::Widget *ui;
    
	QGraphicsScene* scene;
	DeathlyHollowItem* item;
};

#endif // WIDGET_H
```

```c++
#include "widget.h"
#include "ui_widget.h"
Widget::Widget(QWidget *parent)
	: QWidget(parent)
	, ui(new Ui::Widget)
	, scene(new QGraphicsScene)
	, item(new DeathlyHollowItem)
{
	ui->setupUi(this);
        
	scene->setSceneRect(-200, -250, 400, 500);
	ui->graphicsView->setAttribute(Qt::WA_TransparentForMouseEvents, true);
	ui->graphicsView->setScene(scene);
        
	ui->graphicsView->setRenderHint(QPainter::Antialiasing);
	ui->graphicsView->setDragMode(QGraphicsView::NoDrag);
	ui->graphicsView->setBackgroundBrush(QColor(240, 240, 240));
        
	item->setPos(0, 0); //直接添加自定义的图元即可
	scene->addItem(item);
}

Widget::~Widget()
{
	delete ui;
}

void Widget::on_dial_valueChanged(int value)
{
	item->setRotation(value);
}
```

