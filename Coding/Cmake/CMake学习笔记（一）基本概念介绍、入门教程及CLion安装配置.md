# CMake学习笔记（一）基本概念介绍、入门教程及CLion安装配置

## 什么是构建系统

在软件开发中，**构建系统**（*build system*）是用来从源代码生成用户可以使用的**目标**的**自动化工具**。目标可以包括库、可执行文件、或者生成的脚本等等。

通常每个构建系统都有一个对应的**构建文件**（也可以叫**配置文件**、**项目文件**之类的），来指导构建系统如何编译、链接生成可执行程序等等。构建文件中通常描述了`要生成的目标`、`生成目标所需要的源代码文件`、`依赖库`等等内容。不同构建系统使用的构建文件的名称和内容格式规范通常各不相同。

### 常见的构建系统

- `GNU Make`：类Unix操作系统下的构建系统，构建文件名称通常是`Makefile`或`makefile`。
- `NMake`：可以理解为Windows平台下的`GNU Make`，是微软`Visual Studio`早期版本使用的构建系统，比如`VC++6.0`。构建文件的后缀名是`.mak`。
- `MSBuild`：`NMake`的替代品，一开始是与`.net`框架绑定的。`Visual Studio`从`2013`版本开始使用它作为构建系统。`Visual Studio`的项目构建依赖于`MSBuild`，但`MSBuild`并不依赖前者，可以独立运行。构建文件的后缀名是`.vcproj`（以`C++`项目为例）。
- `Ninja`：一个专注于速度的小型构建系统，`Chrome`团队开发。

### CMake

`CMake`是一个开源的跨平台构建系统，用来管理软件建置的程序，并不依赖于某特定编译器，并可支持多层目录、多个应用程序与多个库。虽然`CMake`同样用构建文件控制构建过程（名称是`CMakeLists.txt`），但它却并不直接构建并生成目标，而是产生**其他构建系统**所需要的构建文件，然后再由它们来构建生成最终目标。支持`MSBuild`、`GNU Make`、`MINGW Make`等等构建系统。

### qmake

`qmake`是一个协助简化跨平台进行项目开发的构建过程的工具程序，`Qt`附带的工具之一 。与`CMake`类似，`qmake`同样不是直接构建并生成目标，而是依赖其他构建系统。它能够自动生成`Makefile`、`Visual Studio`项目文件 和 `XCode`项目文件。不管项目是否使用`Qt`框架，都能使用`qmake`，因此`qmake`能用于很多软件的构建过程。

`qmake`使用的构建文件是`.pro`项目文件，开发者能够自行撰写项目文件或是由`qmake`本身产生。`qmake`包含额外的功能来方便 `Qt` 开发，如自动包含`moc`和`uic`的编译规则。值得一提的是，`CMake`同样支持`Qt`开发，但并不依赖于`qmake`。

## CMake入门教程（Step By Step）

这个入门教程我主要是参考了`CMake`官方的[Tutorial](https://cmake.org/cmake-tutorial/)以及网上的一些资料，并进行了一些更改和补充，使得理解起来更加容易。

### 1. 安装、配置开发环境

在开始之前，我们可以选择一个自己喜欢的`IDE`(集成开发环境)来作为`C/C++`开发工具，`CMake`支持`Visual Studio`、`QtCreator`、`Eclipse`、`CLion`等多个`IDE`，当然你也可以使用像是`VSCode`、`Vim`这些文本编辑器并配合一些插件来作为开发工具。由于我个人的习惯和偏好，加上主要是`Linux`系统下开发，最终选择了*JetBrains*家族的`CLion`作为开发工具。

- 安装构建工具链（**CMake、编译器、调试器、构建系统**）：

    `CMake`：Debian系Linux系统可以使用apt安装：`sudo apt install cmake cmake-qt-gui`，如果嫌`apt`里的版本太低，可以手动编译安装最新版本，参考[编译安装cmake及cmake-gui](https://blog.csdn.net/lang523493505/article/details/85283563)；Windows系统可以前往[官网](https://cmake.org/download/)下载安装程序。

    `其他`：`Linux`系统下直接使用自带的`GNU`套件(**make、gcc、gdb**)，`Windows`系统可以使用`MinGW-w64`、`MSVC`或者`WSL`等工具。

- 下载安装：[CLion官网](https://www.jetbrains.com/clion/)，可以免费试用30天。

- 首次运行：登录帐号进行激活授权，偏好配置。学生可以使用`edu`邮箱注册*JetBrains*帐号，然后可以免费使用*JetBrains*家族所有`IDE`的`ULTIMATE`版本。

- 界面汉化（可选）：[平方X JetBrains系列软件汉化包](https://github.com/pingfangx/TranslatorX)。我英文不太行，所以汉化还是挺有必要的。

- 配置构建工具链（**设置 -> 构建，执行，部署 -> 工具链**）：这一步是在`CLion`中配置构建需要的工具的路径。`CMake`可以直接使用`CLion`自带绑定的一个版本，当然也可以选择自己安装的版本。

    

    ![配置构建工具链](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e534ec70e0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

    

- 配置`CMake`选项（**设置 -> 构建，执行，部署 -> CMake**）：设置构建类型（Debug/Release），`CMake`构建选项参数、构建目录等等。一般保持默认的就可以，等到需要修改`CMake`构建相关选项的时候再去配置。

    

    ![配置CMake选项](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e53562531e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

    

### 2. 创建一个CLion C++项目

打开`CLion`，新建一个**C++可执行程序项目**，`C++`标准版本我选择了`C++17`。其实像是标准版本、构建目标类型这些选项，在新建项目时选好了，后面还是可以通过`CMakeLists.txt`文件随时进行更改的，不用太过纠结。



![CLion新建项目](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e532fd3986?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 3. 项目结构及CMakeLists.txt内容分析

创建一个项目后，初始结构是这样的：



![CLion项目结构分析](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e52ca40aae?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



- `CMakeLearnDemo`：项目源目录，包含项目源文件的**顶级目录**。
- `main.cpp`：自动生成的`main`函数源文件，没什么好说的。
- `cmake-build-debug`：`CLion`调用`CMake`生成的默认**构建目录**。什么是构建目录呢，用于存储**构建系统文件**（比如makefile以及其他一些cmake相关配置文件）和**构建输出文件**（编译生成的中间文件、可执行程序、库）的顶级目录。因为我们肯定不想把构建生成的文件和项目源文件混在一块，这样会使项目结构变得混乱，所以一般都会单独创建一个构建目录。当然如果你喜欢，可以直接将项目源目录作为构建目录。使用`CLion`我们不需要手动在命令行调用`CMake`来生成构建目录以及构建项目，`CLion`在`CMakeLists.txt`的内容发生改变时会自动重新生成构建目录，构建项目也只需要点击**构建**按钮就可以了。但是在学习阶段，了解`CMake`的基本用法还是很重要的，等到熟悉了之后，再使用`IDE`也就得心应手了。我们在项目源目录下新建一个**mybuild**目录，作为我们自己手动调用`CMake`命令时所指定的构建目录，如图：



![创建mybuild构建目录](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e53810bdaa?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



- `CMakeLists.txt`：`cmake`项目配置文件，准确点说是项目顶级目录的`cmake`配置文件，因为一个项目在多个目录下可以有多个`CMakeLists.txt`文件。这个应该是`cmake`的核心配置文件了，基本上更改项目构建配置都是围绕着这个文件进行。我们来看看`CLion`为我们自动生成的`CMakeLists.txt`是什么内容：

    

    ![CMakeLists.txt初始内容](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e535537c3f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

    

    ```
    cmake_minimum_required(VERSION 3.15)
    复制代码
    ```

    设置`cmake`的最低版本要求，如果`cmake`的运行版本低于最低要求版本，它将停止处理项目并报告错误。

    ```
    project(CMakeLearnDemo)
    复制代码
    ```

    设置项目的名称，并将其值存储在`cmake`内置变量`PROJECT_NAME`中。当从顶级`CMakeLists.txt`调用时，还将其存储在内置变量`CMAKE_PROJECT_NAME`中。

    ```
    set(CMAKE_CXX_STANDARD 17)
    复制代码
    ```

    设置`C++`标准的版本。目前支持的版本值是`98`, `11`, `14`, `17`和`20`。

    ```
    add_executable(CMakeLearnDemo main.cpp)
    复制代码
    ```

    添加一个**可执行文件**类型的**构建目标**到项目中。`CMakeLearnDemo`是文件名，后面是生成这个可执行文件所需要的源文件列表。

### 4. 一个最基础项目的配置、构建和运行

首先我们将`main.cpp`的内容修改一下，代码如下：

```
#include <cmath>
#include <iostream>

int main(int argc, char *argv[])
{
    if(argc < 2)
    {
        std::cerr << "Must have at least 2 command line arguments." << std::endl;
        return 1;
    }

    try
    {
        double inputValue = std::stof(argv[1]);
        double outputValue = std::sqrt(inputValue);
        std::cout << "the square root of " << inputValue 
                  << " is " << outputValue << std::endl;
    }
    catch(const std::invalid_argument& e)
    {
        std::cerr << e.what() << std::endl;
        return 1;
    }

    return 0;
}
复制代码
```

在`main`函数中，主要做了以下操作：读取命令行参数值，计算它的算术平方根并输出，同时包含了一些错误判断。

#### 明确需要某个C++标准版本

之前设置的`CMAKE_CXX_STANDARD`只是一个可选属性，如果编译器不支持此标准版本，则还是有可能会退化为以前的版本。如果我们想要明确表示需要某个`C++`标准，则可以通过：

```
set(CMAKE_CXX_STANDARD_REQUIRED True)
复制代码
```

来实现。这样的话，如果编译器不支持此标准，则`CMake`会直接报错，停止运行。

#### 生成项目构建系统

这一步可以理解为一个项目配置过程，并没有发生任何的编译工作。`cmake`根据项目的`CMakeLists.txt`文件在**构建目录**中生成对应**构建系统**的**构建文件**，同时也包含了很多`cmake`相关的配置文件，不过这些自动生成文件的内容其实我们目前不需要去关心。

生成项目构建系统的命令有3种形式：

使用当前目录作为构建目录，`<path-to-source>`作为项目源目录。可以是相对或者绝对路径。

```
cmake [<options>] <path-to-source>

# 举例，..代表当前目录的上级目录
cd mybuild
cmake ..
复制代码
```

使用`<path-to-existen-build>`作为构建目录，并从其`CMakeCache.txt`文件中获取到项目源目录的路径，该文件必须是在以前的`CMake`运行时生成的，也就是说从之前已经生成过构建系统的构建目录中获取到之前的项目源目录，并重新生成一次。可以是相对或者绝对路径。

```
cmake [<options>] <path-to-existing-build>

# 举例
cmake mybuild
复制代码
```

使用`<path-to-source>`作为项目源目录，`<path-to-build`作为构建目录。可以是相对或者绝对路径。

```
cmake [<options>] -S <path-to-source> -B <path-to-build>

# 举例，. 代表当前目录
cmake -S . -B mybuild
复制代码
```

我更喜欢第三种方式，因为这样更直观。接下来就让我们亲自生成项目构建系统吧：



![生成项目构建系统](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e5e7c0e60a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### 切换构建系统

对于每种构建系统，都对应着一个`cmake`生成器，负责为此构建系统生成原生的相关构建文件，可以在调用`cmake`命令行工具时通过`-G <generator_name>`来指定，也可以在`CMakeLists.txt`中通过设置`CMAKE_GENERATOR`变量的值来指定。

`cmake`生成器是特定于平台的，因此每个生成器只能在特定的平台上使用。可以使用`cmake --helo`命令行来查看当前平台上可用的生成器，我的`Linux mint 1903`输出如下图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e5e2170ec6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### 切换构建类型

构建类型有`Debug`、`Release`、`RelWithDebInfo`、`MinSizeRel`等，可以通过`CMAKE_BUILD_TYPE`变量来指定，比如：

```
set(CMAKE_BUILD_TYPE Release)
复制代码
```

#### 构建项目

生成项目构建系统后，接下来就可以选择构建项目了。我们可以直接调用相应的构建系统来构建项目，比如`GNU make`，也可以调用`cmake`来让它自动选择相对应的构建系统来构建项目。如下：



![使用make构建项目](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e5f0910b16?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



或者：



![使用cmake调用构建系统来构建项目](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e60dd91355?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### 运行可执行文件

构建完成了，接下来让我们运行可执行文件，看看运行结果：



![运行CMakeLearnDemo](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e62d844fac?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 5. 为项目添加一个版本号

虽然可以直接在源文件里定义版本号，但是在`CMakeLists.txt`里设置会更加灵活方便。配置版本号是作为`project`命令的一个可选择项，语法如下：

```
project(<PROJECT-NAME>
				 [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
复制代码
```

不难看出，版本号最少1个层级，最多4个层级。以版本号`0.8.1.23`为例，各个层级的值及含义如下：

- `major`：主版本号，值为`0`。可以通过`cmake`内置变量`PROJECT_VERSION_MAJOR`或者`<PROJECT-NAME>_VERSION_MAJOR`来获取到它的值。
- `minor`：次版本号，值为`8`。可以通过`cmake`内置变量`PROJECT_VERSION_MINOR`或者`<PROJECT-NAME>_VERSION_MAJOR`来获取到它的值。
- `patch`：补丁版本号，值为`1`。可以通过`cmake`内置变量`PROJECT_VERSION_PATCH`或者`<PROJECT-NAME>_VERSION_PATCH`来获取到它的值。
- `tweak`：微小改动版本号，值为`23`。可以通过`cmake`内置变量`PROJECT_VERSION_TWEAK`或者`<PROJECT-NAME>_VERSION_TWEAK`来获取到它的值。
- 完整版本号`0.8.1.23`可以通过`cmake`内置变量`PROJECT_VERSION`或者`<PROJECT-NAME>_VERSION`来获取到它的值。

现在，让我们创建一个`autoGeneratedHeaders`文件夹，用于存放由`cmake`自动生成或更新的头文件。接着，创建一个`projectConfig.h.in`文件，内容如下：

```
#ifndef CMAKELEARNDEMO_PROJECTCONFIG_H
#define CMAKELEARNDEMO_PROJECTCONFIG_H

#define PROJECT_VERSION "@CPROJECT_VERSION@"
#define PROJECT_VERSION_MAJOR @PROJECT_VERSION_MAJOR@
#define PROJECT_VERSION_MINOR @PROJECT_VERSION_MINOR@
#define PROJECT_VERSION_PATCH  @PROJECT_VERSION_PATCH@

#endif //CMAKELEARNDEMO_PROJECTCONFIG_H
复制代码
```

接着将`CMakeLists.txt`里添加或更改如下内容：

```
project(CMakeLearnDemo VERSION 1.0.0)

configure_file(
        autoGeneratedHeaders/projectConfig.h.in
        ${PROJECT_SOURCE_DIR}/autoGeneratedHeaders/projectConfig.h
)

target_include_directories(CMakeLearnDemo PUBLIC
        autoGeneratedHeaders
)
复制代码
```

解释：`configure_file(<input> <output> ...)`命令的作用是将指定的文件复制到另一个位置并替换某些内容。`<input>`如果使用相对路径，则相对于项目源目录；`<output>`如果使用相对路径，则相对于项目构建目录，所以在`<output>`的值中我使用了`PROJECT_SOURCE_DIR`这个`cmake`变量来获取项目源目录。如果希望直接替换某个文件中的内容，可以将`<input>`和`<output>`的值指向同一文件。

再说替换，在`projectConfig.h.in`中，`@VARIABLE_NAME@`这个语法是引用某个`cmake`变量的值，执行`configure_file`命令后，它就会被替换为相应变量的值。而具体定义多少个项目层级，这由你自己决定，我这里就只定义了3个项目层级和一个完整版本号。

`target_include_directories(...)`命令是将**指定目录**添加到**指定构建目标**的**编译器搜索包含文件目录**中。通过添加`autoGeneratedHeaders`到包含文件目录，在`main.cpp`里可以直接使用

```
#include "projectConfig.h"
// 或者
#include <projectConfig.h>
复制代码
```

而不用

```
#include "autoGeneratedHeaders/projectConfig.h"
// 或者
#include <autoGeneratedHeaders/projectConfig.h>
复制代码
```

接下来，让我们在`main.cpp`里输出项目版本信息，如下：

```
#include "projectConfig.h"

int main(int argc, char *argv[])
{
    std::cout << "Project Version: " << PROJECT_VERSION << std::endl;
    std::cout << "Project Version Major: " << PROJECT_VERSION_MAJOR << std::endl;
    std::cout << "Project Version Minor: " << PROJECT_VERSION_MINOR << std::endl;
    std::cout << "Project Version Patch: " << PROJECT_VERSION_PATCH << std::endl;
    ...
复制代码
```

重新生成构建系统，不出意外会新增一个`projectConfig.h`文件。

构建然后运行可执行文件，查看输出内容：



![版本号输出](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e63aaf4ed9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 6. 添加库

之前我们在`main`函数中是使用标准库中的`std::sqrt`来计算平方根。现在，让我们自己实现一个计算平方根的函数，并将其构建生成**静态库**，最后在`main`函数中使用我们自己的平方根库函数来替代标准库。

#### 代码实现、库生成及使用

创建一个`mymath`文件夹，存放我们自己实现的平方根函数的`.h`、`.cpp`以及`CMakeLists.txt`文件。结构如下：



![mymath结构](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e6b581d15d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



`mymath.h`内容如下：

```
#ifndef CMAKELEARNDEMO_MYMATH_H
#define CMAKELEARNDEMO_MYMATH_H

#include <stdexcept>

namespace mymath
{
    // calculate the square root of number
    double sqrt(double number) noexcept(false);
}

#endif // CMAKELEARNDEMO_MYMATH_H
复制代码
```

`mymath.cpp`内容如下：

```
#include "mymath.h"

namespace mymath
{
    double sqrt(double number)
    {
        static constexpr double precision = 1e-6;
        static constexpr auto abs = [](double n) ->double { return n > 0? n:-n; };

        if(number < 0)
        {
            throw std::invalid_argument("Cannot calculate the square root of a negative number!");
        }

        double guess = number;
        while( abs(guess * guess - number) > precision)
        {
            guess = ( guess + number / guess ) / 2;
        }

        return guess;
    }
}
复制代码
```

`CMakeLists.txt`内容如下：

```
add_library(mymath STATIC mymath.cpp)
复制代码
```

`add_library`与之前的`add_execuable`类似，只不过它添加的是库目标，而不是可执行文件目标。`mymath`是库名，`STATIC`指明生成静态库，`mymath.cpp`是生成这个库所需要的源文件。

接下来我们需要在项目根目录的`CMakeLists.txt`中添加或更改如下内容：

```
add_subdirectory(mymath)

target_include_directories(CMakeLearnDemo PUBLIC
        autoGeneratedHeaders
        mymath
)

target_link_libraries(CMakeLearnDemo PUBLIC
        mymath
)
复制代码
```

`add_subdirectory`的作用是将一个包含`CMakeLists.txt`的子目录添加到构建中，因为子`CMakeLists.txt`如果没有被添加到根`CMakelists.txt`中，它是不会参与构建的。

将`mymath`添加到`target_include_directories`中，因为在`main.cpp`中需要包含`mymath.h`头文件；`target_link_libraries`的作用是将依赖库链接到指定目标，因为`main.cpp`中需要用到`mymath`库。

现在让我们在`main.cpp`将`std::sqrt`换成我们自己的平方根函数，改动的地方如下：

```
- #include <cmath>
+ #include "mymath.h"
...
int main(int argc ,char* argv[])
{
    ...
    double outputValue = mymath::sqrt(inputValue);
    ...
}
复制代码
```

重新生成构建系统并构建运行，输出结果应该是与之前一样的；打开`mybuild`目录，可以发现里面多了一个`mymath`子构建目录，其目录下有生成的库文件`libmymath.a`，如图：



![mymath子构建目录](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e6b5615002?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### 将库设置为可选的

现在让我们将`mymath`库设置为用户可选择的，虽然对一个教程来说不是很有必要，但是在一个大型项目中这种行为还是很常见的。

首先让我们在`CMakeLists.txt`中加入或更改如下内容：

```
option(USE_MYMATH "Use CMakeLearnDemo provided math implementation" ON)
message("value of USE_MYMATH is : " ${USE_MYMATH})

configure_file(
        autoGeneratedHeaders/projectConfig.h.in
        ${PROJECT_SOURCE_DIR}/autoGeneratedHeaders/projectConfig.h
)

if(USE_MYMATH)
    add_subdirectory(mymath)
    list(APPEND EXTRA_INCLUDES mymath)
    list(APPEND EXTRA_LIBS mymath)
endif()

add_executable(CMakeLearnDemo main.cpp)

target_include_directories(CMakeLearnDemo PUBLIC
        autoGeneratedHeaders
        ${EXTRA_INCLUDES}
)

target_link_libraries(CMakeLearnDemo PUBLIC
        ${EXTRA_LIBS}
)
复制代码
```

解释：

```
option(<variable> "<help_text>" [value])
复制代码
```

添加一个选项供用户选择`ON`开启或者`OFF`关闭，用户最终选择的值会存放到变量`<variable>`中，`"<help_text"`是该选项的描述信息，`[value]`是默认开启/关闭值。

`message`命令会在生成构建系统时输出一条信息，一般是用来查看变量的值之类的。

`if()`和`endif()`条件语句和其他高级语言中的作用类似，可以阅读[cmake if条件判断语法](https://cmake.org/cmake/help/v3.16/command/if.html?highlight=#condition-syntax)来了解哪些变量值会被`if`判定为`True`，哪些会被判定为`False`。

`cmake`中的列表型变量是指用`;`分隔开的字符串，比如`"a;b;c;d;e"`。`list()`命令的作用则是对列表型变量进行一系列操作，比如**添加、插入、删除、获取长度**等等。在本教程中，我们使用`EXTRA_INCLUDES`和`EXTRA_LIBS`这2个变量来单独存放由用户勾选的可选库的包含路径和库路径。

可以使用`set()`命令来创建一个列表型变量，尝试在`CMakeLists.txt`中加入如下几行，并观察输出：

```
set(testVariable "a" "b" "c")
message(${testVariable})
list(LENGTH testVariable testLength)
message(${testLength})

set(testVariable "a;b;c")
message(${testVariable})
list(LENGTH testVariable testLength)
message(${testLength})

set(testVariable "a b c")
message(${testVariable})
list(LENGTH testVariable testLength)
message(${testLength})
复制代码
```

接下来让我们在`projectConfig.h.in`中加入如下一行：

```
#cmakedefine USE_MYMATH
复制代码
```

`configure_file`命令会根据`USE_MYMATH`变量的值来将这一行替换为相应的内容。如果是被`if`命令判定为`True`的值，则会被替换为：

```
#define USE_MYMATH
复制代码
```

否则则会被替换为：

```
/* #undef USE_MYMATH */
复制代码
```

这样的话，我们就能通过使用条件编译来判断是否定义`USE_MYMATH`宏，从而在代码中选择使用标准库还是自己的库。在`main.cpp`中我们需要修改以下几个地方：

```
#ifdef USE_MYMATH
#include "mymath.h"
#else
#include <cmath>
#endif
...
int main(int argc ,char* argv[])
{
    ...
#ifdef USE_MYMATH
        double outputValue = mymath::sqrt(inputValue);
#else
        double outputValue = std::sqrt(inputValue);
#endif
    ...
}
复制代码
```

#### 为库添加使用需求

使用需求可以在`CMake`中更好地控制库或可执行文件的链接和包含目录，以及构建目标之间的属性传递。影响使用需求的主要命令有：

- `target_compile_definitions`
- `target_compile_options`
- `target_include_directories`
- `target_link_libraries`

现在让我们重构一下我们的代码，以使用现代的`CMake`方法来添加使用需求。我们首先要求，任何链接了`mymath`库的构建目标都需要包含`mymath`目录，而`mymath`库本身当然不需要，所以这可以是一个接口型（*INTERFACE*）使用需求。

接口型是指消费者（其他使用此库的构建目标）需要而生产者（库自身）不需要的需求。现在让我们在`mymath/CMakeLists.txt`中加入如下内容：

```
target_include_directories(mymath
        INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
)
复制代码
```

`CMAKE_CURRENT_SOURCE_DIR`变量的值是当前由`CMake`处理的源目录的完整路径，在本例中也就是`mymath`目录的完整路径。这样，所有链接了`mymath`库的构建目标就自动包含了`mymath`目录，现在可以将`EXTRA_INCLUDES`安全地删除掉：

```
if(USE_MYMATH)
    add_subdirectory(mymath)
#删除   list(APPEND EXTRA_INCLUDES mymath)
    list(APPEND EXTRA_LIBS mymath)
endif()

target_include_directories(CMakeLearnDemo PUBLIC
        autoGeneratedHeaders
#删除   ${EXTRA_INCLUDES}
)
复制代码
```

#### 使用cmake-gui配置项目、生成构建系统

现在我们的项目有了1个构建选项，而GUI图形界面能使构建选项更直白地向用户体现出来，所以我们这次不使用命令行`cmake`，而是使用`cmake-gui`来配置项目并生成构建系统。

打开`cmake-gui`，首先选择项目源目录和构建目录，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e6bb606af2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

点击Configure，选择`Unix Makefiles`作为构建系统，然后确定，如图：





![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e6bb5825ad?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



确定并等待配置完成后，出现了若干个红色项，这表示当前项目中可由用户进行配置的可选项。我们勾选`USE_MYMATH`选项，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e6e963813f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



再次点击`Configure`，直到没有红色选项了之后，点击`Generate`来生成经过用户配置的项目构建系统，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e6f23f9551?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



构建系统生成之后，观察`projectConfig.h`和`main.cpp`文件的变化，然后就可以进行项目构建了，方法跟之前一样，比如`cmake --build mybuild`。

如果不想使用图形工具，也可以直接在调用命令行`cmake`工具时传递变量值，比如：

```
cmake -B mybuild -S . -D USE_MYMATH:BOOL=ON
复制代码
```

### 7. 安装

所谓安装，可以简单地理解为将软件或程序所需要的若干文件复制到指定位置。那么，对于我们的`CMakeLearnDemo`项目来说，需要安装哪些文件呢？对于`mymath`，需要安装库和头文件；对于整个应用程序，需要安装可执行程序和`projectConfig.h`头文件。

安装规则确定好之后，我们在`mymath/CMakeLists.txt`的末尾加入：

```
install(TARGETS mymath DESTINATION lib)
install(FILES mymath.h DESTINATION include)
复制代码
```

在根`CMakeLists.txt`的末尾加入：

```
install(TARGETS CMakeLearnDemo DESTINATION bin)
install(FILES autoGeneratedHeaders/projectConfig.h DESTINATION include)
复制代码
```

`install`命令用于生成项目的安装规则，即指明需要安装哪些内容。`TARGETS`是安装构建目标；`FILES`是安装文件，如果使用相对路径，则相对于当前`cmake`处理的源目录；`DESTINATION <directory>`是安装路径，如果使用相对路径，则相对于`CMAKE_INSTALL_PREFIX`变量的值。

接下来，让我们打开`cmake-gui`，配置方法和之前一样，只不过这次我们需要设置一下`CMAKE_INSTALL_PREFIX`变量，也就是项目的安装前缀路径，如图：



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1151" height="951"></svg>)



配置项目、生成项目构建系统、构建项目，这3项都完成之后，就可以进行安装了，命令如下：

```
cmake --install mybuild
复制代码
```

完成之后，打开你之前设置的安装前缀路径，不出意外已经有了相应的文件，如图：



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="802" height="408"></svg>)



### 8. 测试

接下来让我们测试`CMakeLearnDemo`应用程序。在根`CMakeLists.txt`的末尾，我们可以启用测试，然后添加一些基本测试来验证应用程序是否正常工作，如下：

```
enable_testing()

function(do_test target arg result)
    add_test(NAME sqrt${arg} COMMAND ${target} ${arg})
    set_tests_properties(sqrt${arg} PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endfunction()

do_test(CMakeLearnDemo 4 "2")
do_test(CMakeLearnDemo 2 "1.414")
do_test(CMakeLearnDemo 5 "2.236")
do_test(CMakeLearnDemo 123.456 "11.111")
do_test(CMakeLearnDemo -4 "NaN|NULL|Null|null|[Ee]rror|[Nn]ot [Ee]xist|[Nn]egative")
复制代码
```

解释：

```
function(<name> [arg1 arg2 ...])
		do sth ...
endfunction()		
复制代码
```

顾名思义，定义一个函数，必须要有一个函数名，参数可选。

`add_test`命令的作用是添加一个测试，`NAME <name>`指定这个测试的名字，`COMMAND <command> [arg ...]`指定测试时调用的命令行，如果`<command>`是一个由`add_execuable()`创建的可执行文件目标，它将自动被替换为构建时生成的可执行文件的路径。

`set_tests_properties`命令的作用是为指定的测试设置属性，`PASS_REGULAR_EXPRESSION`属性的含义是：为了通过测试，命令的输出结果必须匹配这个正则表达式，比如`"\d+(\.\d+)?"`，输出结果是`"result is 1.5"`，则测试通过。要想详细地了解`cmake`中的测试有哪些属性，可以阅读[Properties on Tests](https://cmake.org/cmake/help/latest/manual/cmake-properties.7.html#test-properties)。

项目构建完成后，使用`cd mybuild`跳转到构建目录下，输入`ctest -N`来查看将要运行的测试，但并不实际运行它们，如图：



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="955" height="215"></svg>)



接着运行`ctest -VV`运行测试，并输出详细测试信息，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e7c440d318?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e7c151d545?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 9. 添加系统自检

现在，让我们向`mymath::sqrt`函数中添加一些代码，这些代码所依赖的功能可能在某些目标平台上不支持，所以我们需要检查。我们想要添加的代码是通过下面这个数学公式来计算平方根，需要用到`log`对数函数和`exp`指数函数，如果目标平台不支持这2个函数，则还是使用之前的方法计算：

![\sqrt{x} = e^{ \ln{ \sqrt{x} } } = e^{ 0.5 \times \ln{x} }](https://juejin.im/equation?tex=%5Csqrt%7Bx%7D%20%3D%20e%5E%7B%20%5Cln%7B%20%5Csqrt%7Bx%7D%20%7D%20%7D%20%3D%20e%5E%7B%200.5%20%5Ctimes%20%5Cln%7Bx%7D%20%7D)

现在让我们在`mymath/CMakeLists.txt`中加入如下内容：

```
include(CheckCXXSymbolExists)
check_cxx_symbol_exists(log "cmath" HAVE_LOG)
check_cxx_symbol_exists(exp "cmath" HAVE_EXP)

if(HAVE_LOG AND HAVE_EXP)
    target_compile_definitions(mymath
            PRIVATE "HAVE_LOG" "HAVE_EXP")
endif()
复制代码
```

解释：

`cmake`中有很多可选功能模块，我们可以通过`include`命令来在项目中启用指定模块。

```
check_symbol_exists(<symbol> <files> <variable>)
复制代码
```

启用了`CheckCXXSymbolExists`模块后，此命令才会生效，作用是检查`<symbol>`符号是否在指定的`files` `C++`头文件中可用，符号可以被定义为**宏、变量或者函数名**，如果是**Class、Struct、enum或基本类型名**，则无法被识别；检查的结果值放在`<variable>`中。

`target_compile_definitions`命令的作用是向目标添加编译定义，即`gcc`中的`-D`选项：

```
g++ -D HAVE_LOG -D HAVE_EXP -o mymath.o -c mymath.cpp
复制代码
```

相当于在`mymath.cpp`中手动定义了2个宏：

```
#define HAVE_LOG
#define HAVE_EXP
复制代码
```

概括一下，新添加内容的作用是：检查`cmath`头文件中`log`和`exp`函数是否存在定义并且可用，如果都存在并且可用，则向`mymath`中添加`HAVE_LOG`和`HAVE_EXP`这2个编译定义。

> 也可以使用`configure_file()`的方式，不过要更麻烦些。

接下来让我们对`mymath::sqrt`函数稍作修改，如下：

```
#include "mymath.h"

# if defined(HAVE_LOG) && defined(HAVE_EXP)
#include <cmath>
#endif

namespace mymath
{
    double sqrt(double number)
    {
        static constexpr double precision = 1e-6;
        static constexpr auto abs = [](double n) ->double { return n > 0? n:-n; };

        if(number < 0)
        {
            throw std::invalid_argument("Cannot calculate the square root of a negative number!");
        }

# if defined(HAVE_LOG) && defined(HAVE_EXP)
        return std::exp(0.5 * std::log(number) );
#endif

        double guess = number;
        while( abs(guess * guess - number) > precision)
        {
            guess = ( guess + number / guess ) / 2;
        }

        return guess;
    }
}
复制代码
```

在`CLion`中重新加载`CMakeLists.txt`，不出意外被条件编译包含的那2行现在应该会处于高亮状态，也就是说`log`函数和`exp`函数在当前平台中可用；重新构建运行一下，观察结果是否正常。

### 10. 添加自定义命令和生成文件

假设出于本教程的目的，我们决定不再使用`log`函数和`exp`函数，而是希望生成一个可在`mymath::sqrt`函数中使用的预计算值表。在本节中，我们将在构建过程中创建表，然后将其编译到`mymath`库中。

首先，让我们从`mymath/CMakeLists.txt`和`mymath/mysqrt.cpp`中删除上一节新增的所有内容，然后在`mymath`目录下新增一个`makeSqrtTable.cpp`源文件，用于生成预计算值表的头文件，内容如下：

```cpp
#include <iostream>
#include <fstream>
#include <cmath>

int main(int argc, char* argv[])
{
    // argv[1] : output header file path
    // argv[2] (optional) : max value of precomputed square root

    if(argc < 2)
    {
        std::cerr << "Must have at least 2 command line arguments." << std::endl;
        return 1;
    }

    int maxPrecomputedSqrtValue = 100;
    if(argc >= 3)
    {
        try
        {
            maxPrecomputedSqrtValue = std::stoi(argv[2]);
        }
        catch(const std::invalid_argument& e)
        {
            std::cerr << e.what() << std::endl;
            return 1;
        }
    }

    std::ofstream ofstrm(argv[1], std::ios_base::out | std::ios_base::trunc);
    if(!ofstrm.is_open())
    {
        std::cerr << "Can not open " << argv[1] << " to write!" << std::endl;
        return 1;
    }

    ofstrm << "namespace mymath\n{\n\tstatic constexpr int maxPrecomputedSqrtValue = " << maxPrecomputedSqrtValue << ";\n";
    ofstrm << "\tstatic constexpr double sqrtTable[] =\n\t{\n";

    for(int i = 0; ; i++)
    {
        double precomputedSqrtValue = std::sqrt(i);
        ofstrm << "\t\t" << precomputedSqrtValue;
        if(i == maxPrecomputedSqrtValue)
        {
            ofstrm << "\n\t};\n}";
            break;
        }
        else
        {
            ofstrm << ",\n";
        }
    }

    ofstrm.close();

    return 0;
}
```

接着在`mymath/CMakeLists.txt`中新增如下内容：

```cmake
add_executable(MakeSqrtTable makeSqrtTable.cpp)

add_custom_command(
        OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/sqrtTable.h
        COMMAND MakeSqrtTable ${CMAKE_CURRENT_SOURCE_DIR}/sqrtTable.h 1000
        DEPENDS MakeSqrtTable
)

add_library(mymath STATIC mymath.cpp ${CMAKE_CURRENT_SOURCE_DIR}/sqrtTable.h )
复制代码
```

解释：

第一行不用多说了，后面的`add_custom_command`命令的作用是将自定义构建规则添加到构建系统的生成中，有多种用法，在本例中它的作用是定义用于生成指定输出文件的命令。`OUTPUT output1 [...]`声明了有哪些输出文件；`COMMAND commands [args ...]`声明了在构建时所要执行的命令，在本例中我们调用`MakeSqrtTable`并传递了2个参数，一个是输出文件路径，另一个是最大预计算平方根数值，由于`MakeSqrtTable`是一个构建目标，所以会自动创建一个目标级别的依赖项，以确保在调用此命令之前先构建目标；`DEPENDS [depend ...]`声明了执行此命令所依赖的文件，当依赖是构建目标时，它也会创建一个目标级别的依赖项，除此之外，如果构建目标是可执行文件或库，它还会创建一个文件级别的依赖项，以在重新编译此构建目标时重新运行自定义命令。

有人可能会有疑问，既然已经在`COMMAND`中将输出文件路径作为参数传过去了，那么`OUTPUT output1 [...]`选项的作用是什么？事实上，自定义命令是在构建目标在构建过程中被调用的，是哪个构建目标？是指在同一个`CMakeLists.txt`中将`add_custom_command`中的`OUTPUT`选项中声明的任意输出文件指定为源文件的构建目标，在本例中就是`mymath`库，如果没有任何构建目标用到输出文件，则此自定义命令也不会被调用，虽然头文件是在`cpp`中被包含使用的，不需要在添加构建目标命令中显示指明，但是为了使自定义命令被调用，这种情况下就需要显示指明了；同时，不要在1个以上可能并行构建的独立目标中列出输出文件，否则产生的输出实例可能会有冲突。

最后，修改`mymath::sqrt`函数为如下内容：

```cpp
#include "mymath.h"
#include "sqrtTable.h"
#include <iostream>

namespace mymath
{
    double sqrt(double number)
    {
        static constexpr double precision = 1e-6;
        static constexpr auto abs = [](double n) ->double { return n > 0? n:-n; };

        if(number < 0)
        {
            throw std::invalid_argument("Cannot calculate the square root of a negative number!");
        }

        int integerPart = static_cast<int>(number);
        if(integerPart <= maxPrecomputedSqrtValue && abs(number - integerPart) <= precision)
        {
            std::cout << "use precomputed square root : " << integerPart << std::endl;
            return sqrtTable[integerPart];
        }

        double guess = number;
        while( abs(guess * guess - number) > precision)
        {
            guess = ( guess + number / guess ) / 2;
        }

        return guess;
    }
}
复制代码
```

重新构建并运行，检查`mymath::sqrt`是否使用了预计算表，结果示例如下图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e7c3b0c23a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 11. 打包项目

这是整个教程的最后一步，我们要做的是使用`cpack`工具打包项目，打包有2种形式：**源代码包**和**二进制安装包**：

源代码包是指将软件某个版本的源代码打包，这样发布出去后，下载的用户就可以根据自己的需求进行配置、构建和安装。软件包可以是多种形式：`tar.gz`、`.zip`、`.7z`等。

二进制安装包是指作者预先将某个版本的软件构建好，并将安装文件（由`install()`命令所指定的）打包成一个软件包供用户安装。软件包可以是多种形式：简单的`tar.gz`压缩包形式、`.sh`shell脚本形式、`debian`系下的`.deb`安装包形式、`Windows`系统下的安装包形式等等。

现在我们来简单说一下在项目中使用`cpack`打包的工作流程：

- 对于每种安装程序或软件包格式，`cpack`都有一个特定的后端处理程序，称为“生成器”，它负责生成所需的安装包并调用特定的程序包创建工具。

- 我们可以在`CMakeLists.txt`中设置相关`cmake`变量的值来控制所生成软件包的各种属性，也就是所谓的“定制化”。所有形式软件包的都有一些公共的属性，比如`CPACK_PACKAGE_NAME`软件包名、`CPACK_PACKAGE_VERSION`软件包版本等等，当然每个软件包也有它们独有的一些属性可以设置。设置完相关属性后，最后包含`cpack`模块：

    ```cmake
    include(CPack)
    复制代码
    ```

- 在生成项目构建系统的过程中，`cmake`会根据我们上述设置的一些属性，在构建目录下生成2个配置文件：`CPackConfig.cmake`和`CPackSourceConfig.cmake`，一个用于控制二进制安装包的生成，一个用于控制源代码包的生成。

- 项目构建系统生成后，就可以在构建目录下使用`cpack`命令行工具生成源代码包；项目构建完成后，就可以生成二进制安装包。默认是生成二进制安装包，如果要生成源代码包，则需要指定,如：

    ```cmake
    cpack --config CPackSourceConfig.cmake -G tar.gz
    复制代码
    ```

 `cpack`还有许多其他选项可以设置，具体请参考[cpack options](https://cmake.org/cmake/help/latest/manual/cpack.1.html#options)。

虽然本项目只是一个教程，但既然是要生成软件包供用户使用，还是添加一个软件许可证说明文件会显得更正规哈，在项目根目录下新建一个`LICENSE`文件，内容如下：

```cmake
Copyright (c) 2019: Siwei Zhu

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```

设置软件包的各种属性可能会占据很多行，这会使`CMakeLists.txt`文件中的内容增加很多，如果将这些内容单独放到一个文件中则会更加便于项目配置管理。事实上，`cmake`代码除了放在`CMakeLists.txt`中，还可以放在以`.cmake`作为扩展名的文件中。`include()`命令可以包含`cmake`模块或者`.cmake`文件，跟`c++`中的`#include`类似，其实就是将其他文件中的`cmake`代码包含进来，每个`cmake`模块都对应着一个`<module_name>.cmake`文件，所以包含模块与包含`.cmake`文件的本质其实是一样的。接下来让我们在项目根目录下创建一个`ProjectCPack.cmake`文件，用于配置项目的安装及打包，然后在根`CMakeLists.txt`文件中删掉`install(xxx)`的那几行，并替换为`include(ProjectCPack.cmake)`。最后，`ProjectCPack.cmake`的内容如下：

```cmake
# 安装内容
install(TARGETS CMakeLearnDemo DESTINATION bin)
install(FILES autoGeneratedHeaders/projectConfig.h DESTINATION include)
install(FILES LICENSE DESTINATION .)

# 设置包的名称
set(CPACK_PACKAGE_NAME "${PROJECT_NAME}")
# 设置包的提供商
set(CPACK_PACKAGE_VENDOR "siwei Zhu")
# 设置包描述信息
set(CPACK_PACKAGE_DESCRIPTION "a simple cmake learn demo.")
# 设置LICENSE许可证
set(CPACK_RESOURCE_FILE_LICENSE "${PROJECT_SOURCE_DIR}/LICENSE")
# 设置包版本信息
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
# 设置要生成的包文件的名称，不包括扩展名
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${CPACK_SYSTEM_NAME}")
# 设置源代码包忽略文件，类似gitignore
set(CPACK_SOURCE_IGNORE_FILES "${PROJECT_BINARY_DIR};/cmake-build-debug/;/.git/;.gitignore")
# 设置源代码包生成器列表
set(CPACK_SOURCE_GENERATOR "ZIP;TGZ")
# 设置二进制包生成器列表
set(CPACK_GENERATOR "ZIP;TGZ")
# 设置包安装前缀目录
set(CPACK_PACKAGING_INSTALL_PREFIX "/opt/${PROJECT_NAME}")

if(UNIX AND CMAKE_SYSTEM_NAME MATCHES "Linux")
    # 添加deb安装包的生成器
    list(APPEND CPACK_GENERATOR "DEB")
    # 包维护者
    set(CPACK_DEBIAN_PACKAGE_MAINTAINER "siwei Zhu")
    # 包分类，devel是指开发工具类软件
    set(CPACK_DEBIAN_PACKAGE_SECTION "devel")
    # 包依赖
    set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6")
elseif(WIN32 OR MINGW)
    # 添加Windows NSIS安装包的生成器
    list(APPEND CPACK_GENERATOR "NSIS")
    # 设置包安装目录
    set(CPACK_PACKAGE_INSTALL_DIRECTORY "${PROJECT_NAME}")
    # NSIS安装程序提供给最终用户的默认安装目录位于此根目录下。
    # 呈现给最终用户的完整目录是：${CPACK_NSIS_INSTALL_ROOT}/${CPACK_PACKAGE_INSTALL_DIRECTORY}
    set(CPACK_NSIS_INSTALL_ROOT "C:\\Program Files\\")
    # 设置有关安装过程的问题和意见的联系信息
    set(CPACK_NSIS_CONTACT "siwei Zhu")
    # 首先询问卸载以前的版本。如果设置为ON，
    # 则安装程序将查找以前安装的版本，如果找到，则在继续安装之前询问用户是否要卸载它。
    set(CPACK_NSIS_ENABLE_UNINSTALL_BEFORE_INSTALL ON)
endif()

include(CPack)
复制代码
```

在上述代码中，我们先设置了要安装的文件，软件包的若干属性，最后包含`CPack`模块。其中，源代码包和二进制包都需要生成`.zip`包和`.tar.gz`包，如果当前系统是`Linux`，还生成`.deb`包，如果是`Windows`系统或者编译器是`MINGW`，还生成`NSIS`安装包（`Windows`下的一款安装包制作工具）。

重新生成项目构建系统并构建，完成后，跳转到`mybuild`目录下，运行如下2条命令：

```
cpack
cpack --config CPackSourceConfig.cmake
复制代码
```

来生成源代码包和二进制包（如果没有使用`-G`选项，则生成所有由`CPACK_GENERATOR`或`CPACK_SOURCE_GENERATOR`变量所指定的软件包类型）。

打开`mybuild`目录，不出意外应该已经生成了若干软件包，如图：



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="249"></svg>)



我们双击`CMakeLearnDemo-1.0.0-Source.tar.gz`文件，使用归档管理器查看该源代码包的目录结构，如下：



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="692" height="529"></svg>)



我们双击`CMakeLearnDemo-1.0.0-.deb`来安装我们的二进制软件包，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e8826d6f94?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



安装完成后，来到`/opt`目录，可以看到已经有了安装文件：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e8a2737198?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



打开终端，输入：

```
sudo dpkg -l | grep cmakelearndemo
复制代码
```

结果如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e880d28e69?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



可以看到已经有了此软件包的安装记录，最后输入：

```
sudo apt remove cmakelearndemo
复制代码
```

来卸载此软件包，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e8c385ed4b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## CMake学习心得及资源分享

最后，来说一下学习`CMake`的一些方法及资源分享：

首先，上述给出的入门教程应该已经囊括大部分常见的`cmake`命令及方法了，官方的`cmake tutorial`总共有十几个steps，本文章只包括了前7个，原因是`cmake`的官方文档写得不怎么样，倒不是说内容不详细，主要是缺乏使用案例，而且有些地方不合逻辑，特别是这个`cmake tutorial`，刚开始的几个steps看起来比较顺畅，一气呵成的感觉，越看到后面越让人头大，有些地方莫名其妙就新增了一个文件，结果它一句也不提，文件内容也不给，真是令人窒息；还有一个原因是后面的steps的一些功能用的比较少，也不适合放在入门教程里，有空的话我可以单独拿出来写一篇文章。

第二，说一下[官方文档](https://cmake.org/cmake/help/latest/index.html)的一些使用方法，文档首页是一些`topics`，你可以针对性的去看，比如说我要看一下哪些`cmake`变量可以在项目配置时用到，那就选择`cmake-variables`，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e933c542c4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



更加细化一点，如果想搜要索特定的某个命令或者变量的详细用法和作用，可以在左边的搜索框里直接输入你想要搜索的内容，如图：



![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e932d56d31?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e93b3ca235?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





![img](https://user-gold-cdn.xitu.io/2019/12/7/16edf0e938827285?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)