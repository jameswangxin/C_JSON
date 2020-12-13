# 从零开始的 JSON 库教程（一）：启程

## 1. JSON 是什么

JSON（JavaScript Object Notation）是一个用于数据交换的文本格式，现时的标准为[ECMA-404](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)。

虽然 JSON 源至于 JavaScript 语言，但它只是一种数据格式，可用于任何编程语言。现时具类似功能的格式有 XML、YAML，当中以 JSON 的语法最为简单。

例如，一个动态网页想从服务器获得数据时，服务器从数据库查找数据，然后把数据转换成 JSON 文本格式：

~~~js
{
    "title": "Design Patterns",
    "subtitle": "Elements of Reusable Object-Oriented Software",
    "author": [
        "Erich Gamma",
        "Richard Helm",
        "Ralph Johnson",
        "John Vlissides"
    ],
    "year": 2009,
    "weight": 1.8,
    "hardcover": true,
    "publisher": {
        "Company": "Pearson Education",
        "Country": "India"
    },
    "website": null
}
~~~

网页的脚本代码就可以把此 JSON 文本解析为内部的数据结构去使用。

从此例子可看出，JSON 是树状结构，而 JSON 只包含 6 种数据类型：

* null: 表示为 null
* boolean: 表示为 true 或 false
* number: 一般的浮点数表示方式，在下一单元详细说明
* string: 表示为 "..."
* array: 表示为 [ ... ]
* object: 表示为 { ... }

我们要实现的 JSON 库，主要是完成 3 个需求：

1. 把 JSON 文本解析为一个树状数据结构（parse）。
2. 提供接口访问该数据结构（access）。
3. 把数据结构转换成 JSON 文本（stringify）。

![requirement](images/requirement.png)

我们会逐步实现这些需求。在本单元中，我们只实现最简单的 null 和 boolean 解析。

## 2. 搭建编译环境
JSON 库名为 leptjson，代码文件只有 3 个：

1. `leptjson.h`：leptjson 的头文件（header file），含有对外的类型和 API 函数声明。
2. `leptjson.c`：leptjson 的实现文件（implementation file），含有内部的类型声明和函数实现。此文件会编译成库。
3. `test.c`：我们使用测试驱动开发（test driven development, TDD）。此文件包含测试程序，需要链接 leptjson 库。

为了方便跨平台开发，使用一个现时最流行的软件配置工具 [CMake](https://cmake.org/)。

在 Windows 下，下载安装 CMake 后，可以使用其 cmake-gui 程序：

![cmake-gui](images/cmake-gui.png)

先在 "Where is the source code" 选择 json-tutorial/tutorial01，再在 "Where to build the binary" 键入上一个目录加上 /build。

按 Configure，选择编译器，然后按 Generate 便会生成 Visual Studio 的 .sln 和 .vcproj 等文件。注意这个 build 目录都是生成的文件，可以随时删除，也不用上传至仓库。

在 OS X 下，建议安装 [Homebrew](https://brew.sh/)，然后在命令行键入：

~~~
$ brew install cmake
$ cd github/json-tutorial/tutorial01
$ mkdir build
$ cd build
$ cmake -DCMAKE_BUILD_TYPE=Debug ..
$ make
~~~

这样会使用 GNU make 来生成项目，把 Debug 改成 Release 就会生成 Release 配置的 makefile。

## 练习

1. 修正关于 `LEPT_PARSE_ROOT_NOT_SINGULAR` 的单元测试，若 json 在一个值之后，空白之后还有其它字符，则要返回 `LEPT_PARSE_ROOT_NOT_SINGULAR`。
2. 参考 `test_parse_null()`，加入 `test_parse_true()`、`test_parse_false()` 单元测试。
3. 参考 `lept_parse_null()` 的实现和调用方，解析 true 和 false 值。