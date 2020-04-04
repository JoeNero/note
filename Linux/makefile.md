# 1 makefile的规则

  target ... : prerequisites ...
      command
      ...
      ...

target也就是一个目标文件，可以是Object File，也可以是执行文件。还可以是一个标签（Label），对于标签这种特性，在后续的“伪目标”章节中会有叙述。

prerequisites就是，要生成那个target所需要的文件或是目标。
command也就是make需要执行的命令。（任意的Shell命令）