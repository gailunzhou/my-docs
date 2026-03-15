# QT

## 概述：

**面试题：**QT是一个跨平台的客户端GUI技术，具有大量的库和工具

gui：图形用户界面

**面试题：**

QT的优点：

- 开源的C++应用程序框架
- 具有丰富的文档，接口简单
- 开源社区活跃
- 可跨平台，几乎支持所有平台

### QT工具集

- qmake：用于生成Makefile，和cmake相同
- moc：元对象编译器，是处理QT的C++扩展的程序

## QT下载

中文社区： https://www.qter.org/

## QT文件

### XXX.pro工程配置文件

```shell
# 添加QT的三大模块
QT       += core gui

# core 核心模块
# gui  界面模块
# widgets窗体模块

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

# 配置c++版本
CONFIG += c++11

# You can make your code fail to compile if it uses deprecated APIs.
# In order to do so, uncomment the following line.
#DEFINES += QT_DISABLE_DEPRECATED_BEFORE=0x060000    # disables all the APIs deprecated before Qt 6.0.0

#源文件
SOURCES += \
    main.cpp \
    mainwindow.cpp
#头文件
HEADERS += \
    mainwindow.h
#form，里面就是一些界面文件
FORMS += \
    mainwindow.ui

# Default rules for deployment.
# 部署target path，做了平台差异性处理
qnx: target.path = /tmp/$${TARGET}/bin
else: unix:!android: target.path = /opt/$${TARGET}/bin
!isEmpty(target.path): INSTALLS += target

```

### XXX.h头文件配置

```c++
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

//添加QMainWindow这个类的头文件
#include <QMainWindow>

// 开始定义一个命名空间,并且把MainWindow放入这个命名空间
QT_BEGIN_NAMESPACE
namespace Ui { class MainWindow; }
QT_END_NAMESPACE


//定义一个类继承QMainWindow
class MainWindow : public QMainWindow
{
    //QT 中的信号与槽的宏定义
    Q_OBJECT
//公有成员
public:
    //构造函数
    MainWindow(QWidget *parent = nullptr);
    //析构函数
    ~MainWindow();

//私有成员
private:
    Ui::MainWindow *ui;
};
#endif // MAINWINDOW_H
```

说明：

构造函数里面的MainWindow(QWidget *parent = nullptr)

是带默认参数的有参构造

这里的parent = nullptr 是QT中的父对象机制

parent代表父窗口/父控件指针

当给一个控件指定parent时，会被挂在到父对象的对象树上。

当父对象销毁时会自动递归销毁子对象

如果parent = nullptr 说明该控件是顶层窗口，需要手动管理生命周期

### XXX.cpp文件

```c++
#include "mainwindow.h"

//添加ui_mainwindow.h 界面设计头文件，所有的界面设计都在该文件中进行
#include "ui_mainwindow.h" //不需要用户去管，这个文件是自动生成的

//构造函数
MainWindow::MainWindow(QWidget *parent): QMainWindow(parent),ui(new Ui::MainWindow)
{
    //调用 设置UI 的函数
    ui->setupUi(this);  //void setupUi(QMainWindow *MainWindow)
}

//释放分配的空间  ui(new Ui::MainWindow)
MainWindow::~MainWindow()
{
    delete ui;
}

```

### .ui图形界面文件中的main.cpp文件

```cpp
//添加用户自定义窗体头文件
#include "mainwindow.h"
//添加QT 应用类
#include <QApplication>
//主函数
int main(int argc, char *argv[])
{
    //创建QT 应用对象
    QApplication a(argc, argv);
    //在应用中创建一个窗体
    MainWindow w;
    //显示窗体
    w.show();

    //执行应用
    return a.exec();  //事件循环一直轮询这个应用

    //注意：在窗体之前必须要有 应用， 所有窗体都是在应用上的
}
```

**注意：**应用对象和窗体的创建先后顺序是固定的，必须先创建应用对象，再创建窗体

### QT是如何实现跨平台的

**面试题：**

- 对应平台要安装相应的QT库
- 将代码通过交叉编译生成对应平台的版本

## 设计出一个登录窗口

### 方式1：使用QTDesigner通过拖动控件的方式实现

略

### 方式2：使用原生C++代码的方式实现

```cpp
//使用原生C++来绘制界面
#include <iostream>
//添加窗口应用头文件
#include <QApplication>
//添加窗口头文件
#include <QMainWindow>
//添加label头文件
#include <QLabel>
//添加LineEdit头文件
#include <QLineEdit>
//添加PushButton头文件
#include <QPushButton>

using namespace std;

int main(int argc ,char* argv[])
{
    //先实例化应用对象
    QApplication qa(argc ,argv);
    //再实例化窗口对象
    QMainWindow mywindow;

    //设置窗口标题
    mywindow.setWindowTitle("登录窗口");
    //设置窗口位置和大小
    mywindow.setGeometry(600 ,250 ,800 ,550);
    //设置窗口的背景颜色
    mywindow.setStyleSheet("background-color:white;");

    //添加窗口中主题标题并设置父级窗口
    QLabel *title = new QLabel(&mywindow);
    //设置位置大小
    title->setGeometry(0 ,70 ,800 ,80);
    //设置主题内容
    title->setText("欢迎登录系统");
    //设置字体样式
    title->setStyleSheet("font-size:20pt;font-family:微软雅黑");
    //设置字体对齐方式
    title->setAlignment(Qt::AlignHCenter);

    //添加账号标签
    QLabel *account = new QLabel(&mywindow);
    //设置位置大小
    account->setGeometry(240 ,180 ,80 ,40);
    //设置内容
    account->setText("账号：");
    //设置字体样式
    account->setStyleSheet("font-size:16pt;font-family:微软雅黑");
    //默认为左对齐

    //添加账号输入框
    QLineEdit *account_edit = new QLineEdit(&mywindow);
    //设置输入框大小
    account_edit->setGeometry(330 ,185 ,235 ,35);

    //添加密码标签
    QLabel *pwd = new QLabel(&mywindow);
    //设置位置大小
    pwd->setGeometry(240 ,250 ,80 ,40);
    //设置内容
    pwd->setText("密码：");
    //设置字体样式
    pwd->setStyleSheet("font-size:16pt;font-family:微软雅黑");

    //添加密码输入框
    QLineEdit *pwd_edit = new QLineEdit(&mywindow);
    //设置位置大小
    pwd_edit->setGeometry(330 ,255 ,235 ,35);

    //添加登录按钮
    QPushButton *login_button = new QPushButton(&mywindow);
    //设置位置大小
    login_button->setGeometry(240 ,330 ,100 ,50);
    //设置内容
    login_button->setText("登录");
    //设置字体样式
    login_button->setStyleSheet("font-size:16pt;font-family:微软雅黑;background-color:pink");

    //添加注册按钮
    QPushButton *register_button = new QPushButton(&mywindow);
    //设置位置大小
    register_button->setGeometry(470 ,330 ,100 ,50);
    //设置内容
    register_button->setText("注册");
    //设置字体样式
    register_button->setStyleSheet("font-size:16pt;font-family:微软雅黑;background-color:pink");

    //显示窗口
    mywindow.show();
    //事件循环
    return qa.exec();
}

```

## 核心机制

qt core就是qt的核心部分

QObject类是比较核心的基类，大部分对应都要直接或间接继承QObject

### QString类

属于core模块

相关函数

```cpp
 append:字符串拼接
 preappend:在前面拼接
 arg： 参数  Qstring("xxx%1yyy%2").arg(1).arg(2);
 contains：包含
 isNull: 判Null
 isEmpty:判Empty
 startWith:以什么开始
 endWith:以什么结束
 indexOf:在那个位置
 replace:替换
 split:拆分
```

### QByteArray类

qt中大部分数据都是QByteArray类型

### QStirng和QByteArray类型相互转换

- QString转QByteArray：QString.toUtf8();
- QByteArray转QString：QString(QByteArray);有参构造

### 集合

QT本来就是用C++编写的，因此QT也有C++中的所有的集合，例如vector、list、set/multiset、map/multimap等

QT中还有专门用于QString类型的集合**QStringList**

#### 信号与槽函数

信号和槽函数是用于QT中对象之间通信，当特定事件（例如点击）发生时会自动发出信号，信号会被槽函数接收并响应，执行相应的业务逻辑

QT中控件的信号可以通过在设计师页面右键控件选择“转到槽”去使用预定义的信号，也可以通过自定义信号和自定义槽函数来处理一些特殊的信号，完成特殊的逻辑实现

**信号：**由QObject或其子类的对象发出的消息，信号本身不包含任何功能实现，而是作为一个通知机制存在。在C++中，它们通常被声明为类的成员函数，但不需要实现

**槽：**用来响应信号的方法，可以是任何可调用的代码段，通常是成员函数

**槽和信号之间是多对多的关系**

### 如何自定义信号和槽函数

- 自定义信号：在控件的类定义当中，使用关键字**signal：**在下方写函数声明即可，**不需要实现**
- 自定义槽函数：在控件的类定义当中，使用 访问控制修饰符 + 关键字**slot：**然后在下方写函数声明即可，**需要实现**

### 信号和槽函数的连接机制

信号和槽函数通过connect函数进行绑定

**面试题：**

connect函数的五个参数：

1. 发送信号的**对象**
2. 发送的什么信号
3. 接收信号的**对象**
4. 接收信号对象所使用的槽函数
5. 连接方式：
   1. 自动连接（默认）如果发送信号的对象和接收信号的对象在同一个线程就是直接连接，如果不在同一个线程就是队列连接
   2. 直接连接 在发送信号的线程中执行槽函数
   3. 队列连接 信号触发后不会立即执行槽函数，而是放入接收者线程的事件循环队列中，等待事件循环处理时再执行
   4. 阻塞队列连接 和队列连接类似，但是会阻塞发送信号的线程，直到处理信号对象所在线程执行完槽函数
   5. 唯一连接 本质是一个修饰符，是确保信号和槽之间只建立一次连接，后续再使用connect则无效，不是全局一对一，只针对当前所修饰的信号和槽

**面试题:**

信号和槽的连接机制

1. QT会为每个QObject类的派生类生成 “元对象” ，用于存储信号和槽的原信息（名称，参数，索引等）
2. 使用connect函数绑定信号和槽时，会将信号和槽在一个map中注册绑定信息
3. 当通过emit函数发送了信号时，QT会遍历那个map，找到所有绑定的槽函数，根据连接类型决定槽函数的执行时间和线程

### 连接信号的操作

以按钮控件点击为例

#### 方式1：QT4写法（仅了解）

connect(ui->pushButton ,SIGNAL(clicked())) ,this ,SLOT(setLable()));

#### 方式2：QT5写法

connect(ui->pushButton ,&QPushButton::clicked ,this ,&MainWindow::setLable);

### 事件

事件是用户和应用软件间产生的一个交互操作，由用户操作产生或者系统内部产生，通过事件循环对事件进行处理，事件也可以用来在对象间进行信息交互

### QT中事件的处理流程

**重要**

1. 程序开始时会创建一个事件循环实例，一般都是QT应用对象调用exec()函数启动事件循环，事件循环会监听事件队列
2. 当有新事件产生时，事件会被封装成QEvent类的一个实例并加入到事件队列中
3. 根据事件类型将其发送给合适的对象
4. 在到达对象之前会先检查事件是否安装了事件过滤器，如果有则进入事件过滤器
   1. 事件过滤器执行完，返回true说明在过滤器内已处理完毕，不用继续传递给对象
   2. 事件过滤器执行完，返回false说明过滤器内还没处理完，需要传递给对象，也可以向上传递给父类决定是否需要传递给对象
5. 如果事件过滤器没有处理完毕传递给了对象，则检查对象中是否重写了事件处理函数
   1. 重写了事件处理函数：执行处理函数
   2. 没重写处理函数：则将事件向上传递给父类由父类处理（一般是QObject类的event函数）

### 重写事件处理函数

事件处理一般是重写已有控件的事件处理，因此一般都是在自定义控件类的情况下需要使用

这里简单说一下怎么自定义控件：自定义一个类去继承已有的控件类，比如自定义一个按键类，就要继承QPushButton类

重写事件处理函数即重写XXXXEvent()函数，以鼠标点击函数为例就是重写mousePressEvent()该函数是QObject类中的一个虚函数，可以被派生类重写

头文件：

```c++
#ifndef MYPUSHBUTTON_H
#define MYPUSHBUTTON_H

#include <QPushButton>

class MyPushButton : public QPushButton
{
    Q_OBJECT
public:
    explicit MyPushButton(QWidget *parent = nullptr);

    //重写鼠标点击事件函数
    void mousePressEvent(QMouseEvent *e);

signals:

};

#endif // MYPUSHBUTTON_H

```

源文件：

```c++
#include "mypushbutton.h"
#include <QDebug>
//自定义控件：自定义一个类去继承QT当中的控件类
MyPushButton::MyPushButton(QWidget *parent) : QPushButton(parent)
{

}
//重写鼠标点击事件处理函数
void MyPushButton::mousePressEvent(QMouseEvent *e)
{
    qDebug() << "重写鼠标点击函数";
    //手动发送信号触发槽函数
    emit clicked();
    //或者向上使用父类中的处理函数
    QPushButton::mousePressEvent(e);
}

```

编写好以后需要在设计师页面中将控件提升为自己定义的控件

### 事件过滤器

事件过滤器会在事件到达目标对象之前将事件进行拦截，就比如打游戏时按WASD四个键会拦截，防止进行输入

#### 如何编写事件过滤器

自定义事件过滤器类，**继承QObject类，重写eventFilter函数**

头文件：

```c++
#ifndef MYEVENTFILTER_H
#define MYEVENTFILTER_H

#include <QObject>
// 若要给对象安装事件过滤器，需要自定义过滤器类继承QObject，并重写EventFilter函数
#include <QEvent>

class MyEventFilter : public QObject
{
    Q_OBJECT
public:
    explicit MyEventFilter(QObject *parent = nullptr);

    //重写EventFilter函数
    bool eventFilter(QObject *watched, QEvent *event);

signals:

};

#endif // MYEVENTFILTER_H

```

源文件：

```c++
#include "myeventfilter.h"
#include <QDebug>

MyEventFilter::MyEventFilter(QObject *parent) : QObject(parent)
{

}
//重写事件过滤器函数，参数1：目标对象，参数2：当前事件
bool MyEventFilter::eventFilter(QObject *watched, QEvent *event)
{
    //qDebug() << watched->objectName() << "\t" << event->type();
    //判断处理特定事件
    if(event->type() == QEvent::MouseButtonPress)
    {
        qDebug() << "事件过滤器";
    }

    //返回值
    //return false;
    //也可以向上使用父类来判断是否需要返回给对象
    return QObject::eventFilter(watched ,event);
}

```

创建好事件过滤器后需要安装到指定控件时

```cpp
指定控件->installEventFilter(事件过滤器名);
```

## QT中的线程和进程

**面试题：**什么是线程，什么是进程

- 进程：一个应用程序被操作系统拉起来加载到内存后从开始执行到执行结束的过程
- 线程：线程是进程中的一个实体，是被系统独立分配和调度的基本单位，线程是CPU可执行调度的最小单位

**面试题：**进程和线程的区别

- 进程要独立占用系统资源，而同一进程的线程之间是共享资源的，进程本身并不能获取CPU时间片，只有线程可以

### QT中如何操作进程

进程属于Qprocess类

1. 实例化一个QProcess类对象process
2. 使用成员函数start()来执行业务逻辑，例如process.start("ping www.baidu.com");
3. 使用成员函数waitForFinished()来等待进程执行完毕，函数参数是执行的超时时间，返回值是bool类型
   1. 返回true说明已有返回值
      1. 获取返回值使用成员函数readAllStandardOutput();该函数返回值是QByteArray类型
      2. 如果返回值中存在中文需要进行转码：
         1. 如何进行转码：需要使用QTextCodec类来进行转码
            1. 先实例化一个QTextCodec类对象
            2. 使用成员函数codecForName("system");获取系统编码`QTextCodec *textcodec = QTextCodec::codecForName("system");`
            3. 再将返回值转换为unicode类型 `QString res = textcodec->toUnicode(data);`
   2. 返回false说明无返回值
      1. 使用readAllStandardError()；函数获取错误详情

### 进程之间的通信

两个进程之间进行数据通信

#### 实现方式

##### 两个进程在同一个电脑 

- 使用一个中间文件
- 使用共享内存
- 使用数据库

##### 两个进程不在同一个电脑

- 使用网络

### QT中如何操作线程

QT中是使用QThread类来管理线程的

**关于主线程：**

- 运行一个程序时，会为QT创建一个默认的主线程
- QT中只有主线程可以操作控件，子线程无法操作控件

**为什么要使用线程：**

- 进行耗时操作时，如果是在主线程，会影响用户操作，产生卡界面的效果
- 使用线程可以提高程序性能，现在电脑一搬是多CPU，多线程并行处理事物，可以大大提升程序性能

#### 如何操作线程

方式1：自定义类继承QThread类并重写run方法实现业务逻辑，在主线程中new一个自定义类对象，使用start函数创建子线程并自动运行run函数中的逻辑

方式2：movetoThread函数，将**继承于QObject类的自定义类对象**移动到指定的子线程（子线程需要提前创建）

方式3：

线程池

- 为什么要使用线程池？
  - 可以避免频繁创建和销毁线程，节约时间和资源
  - 防止创建过多线程导致内存溢出和程序崩溃
  - 线程的生命周期由线程池进行管理
- 使用方式：
  - QThreadpool类和QRunnable类
    - 每一个QT应用一开始就自带一个线程池对象，可以通过globalinstance()来进行访问
    - 使用线程池中的线程执行业务时，start函数中应该填入**继承了QRunnable类的自定义类**，这个自定义类中去重写run函数

方式4：使用QConcurrentPool类

- 优点：
  - 自动根据可用处理器内核的数量调整所使用的线程数
  - 不用考虑共享数据的保护问题，自动处理
  - 可以获取子线程的返回结果
- 使用：
  - 普通函数即可成为业务逻辑，无需重写run函数
  - 需要在QT的pro文件中导入concurrent才可以使用

**面试题**

线程池的最大线程数如何设置？

- CPU密集型（需要大量计算）：线程数一般为电脑的CPU核数量
- IO密集型（需要大量输入输出）：一般为电脑CPU核心数*2~4

## QT中如何进行页面跳转

需求：按下按钮后进行页面跳转，并隐藏当前主页面，且新页面可以返回主页面

方式：

创建的新页面也是一个继承了QMainWindow的自定义类，将这个类包含到主页面类中作为主页面类的私有成员，在实例化主页面时，也要实例化子页面，并且子页面的父类为主页面

主页面头文件

```c++
//需要包含子页面的头文件用于添加相关对象
#include <subWindow.h>
//主页面类声明
class MainWindow:
{
public:
    ...
private:
    ...
    //将子页面作为私有成员包含
    SubWindow *subwin;
};
```

主页面源文件（实例化部分）

```c++
void MainWindow::Mainwindow(void)
{
    ...//主页面相关的实例化
    //实例化子页面并设置父页面为主页面
    this->subwin = new SubWindow(this);//因为在类中，通过this指针指定为主页面
}
```

主页面中的按钮点击槽函数

```c++
void MainWindow::pushbutton_on_clicked(void)
{
    this->hide();//隐藏主页面
    this->subwin->show();//显示子页面
}
```

要想子页面中点击一下按钮也返回到主页面

子页面中的按钮点击槽函数

```c++
void SubWindow::pushbutton_on_clicked(void)
{
    this->hide();//隐藏子页面
    this->getParentWidget()->show();//显示主页面
}
```

## QT中实时显示时间，日期

主要使用QTimer定时器类、QDateTime类，前者用于显示实时秒数，后者获取日期和时间

### 日期获取

```c++
QDateTime::currentDateTime().toString("HH:mm:ss");
```

`QDateTime::currentDateTime()`是获取当前日期和时间的函数，这里使用toString()函数转换为QString类型数据

- HH：24小时制
- mm：分钟
- ss：秒数

### 实时显示

定时器在页面实例化中就创建，并且设置间隔为1s

绑定定时器超时信号 `QTimer::timeout`和显示时间槽函数

启动定时器`timer.start()`

## QT中如何实现API请求

需求：有些数据需要从第三方获取，这就需要使用API请求

主要使用QNetworkAccessManager类，**需要在pro配置文件中添加模块network**

将QNetworkAccessManager类对象作为页面的私有化成员，在实例化页面时就为manager申请内存

### 发送请求

使用get成员函数：

- 函数的参数是QUrl类型，是要请求的具体网址
- 函数的返回值是QNetworkReply类型数据

绑定发送请求完毕信号`QNetworkAccessManager::finished`和类中**自定义的处理返回结果槽函数**

### 处理请求

主要使用**QJsonDocument类、QJsonObject类、QJsonArray类**

一般都是Json解析（API返回的一般都是Json文件）

具体步骤：

1. 先用QByteArray类对象来接收返回结果
2. 使用**QJsonDocument**类将接收到的结果转换为Json文档类型：`QJsonDocument  document =  QJsonDocument::fromJson(reply);`
3. 处理API时需要根据第三方所提供的API文档进行操作：
   1. 先判断转换后的Json文档是否是一个Json对象：`document.isObject()`返回值是bool类型
   2. 是对象则转换为对象（一般情况下是）：`QJsonObject api_obj = document.object();`
   3. 使用`value()`函数去获取指定数据：例如要获取data这个数据`api_obj.value("data");`
   4. 假设文档中data数据是一个Json对象数组，因此需要用一个数组来接收：`QJsonArray arr = api_obj.value("data");`
   5. Json对象组成的数组中每一个元素都是一个Json对象，使用for循环遍历数组，将每一个元素都转换为Json对象：`for(int i = 0; i < arr.size() ; i ++) {    QJsonObject  current_object = arr.at(i).toObject();    ... ... }`

## QT中如何实现和数据库连接

QT中主要是Sqlite数据库，对比MySql更轻量级

方式：

**先在pro配置文件中添加`sql`模块**

### 连接数据库

使用QSqlDataBase类中的成员函数`addDataBase`参数分别是：

- "QSQLITE"
- "连接名"

使用成员函数setDatabaseName来指定要连接的数据库，参数是数据库路径：`setDatabaseName("数据库路径");`

使用open函数来打开数据库（bool类型）：

- 打开成功，获取数据

- 打开失败，添加**QSqlError**类来获取错误信息：数据库名.LastError.text()

### 操作数据库

主要使用**QSqlQuery**类来执行sql语句

sql语句语法就是MySQL中所用的语法，执行需要先创建QSqlQuery类对象，并使用成员函数`exec()`来执行，参数就是要执行的sql语句