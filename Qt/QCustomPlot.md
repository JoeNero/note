# QCustomPlot

```
QT       += core gui printsupport
```

选择项目的.pro文件，添加printsupport，如图，QCustomPlot包含了一些打印的东西，如果没有这一步，程序会报错

基类为QWidget：提升为`QCustomPlot`

这里强调一下：Qt提升控件时，通常提升的类名称中，每个单词的首字母必须大写与Qt控件命名规则保持一致，各种第三方控件都采用这种命名格式，否则无法识别，如这里必须写成`QCustomPlot`而不能写成`Qcustomplot`或`qcustomplot`