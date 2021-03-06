---
layout: wiki
title: CMake
categories: Linux
description: 学会CMake的使用和CMakeLists的编写
keywords: CMake, CMakeLists
---

# 基本语法规则

* 命令不区分大小写；
* 以“#”号为注释符号；
* 命令由命令名称、小括号、参数构成，其中参数之间以空格分开；
* CMake工程中源文件包含：CMakeLists.txt,<script>.cmake,<module>.cmake;
* CMake工程的入口是顶层目录CMakeLists.txt所在目录，通过**add_subdirectory()**指令添加子目录，每个子目录包含CMakeLists.txt将对应被CMake构建成一个树形的子工程；
* <script>.cmake不能构建工程系统，只能作为CMake在命令行下运行的脚本；
* 在CMakeLists.txt或<script>.cmake中通过include()命令加载<module>.cmake，也可以通过**CMAKE_MODULE_PATH**变量来搜索<module>.cmake；
* 通过${variable_name}在参数(引号变量或非引号变量)中引用变量，\$ENV{VAR}来引用环境变量；
* if()/elseif()/else()/endif()作为条件控制语言，判断条件中引用变量直接是变量名；
* foreach()/endforeach()和while()/endwhile()作为循环语句；
* 自定义命令：macro()/endmacro()和function()/endfunction();
* 变量：set()/unset()设置和取消设置变量，变量是大小写敏感的，字符串类型；
* 变量存活周期：function()内变量仅在函数内有用，子目录CMakeLists.txt继承父目录变量；
* CACHE变量：set()unset()设置和取消，在程序run周期内永久存在；
* 列表1：set(srcs a.c b.c c.c) # sets "srcs" to "a.c;b.c;c.c"；
* 列表2：set(x a "b;c") # sets "x" to "a;b;c", not "a;b\;c"；

# 基本使用

## 1. 预定义变量解释
* **CMAKE/PROJECT_BINARY_DIR**：工程编译发生的当前目录；
* **CMAKE/PROJECT_SOURCE_DIR**：工程的顶级目录；
* **CMAKE_CURRENT_SOURCE_DIR**:当前处理的cmakelist目录；
* **CMAKE_CURRENT_BINARY_DIR**: Target编译目录，可通过add-subdirectory来修改目录名；
* **CMAKE_LIBRARY_OUTPUT_DIRECTORY**：
* **CMAKE_RUNTIME_OUTPUT_DIRECTORY**：
* **EXECUTABLE_OUTPUT_PATH**：可执行文件最终输出目录，如果区分版本，则会建立相应的子目录；
* **LIBRARY_OUTPUT_PATH**：静态库、动态库最终输出目录；
* **CMAKE_CXX_FLAGS**:设置 C ++编译选项,也可以通过指令 ADD_DEFINITIONS()添加;
* **CMAKE_C_FLAGS**:设置 C 编译选项,也可以通过指令 ADD_DEFINITIONS()添加
* **CMAKE_BUILD_TYPE**: 可以取值**Debug, Release, RelWithDebInfo and MinSizeRel**, 仅对单配置产生器有用，如Makefile Generators or Ninja Generator. 不包含IDE Build Tool Generators，如visual studio Generators.
* **CMAKE_CURRENT_LIST_LINE**：当前所在的行；
* **CMAKE_CURRENT_LIST_FILE**：包含Cmakelists文件名的完整路径；
* **CMAKE_CURRENT_LIST_DIR**：当前文件所在路径；
* **CMAKE_LIBRARY_OUTPUT_DIRECTORY**：target的输出类型：Module库总是被作为LIBRARY target，此变量在源工程顶层中配置，优先级高于**EXECUTABLE_OUTPUT_PATH**(猜测)；
* **CMAKE_ARCHIVE_OUTPUT_DIRECTORY**：静态库总是被作为ARCHIVE target，windows下，对应的Import library作为ARCHIVE target；
* **CMAKE_RUNTIME_OUTPUT_DIRECTORY**：可执行文件总是被作为RUMTIME target，在Windows平台，shared library的DLL部分作为RUNTIME target；
* **CMAKE_<CONFIG>_POSTFIX**：设置输出文件后缀，CONFIG为对应DEBUG或RELEASE版本；

## 2. 常用指令解析
* **ADD_EXECUTABLE(target source1 source2)**: 指定生成可执行文件；
* **ADD_LIBRARY(name [SHARED\|STATIC\|MODULE][EXCLUDE_FROM_ALL]source1 source2 ... sourceN)**：指定由source源文件生成静态库或动态库libname.a或libname.so（name唯一，不能重名）；
* **ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])**：添加源文件目录、设置源文件输出目录(包含生成的中间文件，默认目录名与source-dir一致，采用相对目录)、排除目录；
* **ADD_DEPENDENCIES**：
* **ADD_DEFINITIONS**: 向 C/C++编译器添加-D 定义,如
**ADD_DEFINITIONS(-DENABLE_DEBUG -DABC) //参数之间用空格分割**, 在代码中通过#ifdef ABD判断使用；
* **SET_TARGET_PROPERTIES(target1 target2 ...PROPERTIES prop1 value1prop2 value2 ...)**：设置目标属性；
* **INCLUDE\_DIRECTORIES([AFTER\|BEFORE] [SYSTEM] dir1 dir2 ...)**:工程中添加头文件搜索目录，目录中有空格则用双引号括起来；
* **LINK_DIRECTORIES(directory1 directory2 ...)**：添加链接库搜索目录；
* **TARGET_LINK_LIBRARIES(target library1<debug | optimized> library2...)**：为指定目标设置链接库；
* **message([status\|warning\|send_error\|fatal_error] “some message”)**:打印输出信息；
* **option(address "This is a option for address" ON)**：设置cmake编译选项，以及默认值；
* **aux_source_directory(dir  variable)**:搜集所有在指定路径下的源文件的文件名，将输出结果列表储存在指定的变量中;
* **INSTALL(TARGET target1 DISTINATION dst1)**: 拷贝target1至dst1目录；


## 3. 简单的开始
```cmake
cmake_minimum_required (VERSION 2.6)
project (Tutorial)

# The version number.
set (Tutorial_VERSION_MAJOR 1)
set (Tutorial_VERSION_MINOR 0)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file (
  "${PROJECT_SOURCE_DIR}/TutorialConfig.h.in"
  "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  )

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
include_directories("${PROJECT_BINARY_DIR}")

# add the executable
add_executable(Tutorial tutorial.cxx)
```

## 3. 构建静态库和动态库
```cmake
### 在t3文件夹的src下编写hello.c和hello.h文件，如下为CMakeLists.txt文件：
# 设置静态库和动态链接库放置目录，不包括中间生成文件；
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

SET(LIBHELLO_SRC hello.c)
# 生成libhello.so动态链接库；
ADD_LIBRARY(hello SHARED ${LIBHELLO_SRC})
生成libhello_static.a静态链接库；
ADD_LIBRARY(hello_static STATIC ${LIBHELLO_SRC})
#修改静态链接库名字为libhello.a;
SET_TARGET_PROPERTIES(hello_static PROPERTIES OUTPUT_NAME "hello")
# 设置不清理同名target；
SET_TARGET_PROPERTIES(hello PROPERTIES CLEAN_DIRECT_OUTPUT 1)
SET_TARGET_PROPERTIES(hello_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)
# 设置版本号；
SET_TARGET_PROPERTIES(hello PROPERTIES VERSION 1.2 SOVERSION 1)
```

## 4. 使用外部链接库和头文件
```cmake
### 在t4目录下的src新建main.c文件，调t3中libhello.so中的函数，并添加CMakeLists.txt文件，如下所示：

INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/../t3/src)
LINK_DIRECTORIES(${PROJECT_SOURCE_DIR}/../t3/build/lib)
ADD_EXECUTABLE(main main.c)
TARGET_LINK_LIBRARIES(main libhello.so)
# 放在ADD_EXECUTABLE之后才不会报错，这根LIBRARY不一样；
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

# 在build下执行：cmake ..
# 然后执行：make VERBOSE=1
# 使用指令： ldd bin/main 查看可执行程序的链接情况；

### 在搜索头文件和库文件中，可以使用这两个环境变量来弥补CMAKE_INCLUDE_PATH和CMAKE_LIBRARY_PATH
## 在上述CMakeLists.txt中将INCLUDE_DIRECTORIES替换为：

FIND_PATH(myHeader hello.h)
IF(myHeader)
INCLUDE_DIRECTORIES(${myHeader})
ENDIF(myHeader)
```
