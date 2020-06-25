# 1 语法
Android.mk 文件必须先定义 LOCAL_PATH 变量：
```
LOCAL_PATH :=$(call my-dir)
```
此变量表示源文件在开发树中的位置。
在这行代码中，编译系统提供的宏函数 my-dir 将返回当前目录（Android.mk 文件本身所在的目录）的路径。
下一行声明 `CLEAR_VARS` 变量，其值由编译系统提供。

```
include $(CLEAR_VARS)
```
CLEAR_VARS 变量指向一个特殊的 GNU Makefile，后者会清除许多 LOCAL_XXX 变量，例如 LOCAL_MODULE、LOCAL_SRC_FILES 和 LOCAL_STATIC_LIBRARIES。请注意，GNU Makefile 不会清除 LOCAL_PATH。此变量必须保留其值，因为系统在单一 GNU Make 执行环境（其中的所有变量都是全局变量）中解析所有编译控制文件。在描述每个模块之前，必须声明（重新声明）此变量。
```
LOCAL_MODULE := hello-jni
```
每个模块名称必须唯一，且不含任何空格。编译系统在生成最终共享库文件时，会对您分配给 LOCAL_MODULE 的名称自动添加正确的前缀和后缀。例如，上述示例会生成名为 libhello-jni.so的库。
注意：如果模块名称的开头已经是 lib，则编译系统不会附加额外的 lib 前缀；而是按原样采用模块名称，并添加 .so 扩展名。因此，比如原来名为 libfoo.c 的源文件仍会生成名为 libfoo.so 的共享对象文件。此行为是为了支持 Android 平台源文件根据 Android.mk 文件生成的库；所有这些库的名称都以 lib 开头。
下一行会列举源文件，以空格分隔多个文件：
```
LOCAL_SRC_FILED :=hello-jni.c
```
LOCAL_SRC_FILES 变量必须包含要编译到模块中的 C 和/或 C++ 源文件列表。
最后一行帮助系统将所有的内容连接到一起:
```
include $(BUILD_SHARED_LIBRARY)
```
BUILD_SHARED_LIBRARY 变量指向一个 GNU Makefile 脚本，该脚本会收集您自最近 include 以来在 LOCAL_XXX 变量中定义的所有信息。此脚本确定要编译的内容以及编译方式
# 2 变量
## 2.1 CLEAR_VARS
此变量指向的编译脚本用于取消定义下文“开发者定义的变量”部分中列出的几乎所有 LOCAL_XXX 变量。在描述新模块之前，请使用此变量来包含此脚本。使用它的语法为：
```
include $(CLEAR_VARS)
```
## 2.2 BUILD_SHARED_LIBRARY
此变量指向的编译脚本用于收集您在 LOCAL_XXX 变量中提供的模块的所有相关信息，以及确定如何根据您列出的源文件编译目标共享库。请注意，使用此脚本要求您至少已经为 LOCAL_MODULE 和 LOCAL_SRC_FILES 赋值
```
include $(BUILD_SHARED_LIBRARY)
```
## 2.3 BUILD_STATIC_LIBRARY
用于编译静态库的 BUILD_SHARED_LIBRARY 的变体。编译系统不会将静态库复制到您的项目/软件包中，但可以使用静态库编译共享库（请参阅下文的 LOCAL_STATIC_LIBRARIES 和 LOCAL_WHOLE_STATIC_LIBRARIES）。使用此变量的语法为：
```
include $(BUILD_STATIC_LIBRARY)
```
静态库变量会导致编译系统生成扩展名为 .a 的库
## 2.4 PREBUILT_SHARED_LIBRARY
指向用于指定预编译共享库的编译脚本。与 BUILD_SHARED_LIBRARY 和 BUILD_STATIC_LIBRARY的情况不同，这里的 LOCAL_SRC_FILES 值不能是源文件，而必须是指向预编译共享库的一个路径，例如 foo/libfoo.so。使用此变量的语法为
```
include $(PREBUILT_SHARED_LIBRARY)
```
## 2.5 PREBUILT_STATIC_LIBRARY

# 3 目标信息变量

## 3.1 TARGET_ARCH
编译系统解析此 `Android.mk` 文件时面向的 CPU 系列。此变量是 `arm`、`arm64`、`x86` 或 `x86_64`之一。
编译系统解析此 Android.mk 文件时面向的 Android API 级别编号。例如，Android 5.1 系统映像对应于 Android API 级别 22：android-22。如需平台名称和对应 Android 系统映像的完整列表，请参阅 Android NDK 原生 API。以下示例演示了使用此变量的语法：
```
ifeq ($(TARGET_PLATFORM),android-22)
        # ... do something ...
endif
```
编译系统解析此 Android.mk 文件时面向的 Android API 级别编号。例如，Android 5.1 系统映像对应于 Android API 级别 22：android-22。如需平台名称和对应 Android 系统映像的完整列表，请参阅 Android NDK 原生 API。以下示例演示了使用此变量的语法：
```
ifeq ($(TARGET_ARCH_ABI),arm64-v8a)
      # ... do something ...
endif
```

| CPU和架构     | 设置        |
| ------------- | ----------- |
| ARMv7         | armeabi-v7a |
| ARMv8 AArch64 | arm64-v8a   |
| i686          | x86         |
| x86-64        | x86_64      |

# 4 模块描述变量
每个模块描述都应遵守以下基本流程：
1.  使用 CLEAR_VARS 变量初始化或取消定义与模块相关的变量。
2.	为用于描述模块的变量赋值。
3.	使用 BUILD_XXX 变量设置 NDK 编译系统，使其将适当的编译脚本用于该模块。

LOCAL_PATH
此变量用于指定当前文件的路径，必须在Andorid.mk文件开头定义此变量
```
LOCAL_PATH :=$(call my-dir)
```
此变量用于存储模块名称。指定的名称必须唯一，并且不得包含任何空格。必须在包含任何脚本（CLEAR_VARS 的脚本除外）之前定义此变量。无需添加 lib 前缀或者 .so 或 .a 文件扩展名；编译系统会自动进行这些修改。在整个 Android.mk 和 Application.mk 文件中，请通过未经修改的名称引用模块。例如，以下行会导致生成名为 libfoo.so 的共享库模块：
```
LOCAL_MODULE := "foo"
```
如果希望生成的模块使用除“lib + LOCAL_MODULE 的值”以外的名称，您可使用 LOCAL_MODULE_FILENAME 变量为生成的模块指定自己选择的名称。
此可选变量使您能够替换编译系统为其生成的文件默认使用的名称。例如，如果 LOCAL_MODULE 的名称为 foo，您可以强制系统将它生成的文件命名为 libnewfoo。以下示例演示了如何完成此操作：
```
LOCAL_MODULE := foo
    LOCAL_MODULE_FILENAME := libnewfoo
```
对于共享库模块，此示例将生成一个名为 libnewfoo.so 的文件。
注意：您无法替换文件路径或文件扩展名
**LOCAL_SRC_FILES**
此变量包含编译系统生成模块时所用的源文件列表。只列出编译系统实际传递到编译器的文件，因为编译系统会自动计算所有相关的依赖关系。请注意，您可以使用相对（相对于 LOCAL_PATH）和绝对文件路径。
注意：务必在编译文件中使用 Unix 样式的正斜杠 (/)。编译系统无法正确处理 Windows 样式的反斜杠 (\)。
**LOCAL_CPP_EXTENSION**
```
LOCAL_CPP_EXTENSION := .cxx
LOCAL_CPP_EXTENSION := .cxx .cpp .cc
```
例如，要指明您的代码使用 RTTI（运行时类型信息），请输入
```
LOCAL_CPP_FEATURES := rtti
```
要指明您的代码使用 C++ 异常，请输入：
```
LOCAL_CPP_FEATURES := exceptions
```
您还可以为此变量指定多个值。例如：
```
您还可以为此变量指定多个值。例如：
```



