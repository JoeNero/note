# 1 linux 下注意点

找不到mysql.h文件

sudo apt-get install libmysqlclient-dev

编译太慢,更改

-j 4       4为线程 <= cpu的线程

缺少:-1: error: 找不到 -lGL
sudo apt-get install libgl1-mesa-dev

使用以下模块需要 实现下载这个 qt会自动查找

sudo apt-get install qtmultimedia5-dev

如果不行则加:

sudo apt-get install libpul se-dev

视频相关控件需要安装如下内容

```
apt-get install libpulse-dev
```

```
QT       += multimedia
```
libusbzhic库
```shell
sudo apt-get install libusb-dev
sudo apt-get install libusb-1.0-0-dev
```

# 2 布局管理

## 2.1 QGridLayout网格布局

```cpp
layout->setRowStretch(int row, int stretch);//设置行比例系数
layout->setColumnStretch(int column, int stretch);//设置列比例系数
```

布局示意如下:

```cpp
layout->addWidget(&TestBtn1, 0, 0);//往网格的不同坐标添加不同的组件
layout->addWidget(&TestBtn2, 0, 1);
layout->addWidget(&TestBtn3, 1, 0);
layout->addWidget(&TestBtn4, 1, 1);
```

# 3 控件：

## QLable

显示数字

setText ( const QString & )setText参数必须是QString类型才可以 你的变量如果是整形，可以直接转换，比如QString()::number( int num).

```c++
ui->labelTime->setText(QString::number(times));
```

## QLCDNumber：

lcd是直接通过方法value来获取当前显示的值，通过方法display来显示

QLCDNumber有以下几种模式：

[setHexMode](https://blog.csdn.net/xuancailinggan/article/details/qlcdnumber.html#setHexMode)()-十六进制

[setDecMode](https://blog.csdn.net/xuancailinggan/article/details/qlcdnumber.html#setDecMode)()-十进制

[setOctMode](https://blog.csdn.net/xuancailinggan/article/details/qlcdnumber.html#setOctMode)()-八进制

[setBinMode](https://blog.csdn.net/xuancailinggan/article/details/qlcdnumber.html#setBinMode)()-二进制

lcdNum->setDecMode();

## QTimer

Q_DECL_OVERRIDE也就是c++的override

```
# define Q_DECL_OVERRIDE override
```

在重写虚函数时会用到，

作用是防止写错虚函数：

```
void keyPressEvent(QKeyEvent *event) Q_DECL_OVERRIDE;
```



```cpp
//头文件在QTimer文件下
#include <QTimer>
...
QTimer *m_pTimer;
m_pTimer = new QTimer(this);
// 设置超时间隔
m_pTimer->setInterval(100);
...
connect(m_pTimer, SIGNAL(timeout()), this, SLOT(updateProgress()));
m_pTimer->start(1000);//这一步会覆盖之前设置的时间间隔
```

start()之后，每秒都会调用update()

可以通过设置setSingleShot(true)来让定时器只执行一次。也可以使用静态函数QTimer::singleShot()：

```
QTimer::singleShot(200, this, SLOT(updateCaption()));
```

QTimer和lable配合显示系统时间

头文件

```c++
#include <QTimer>
```

构造函数

```c++
//日期/时间显示
QTimer *timer = new QTimer(this);
connect(timer,SIGNAL(timeout()),this,SLOT(timerUpdate()));
timer->start(1000);
```

定义成员函数timerUpdate()实现用户界面显示时间：

```c++
void userwindow::timerUpdate()
{
    QDateTime time = QDateTime::currentDateTime();

    QString str = time.toString("yyyy-MM-dd hh:mm:ss dddd");

    ui->dateTime->setText(str);
}
```

# 4 QChart

.pro文件添加模块

```c++
QT +=charts
```

```cpp
//使用charts模板需要加入命名控件或者宏
using namespace Qtcharts
或者一个宏 QT_CHARTS_USE_NAMESPACE
//头文件
#include <QChart>
#include <QChartView>
...
QChart *chart;
QChartView *chartView;
```

```cpp
//坐标系
#include <QValueAxis>
...
QValueAxis *axisX;
QValueAxis *axisY;
...
axisX->setRange(0, 20);    		//设置范围
axisX->setLabelFormat("%u");   	//设置刻度的格式 y轴同理
axisX->setTitleText("X");           		//设置描述
axisY->setTitleText("Y");
/************************************
    %u 无符号十进制整数
    %s 字符串
    %A 浮点数、十六进制数字和p-记法
    %c 一个字符
    %d 有符号十进制整数
    %e 浮点数、e-记数法
    %E 浮点数、E-记数法
    %f 浮点数、十进制记数法
    %g 根据数值不同自动选择％f或％e．
    %G 根据数值不同自动选择％f或％e.
    %i 有符号十进制数（与％d相同）
    %o 无符号八进制整数
    %p 指针
    %s 字符串
    %x/%X 使用十六进制数字0f的无符号十六进制整数
****************************************/
//标记的个数
    axisX->setTickCount(11);           
    axisY->setTickCount(11);
//次标记的个数
	axisX->setMinorTickCount(1);        //设置每个大格里面小刻度线的数目
//    axisY->setMinorTickCount(1);
	chart->createDefaultAxes(); //建立默认坐标轴，不需要QValueAxis 默认是4x4大格子
//	chart->axisY()->setRange(0, 10);//默认坐标轴限定范围
chart->addAxis(axisX, Qt::AlignBottom); //下：Qt::AlignBottom  上：Qt::AlignTop
chart->addAxis(axisY, Qt::AlignLeft);   //左：Qt::AlignLeft    右：Qt::AlignRight

```

```cpp
    chart->setTitle("曲线图实例");
    chart->setAnimationOptions(QChart::SeriesAnimations);//设置曲线动画模式
    chart->legend()->hide(); 							//隐藏图例
    chart->addSeries(splineSeries);						//输入数据
    chart->setAxisX(axisX, splineSeries);
    chart->setAxisY(axisY, splineSeries);
```

需要将chart添加到chartView

```cpp
    chartView->show();
    chartView->setChart(chart);
    chartView->setRenderHint(QPainter::Antialiasing);//防止图形走样
```

## 曲线图

QSplineSeries

```cpp
#include <QSplineSeries>
```

```cpp
splineSeries = new QSplineSeries;
//添加数据的两周方式
splineSeries->append(10, 5);
*splineSeries << QPointF(11, 1) << QPointF(13, 3) << QPointF(17, 6)<< QPointF(20, 2);
//源数据添加到图表上
chart->addSeries(splineSeries);//输入数据
chart->setAxisX(axisX, splineSeries);
chart->setAxisY(axisY, splineSeries);
```

## 柱状图

先包含头文件

```cpp
#include <QBarSet> 
```

QBarSet类表示条形图中的一组条形。
一个bar集包含每个类别的一个数据值。
假设集合的第一个值属于第一个类别，
第二个属于第二个类别，依此类推。
如果集合的值小于类别的值，则假设缺失值位于集合的末尾。对于位于集合中间的缺失值，则使用0的数值。
没有显示零值集的标签。

```
#include <QBarSeries>
```

QBarSeries类表示的是柱状图数据，需要将相应的QBarSet添加进来

柱状图关系示意如下:

## 饼状图

```cpp
#include <QCharts/QPieSeries>
```

QPieSeries是一块饼图

```cpp
#include <QCharts/QPieSlice>
```

QPieSlice是饼图上的碎片

# 5 QCustomPlot

```
QT       += core gui printsupport
```

选择项目的.pro文件，添加printsupport，如图，QCustomPlot包含了一些打印的东西，如果没有这一步，程序会报错

基类为QWidget：提升为`QCustomPlot`

这里强调一下：Qt提升控件时，通常提升的类名称中，每个单词的首字母必须大写与Qt控件命名规则保持一致，各种第三方控件都采用这种命名格式，否则无法识别，如这里必须写成`QCustomPlot`而不能写成`Qcustomplot`或`qcustomplot`

# 动态库的调用

## liunx

详细图例见dll文件

创建c++库，添加代码生成对应的.so文件

在debug文件下将.so文件后缀的复制到新建工程的debug文件下



并将相应的头文件添加到目标工程中

在.pro里面添加 如下 格式 -L./lib -l(文件名)  因为直接放在debug文件下所以直接 在该文件夹下找

```
LIBS +=  -L -llibdll
```
# Linux 下opencv的搭建

在.pro文件中添加如下内容,根据个人情况,就是你opencv的安裝路勁

```
INCLUDEPATH += /usr/local/include \
                /usr/local/include/opencv4 \

LIBS += /usr/local/lib/libopencv*
```

有的人是

```
INCLUDEPATH += /usr/local/include \
                /usr/local/include/opencv \
                /usr/local/include/opencv2

LIBS += /usr/local/lib/lib*
```

出现这个错误，只需要在对应的文件中添加头文件
#include <opencv2/highgui/highgui_c.h>

```shell
/home/xtt/prj/Qt/pack/OpenNCC_View/OpenNCC_View/widget.cpp:330: error: ~~‘cvGetWindowHandle’~~ was not declared in this scope
                 if (!cvGetWindowHandle("OpenNCC"))
                      ^~~~~~~~~~~~~~~~~
```





# Linux 下qt的打包

将release 版本下的hi可执行文件拷到你新建的bin文件夹下

新建一个打包的脚本pack.sh

```shell
#!/bin/sh
exe="OpenNCC_View" #需发布的程序名称
des="/home/xtt/Qt/OpenNCC_View/bin" #新建目录的完整路径
deplist=$(ldd $exe|awk '{if (match($3,"/")){printf("%s "),$3}}')
cp $deplist $des
```

运行脚本 sh pack.sh会在该文件夹下添加一些.so动态库

然后编写一个部署脚本

文件名和你的项目名字一致，这里的项目名字是OpenNCC_View，所以部署脚本是OpenNCC_View.sh

以下是该脚本的内容

```shell
#!/bin/sh  
appname=`basename $0 | sed s,\.sh$,,`  
dirname=`dirname $0`  
tmp="${dirname#?}"  
if [ "${dirname%$tmp}" != "/" ]; then  
dirname=$PWD/$dirname  
fi  
LD_LIBRARY_PATH=$dirname  
export LD_LIBRARY_PATH  
$dirname/$appname "$@"

```

用軟件打包

# Window下打包程序
Win+r 打开dos命令 cmd

输入命令：

cd /d H:\QT\Test\release

必须要加/d 不然没办法进入目录

Qt Quick Application版本:

windeployqt  Test.exe

Test.exe 为release版本的exe