# 1 基础操作





这些错误是说add-apt-repository的远程仓库没有这个文件，这个IP也是ping不通的。

添加的仓库保存在 /etc/apt/sources.list.d目录下。删除对应的错误仓库文件即可



阿里镜像源

 deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
 deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

 deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
 deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

 deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
 deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

 deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
 deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

 deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
 deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse



运行权限chmod +x qt-opensource-linux-x64-android-5.8.0.run

```shell
cd /etc/apt/
sudo cp sources.list sources.list.bak
sudo cp sources.list sources.list.bak #更换镜像源
```

ctrl + alt +t :	打开终端

ctrl + d 	:	关闭终端

ctrl + c	:	终止进程

win +e 	: 	打开计算机

ctrl+s	:	阻断向终端输出

ctrl+q	:	恢复向终端输出	

文件基本操作

-r 就是向下递归，不管有多少级目录，一并删除

 -f 就是直接强行删除，不作任何提示的意思

```shell
mkdir test1 	#创建文件夹
rm -f main2.cpp #直接删除文件
```
sudo apt-get autoclean                清理旧版本的软件缓存 sudo apt-get clean                    清理所有软件缓存 

sudo apt-get autoremove             删除系统不再使用的孤立软件 这三个命令主要清理升级缓存以及无用包的。

cpu:

查看物理cpu个数

```shell
cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l
```

查看逻辑cpu个数

```shell
cat /proc/cpuinfo |grep "processor"|wc -l
```

查看cpu是几核的

```shell
cat /proc/cpuinfo |grep "cores"|uniq
```

查看cpu的主频

```shell
cat /proc/cpuinfo |grep MHz|uniq
```

查看操作系统的内核

```shell
uname -a
```

查看

```shell
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
#8  Intel(R) Core(TM) i5-8300H CPU @ 2.30GHz
cat /proc/cpuinfo | grep physical | uniq -c
getconf LONG_BIT
#输出64 当前工作在64位
```

find命令如果出现   umount: /run/user/1000/gvfs: 权限不够

直接下面命令卸载

```
umount /run/user/1000/gvfs    // 卸载该文件
rm -rf /run/user/1000/gvfs    // 删除该文件
```


meld 比较工具

```shell
meld main1.cpp main2.cpp
```

解决：Gtk-Message: Failed to load module "canberra-gtk-module"
```shell
 sudo apt-get install libcanberra-gtk-module
```

cp 复制操作

mv指令  mv before.txt after.txt

## 1.1 vim 

按i进入插入模式

命令模式直接输入

语法高亮

```
syntax enable
```

显示行数

```
set nu
```

突出当前行

```
set cursorline  
```

 如何把另外一个文件的内容拷贝到你文件内容下

例子:把main.cpp的内容拷贝到光标所在的位置

```
:r!cat main.cpp
```

按下数字 0 光标移动到行首

ctrl + u :向上翻页

ctrl + d :向下翻页

H ：光标移至屏幕首行大写

M ：光标移至屏幕中间

L ：光标移至屏幕最末行

：数字 光标移动到指定行

跳转到文件末尾:shift+g  或在G

跳转到最后一行最后一个字符 shitf+g $

跳转到第一行的第一个字符:俩下g

复制粘贴:

:reg 查看粘贴板子

其他地方的内容复制过来"+p

要选中内容进行复制，先在命令模式下按 v 进入 Visual Mode，然后用方向键 或 hjkl 选择文本，再按 y 进行复制。

## 1.2 apt-get

ubuntun

```shell
sudo add-apt-repository ppa:apt-fast/stable
sudo apt-get install apt-fast
```

---

sudo add-apt-repository ppa:apt-fast/stable

sudo add-apt-repository ppa:saiarcot895/myppa

是可以直接使用的，格式为

```shell
sudo apt-get install/delete package
sudo apt-get -f install                                   #修复安装
sudo apt-get dist-upgrade                                 #升级系统
sudo apt-get upgrade                                      #更新已安装的包
apt-get source package                                    #下载该包的源代码
sudo apt-get build-dep package                            #安装相关的编译环境
sudo apt-get clean && sudo apt-get autoclean              #清理无用的包

#pip需要安装才能使用，配合virtualenvwrapper会锦上添花。安装过程如下（适用Ubuntu 10.10及以上版本），#使用格式为：pip install package。

sudo apt-get install python-pip python-dev build-essential
sudo pip install --upgrade pip
sudo pip install --upgrade virtualenv
```

## 1.3 工具

liunx 下载哪些:

1. 视频:	vlc ,	ffmepg

2. 编程:      g++,    gcc,     clion ,   Go ,   goland,    pycharm,  qt,   vscode     cmake

3. 工具:       meld(比较),     obs-studio(录屏),   typora  (md笔记),   teamviewer,   火狐浏览器,plank

4. 交流:       Tim  ,Wechat

5. 网易云音乐

6. apt-fast 

   注意:clion/goland/pycharm若build卡顿，找到安装文件clion.vmoptions更改-Xmx1024m 将数字改大一点就行

   apt-fast安装

   ```shell
   sudo apt-get install aria2    
   wget https://github.com/ilikenwf/apt-fast/archive/master.zip    
   unzip master.zip    
   cd apt-fast-master    
   sudo cp apt-fast /usr/bin    
   sudo cp apt-fast.conf /etc    
   sudo cp ./man/apt-fast.8 /usr/share/man/man8    
   sudo gzip /usr/share/man/man8/apt-fast.8   
   sudo cp ./man/apt-fast.conf.5 /usr/share/man/man5    
   sudo gzip /usr/share/man/man5/apt-fast.conf.5
   ```

   安装wechat不能发送截图
   
   ```
   sudo apt install libjpeg62:i386
   ```

画图工具：sudo apt-get install kolourpaint4

# 2 python

sudo pip3 install安装软件的时候出现sudo: pip3找不到命令的解决方法如下图所示：

```shell
sudo apt install python3-pip
```

PIL 是一个 Python 图像处理库，是本课程使用的重要工具，使用下面的命令来安装 pillow（PIL）库：

```shell
 sudo pip3 install --upgrade pip
 sudo pip3 install pillow
```
```shell
 sudo pip3 install jieba
```
```
mkdir work && cd work
mkdir gephi && cd gephi
wget http://labfile.oss.aliyuncs.com/courses/677/gephi-0.9.1-linux.tar.gz                         #下载
tar -zxvf gephi-0.9.1-linux.tar.gz     #解压 
```
运行py

```
python3 ascii.py ascii_dora.png
```

//分析人物关系

# 3 C++

liunx下编译

g++ -o main main.cpp


## 3.1 zlib

### 搭建环境:

win10下有编译好的源码，直接添加lib和dll就能使用(cmakeGUI搭建vs项目build all,最后两个项目一个是动态库，一个是静态库)

liunx 下下载好zlib源码，进入源码目录执行以下操作

```shell
./configure
make 
make check
sudo make install
```

安装成功后，可以在/usr/local/lib下找到libz.a

libz.a是一个静态库，为了使用zlib的接口，我们必须在连接我们的程序时，libz.a链接进来。  只需在 链接命令后加`-lz /usr/llocal/lib/libz.a` 即可。

```shell
-lz /usr/local/lib/libz.a
```

### 测试代码

如下

```c++
#include <iostream>
#include <zlib.h>

using namespace std;

int main(int argc, char *argv[])

{
    //原始数据
    unsigned char pchSrc[] = "xxx...." ;
    unsigned long nSrcLen = sizeof(pchSrc);
    //压缩之后的数据
    unsigned char achComp[1024];
    unsigned long nCompLen = 1024 ;
    //解压缩之后的数据
    unsigned char achUncomp[1024];
    unsigned long nUncompLen = 1024 ;
    //压缩
    compress(achComp,&nCompLen, pchSrc,nSrcLen);
    //解压缩
    uncompress(achUncomp,&nUncompLen, achComp,nCompLen);
    //显示原始数据信息
    printf("原始数据(%d):\n%s\n\n", nSrcLen,pchSrc);
    //显示压缩之后的数据
    printf("压缩数据(%d):\n%s\n\n", nCompLen,achComp);
    //显示解压缩之后的数据
    printf("解压数据(%d):\n%s\n\n", nUncompLen,achUncomp);
    
    return 0;
}
```
```cmake
#clion配置中需要在CMakeLists.txt添加以下内容
SET(CMAKE_EXE_LINKER_FLAGS
        "${CMAKE_EXE_LINKER_FLAGS} -Wl,-rpath -Wl,/usr/local/lib")
INCLUDE_DIRECTORIES(/usr/local/include)
TARGET_LINK_LIBRARIES(test libz.a)#test为项目名称
```
### 函数

#### 基础压缩

**compress**和**uncompress**是最基本的两个,分别用于压缩和解压

函数原型

```c++
ZEXTERN int ZEXPORT compress OF((Bytef *dest, uLongf *destLen,
                                 const Bytef *source, uLong sourceLen));
```

```cpp
ZEXTERN int ZEXPORT uncompress OF((Bytef *dest, uLongf *destLen,
                                   const Bytef *source, uLong sourceLen));
```

参数类型`Bytef`表示字节流，它与字符串有所不同，字节流没有结束符，因而需要配备长度信息，处理字符串的时候需要把结束符也当成一个普通的字节。 而`uLongf`则用于指明长度信息了， 其实相当于`unsigned long`。

```c++
//修改后demo
#include <iostream>
#include <zlib.h>

using namespace std;

int main(int argc, char *argv[]) {
    //原始数据
    unsigned char pchSrc[] = "xxx....";
    unsigned long nSrcLen = sizeof(pchSrc);
    //压缩之后的数据
    unsigned char achComp[1024];
    unsigned long nCompLen = 1024;
    //解压缩之后的数据
    unsigned char achUncomp[1024];
    unsigned long nUncompLen = 1024;
    //压缩
    compress(achComp, &nCompLen, pchSrc, nSrcLen);
    //解压缩
    uncompress(achUncomp, &nUncompLen, achComp, nCompLen);
    //显示原始数据信息
    cout << "原始数据:" << pchSrc << endl;
    cout << "压缩数据:" << achComp << endl;
    cout << "解压缩数据:" << achUncomp << endl;
//    printf("原始数据(%d):\n%s\n", nSrcLen,pchSrc);
//    //显示压缩之后的数据
//    printf("压缩数据(%d):\n%s\n", nCompLen,achComp);
//    //显示解压缩之后的数据
//    printf("解压数据(%d):\n%s\n", nUncompLen,achUncomp);

    return 0;
}
```
## 3.2 cmake

```cmake
cmake_minimum_required(VERSION 3.15) 	#cmkae最低版本
project(leetcode)						#项目名字

set(CMAKE_CXX_STANDARD 14)				#c++14

add_executable(leetcode main.cpp)		#往项目中添加文件

#指定头文件目录
include_directories(${PROJECT_SOURCE_DIR}/include)

```
```cmake
#指定CMake编译最低要求版本
CMAKE_MINIMUM_REQUIRED(VERSION 3.14)
#给项目命名
PROJECT(MYPRINT)
#收集c/c++文件并赋值给变量SRC_LIST_CPP  ${PROJECT_SOURCE_DIR}代表区当前项目录
FILE(GLOB SRC_LIST_CPP ${PROJECT_SOURCE_DIR}/src/*.cpp)
FILE(GLOB SRC_LIST_C ${PROJECT_SOURCE_DIR}/src/*.c)
#指定头文件目录
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/include)
#指定生成库文件的目录
SET(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
#去变量SRC_LIST_CPP 与SRC_LIST_C 指定生成libmyprint 动态库   默认生成静态库  SHARED指定生成库类型为动态库
ADD_LIBRARY(myprint SHARED ${SRC_LIST_CPP} ${SRC_LIST_C})
```



## 3.3 thread

```c++
//liunx 下线程需要在cmake文件下添加库
find_package(Threads REQUIRED)
target_link_libraries(Test1 Threads::Threads)
```

测试代码：创建5个线程//非11标准以上的写法

```c++
#include <iostream>
#include <pthread.h>

using namespace std;

const int NUM_THREADS = 5;

void* say_hello(void* args){
    cout << "hello" << endl;
    return 0;
}

int main() {
    //定义线程的id变量，多个变量使用数组
    pthread_t tids[NUM_THREADS];
    for(int i = 0; i < NUM_THREADS ;++i){
        int ret = pthread_create(&tids[i], NULL,say_hello, NULL);
        if(ret !=0){
            cout << "pthread_create error: error_code=" << ret << endl;
        }
    }
    pthread_exit(NULL);
}
```

demo2

```c++
#include <iostream>
#include <cstdlib>
#include <pthread.h>

using namespace std;

const int  NUM_THREADS = 5;

void *PrintHello(void *threadid)
{
    // 对传入的参数进行强制类型转换，由无类型指针变为整形数指针，然后再读取
    int tid = *((int*)threadid);
    cout << "Hello Runoob! 线程 ID, " << tid << endl;
    pthread_exit(NULL);
}

int main ()
{
    pthread_t threads[NUM_THREADS];
    int indexes[NUM_THREADS];// 用数组来保存i的值
    int rc;
    int i;
    for( i=0; i < NUM_THREADS; i++ ){
        cout << "main() : 创建线程, " << i << endl;
        indexes[i] = i; //先保存i的值
        // 传入的时候必须强制转换为void* 类型，即无类型指针
        rc = pthread_create(&threads[i], NULL,
                            PrintHello, (void *)&(indexes[i]));
        if (rc){
            cout << "Error:无法创建线程," << rc << endl;
            exit(-1);
        }
    }
    pthread_exit(NULL);
}
```

线程传递信息:

```c++
#include <iostream>
#include <cstdlib>
#include <pthread.h>

using namespace std;

conts int NUM_THREADS = 5;

struct thread_data {
    int thread_id;
    char *message;
};

void *PrintHello(void *threadarg) {
    struct thread_data *my_data;

    my_data = (struct thread_data *) threadarg;

    cout << "Thread ID : " << my_data->thread_id;
    cout << " Message : " << my_data->message << endl;

    pthread_exit(NULL);
}

int main() {
    pthread_t threads[NUM_THREADS];
    struct thread_data td[NUM_THREADS];
    int rc;
    int i;

    for (i = 0; i < NUM_THREADS; i++) {
        cout << "main() : creating thread, " << i << endl;
        td[i].thread_id = i;
        td[i].message = (char *) "This is message";
        rc = pthread_create(&threads[i], NULL,
                            PrintHello, (void *) &td[i]);
        if (rc) {
            cout << "Error:unable to create thread," << rc << endl;
            exit(-1);
        }
    }
    pthread_exit(NULL);
}
```

# 4 OpenGl

下载OpenGL需要的包

```shell
sudo apt-get install build-essential libgl1-mesa-dev  
sudo apt-get install freeglut3-dev  
sudo apt-get install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev
```

需要在cmake中添加如下内容

```cmake
find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)

include_directories(${OPENGL_INCLUDE_DIRS} ${GLUT_INCLUDE_DIRS})

target_link_libraries(${PROJECT_NAME} ${OPENGL_LIBRARIES} ${GLUT_LIBRARY})
```

demo代码

```c++
#include <GL/glut.h>

void init(void)
{
    glClearColor(0.0, 0.0, 0.0, 0.0);
    glMatrixMode(GL_PROJECTION);
    glOrtho(-5, 5, -5, 5, 5, 15);
    glMatrixMode(GL_MODELVIEW);
    gluLookAt(0, 0, 10, 0, 0, 0, 0, 1, 0);

    return;
}

void display(void)
{
    glClear(GL_COLOR_BUFFER_BIT);
    glColor3f(1.0, 0, 0);
    glutWireTeapot(3);
    glFlush();

    return;
}

int main(int argc, char *argv[])
{
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
    glutInitWindowPosition(0, 0);
    glutInitWindowSize(300, 300);
    glutCreateWindow("OpenGL 3D View");
    init();
    glutDisplayFunc(display);
    glutMainLoop();

    return 0;
}
```
# 5 ncurses

安装

```shell
sudo apt-get install libncurses5-dev
```

测试demo

```c++
#include <string.h>

#include <ncurses.h>

int main(int argc,char* argv[]){

    initscr();
    raw();
    noecho();
    curs_set(0);
    const char* c = "Hello, World!";
    mvprintw(LINES/2,(COLS-strlen(c))/2,c);
    refresh();
    getch();
    endwin();
    return 0;
}
```
编译

```shell
gcc test.c -o test -lncurses
```

# 6 Opencv

## 6.1 环境搭建

先装好依赖项

```shell
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev
```

下载好源码后进入到源码目录

创建一个目录编译opencv

```shell
mkdir build
cd build
```

cmake一下

```shell
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

执行编译过程

```shell
sudo make -j8
```

将make生成的文件安装到系统目录下

```shell
sudo make install 
```

配置环境

打开文件

```shell
sudo vim /etc/ld.so.conf
```

再打开的文件添加makefile安装路劲

```
/usr/local/lib
```

再运行

```shell
sudo ldconfig
```

修改bash.bashrc文件

```shell
sudo vim /etc/bash.bashrc
```

在文件末加入：

```shell
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig

export PKG_CONFIG_PATH
```



cmakelist.txt添加如下内容

```cmake
include_directories(/usr/local/include/opencv4/opencv2)
set (OpenCV_LIBS /usr/local/lib)
find_package(OpenCV REQUIRED)
target_link_libraries(helloCV ${OpenCV_LIBS}) #helloCV 工程名字
```

demon代码如下

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
#include <string>
using namespace cv;
using namespace std;
int main(int argc,char **argv)
{
    Mat img = imread("../test.jpeg");
 //   cout<<img;
    if(img.empty())
    {
        cout<<"error";
        return -1;
    }
    cout<<"My picture: "<< img.size() <<endl;
    imshow("image",img);
    waitKey();
    return 0;
}
```

查看opencv安装的库

```shell
pkg-config opencv --libs
```

查看opencv安装的版本

```shell
pkg-config opencv --modversion
```

查看opencv安装路劲

```shell
sudo find / -iname "*opencv*" > /home/xtt/Desktop/opencv_find.txt
```
# 7 SDL2

```shell
#sdl2
sudo apt-get install libsdl2-2.0
sudo apt-get install libsdl2-dev
sudo apt-get install libsdl2-mixer-dev
sudo apt-get install libsdl2-image-dev
sudo apt-get install libsdl2-ttf-dev
sudo apt-get install libsdl2-gfx-dev
#sdl1.1
sudo apt-get install libsdl1.2-dev
sudo apt-get install libsdl-image1.2-dev
sudo apt-get install libsdl-mixer1.2-dev
sudo apt-get install libsdl-ttf2.0-dev
sudo apt-get install libsdl-gfx1.2-dev
```

检测SDL装上了没有：

sdl-config --exec-prefix --version --cflag

```c++
#include <iostream>
#include <SDL/SDL.h>

using namespace std;

int main()
{
    int res;
    res = SDL_Init(SDL_INIT_EVENTTHREAD);
    if(res == 0){
        cout << "SDL init success!" << endl;
    }else{
        cout << "SDL init fail!" << endl;
    }
    return 0;
}
```

编译  

-lSDL

# 8 个人博客

## 8.1 下载nodejs

https://nodejs.org/en/

解压进入到bin文件夹下运行./node -v

```shell
sudo ln -s /home/xtt/nodejs/bin/npm /usr/local/bin/
sudo ln -s /home/xtt/nodejs/bin/node /usr/local/bin/
```

在别的目录下  确认是否正确

```shell
node -v
npm -v  
```

```shell
#下载cnpm
npm install -g cnpm --registry=http://registry.npm.taobao.org
sudo ln -s /home/xtt/nodejs/bin/cnpm /usr/local/bin/cnpm  
#下载cnpm
```

hexo

```
sudo ln -s /home/xtt/nodejs/bin/hexo /usr/local/bin/hexo
```



```
cnpm install -g hexo-cli
```

hexo -v 验证

进入创建好的博客目录

```shell
sudo hexo init
```

运行

```shell
hexo s
```

需要退出后创建新

hexo n "第一篇博客"

博客的md文件会自动生产放在下面路劲

```shell
cd source/_posts/
hexo clean
hexo g
sudo cnpm install --save hexo-deployer-git
```



设置_config.yml

```
# Deployment

## Docs: https://hexo.io/docs/deployment.html

deploy:
  type: git
  repo: https://github.com/JoeNero/JoeNero.github.io.git
  branch: master
```

hexo d



创建博客md

hexo n “博客”

清空数据库

hexo clean

生产数据库

hexo g

推送

hexo d 



# 9 github

下载git

```shell
sudo apt-get install git
```

检查下载版本

```shell
git version
```

下载ssh

```shell
sudo apt-get install ssh  
```

检查ssh服务状态

```shell

 #显示sshd的话表示ssh-server已经启动
```

使用  命令查看 ssh key 是否存在，若存在则忽略这一步

```
ssh-keygen -t rsa -C "526988861@qq.com"
```

clone

```shell
git clone 网址
```



```shell
git init
git add readme.md #将文件添加到暂存区域
```

```shell
git commit -m "add readme file" #提交本次修改
```

```shell
git push origin master	#推送到远程仓库
```

# 10 服务器

ssh 链接服务器

```
ssh username@IP
```

ssh root@106.14.28.137
客户端到服务器
scp ./filename username@IP:/home/bio321/Desktop
服务器到客户端
scp ./filename username@IP:/home/bio321/Desktop
//客户端
scp -r Kail root@106.14.28.137:/root/liunx

teamviewer

1569607240

647emx



美化主题

```
sudo add-apt-repository ppa:numix/ppa
sudo apt-get update
sudo apt-get install numix-gtk-theme numix-icon-theme-circle
```

shell 美化
```shell
sudo apt-get install zsh #安装zsh

zsh --version #确认是否安装成功

sudo chsh -s $(which zsh)  #设置zsh为默认shell
```