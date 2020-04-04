# 1 配置

添加模块

```
QT       += multimedia
```

# 2 准备




# 3 功能

按钮的样式
```cpp
  setStyleSheet(
                   //正常状态样式
                   "QPushButton{"
                   "background-color:rgba(100,225,100,30);"//背景色（也可以设置图片）
                   "border-style:outset;"                  //边框样式（inset/outset）
                   "border-width:4px;"                     //边框宽度像素
                   "border-radius:10px;"                   //边框圆角半径像素
                   "border-color:rgba(255,255,255,30);"    //边框颜色
                   "font:bold 10px;"                       //字体，字体大小
                   "color:rgba(0,0,0,100);"                //字体颜色
                   "padding:6px;"                          //填衬
                   "}"
                   //鼠标按下样式
                   "QPushButton:pressed{"
                   "background-color:rgba(100,255,100,200);"
                   "border-color:rgba(255,255,255,30);"
                   "border-style:inset;"
                   "color:rgba(0,0,0,100);"
                   "}"
                   //鼠标悬停样式
                   "QPushButton:hover{"
                   "background-color:rgba(100,255,100,100);"
                   "border-color:rgba(255,255,255,200);"
                   "color:rgba(0,0,0,200);"
                   "}");
```
移动到按钮上鼠标变形
![1584743914(1).jpg](https://i.loli.net/2020/03/21/TDsvjmYVLXMzRGo.png)

右键按钮控件->改变样式表->添加资源->border-image->选择你要的图片
有效样式表ok

按钮添加图片

![1584745733(1).jpg](https://i.loli.net/2020/03/21/f4K96NzlnkRJaDe.png)

![1.png](https://i.loli.net/2020/03/21/Dayg5lpkUwV471F.png)



编译
![1.jpg](https://i.loli.net/2020/03/21/ULPv2D8qNZR91xH.png)

spinbox


![1584851035(1).jpg](https://i.loli.net/2020/03/22/7REAbp3BzurVgNm.png)

图标


![1584852308(1).jpg](https://i.loli.net/2020/03/22/Ej63XK1pnoYbHGM.png)








