## cmake语法规则



cmake变量与shell变量使用不同 shell变量取值是用$() 而cmake变量取值是用${}是用取值

还用在cmake指令中参数之间之间是用逗号或者空格分开

**举例：**

  ADD_EXECUTABLE指令中通常目标文件只用要跟多个源文件多个源文件之间使用空格或者逗号隔开

cmake指令不分大小写，都可以但是通常变量使用的是小写，指令使用的大写。

cmake的环境变量是使用**$ENV{}**取值，使用**SET(ENV{VAR} VALUE)**赋值

## cmake常用指令介绍

### project

指令原型：PROJECT(projectname [CXX] [C] [Java])

指令功能：指定工程名称，并可指定工程支持的语言。支持语言列表可忽略，默认支持所有语言

### set

指令原型：SET(VAR [VALUE]  [CACHE TYPE DOCSTRING [FORCE]])

指令功能：定义变量(可以定义多个VALUE，如SET(SRC_LIST main.c util.c reactor.c))

### message

指令原型：MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] “message to display” …)

指令功能：向终端输出用户定义的信息或变量的值

- SEND_ERROR, 产生错误,生成过程被跳过
- STATUS, 输出前缀为—的信息
- FATAL_ERROR, 立即终止所有cmake过程

### add_executable

指令原型：ADD_EXECUTABLE(bin_file_name ${SRC_LIST})

指令功能：生成可执行文件

### add_library

指令原型：ADD_LIBRARY(libname [SHARED | STATIC | MODULE] [EXCLUDE_FROM_ALL] SRC_LIST)

指令功能：生成动态库或静态库
  - SHARED 动态库
  - STATIC 静态库

- MODULE 在使用dyld的系统有效,若不支持dyld,等同于SHARED
- EXCLUDE_FROM_ALL 表示该库不会被默认构建

### cmake_minmum_required

指令原型：CMAKE_MINIMUM_REQUIRED(VERSION version_number [FATAL_ERROR])

指令功能：声明CMake的版本要求

### **ADD_SUBDIRECTORY**

指令原型：ADD_SUBDIRECTORY(src_dir [binary_dir] [EXCLUDE_FROM_ALL])

指令功能：向当前工程添加存放源文件的子目录,并可以指定中间二进制和目标二进制的存放位置

-   EXCLUDE_FROM_ALL含义：将这个目录从编译过程中排除

### INCLUDE_DIRECTORIES

指令原型：INCLUDE_DIRECTORIES([AFTER | BEFORE] [SYSTEM] dir1 dir2 … )

指令功能：向工程添加多个特定的头文件搜索路径,路径之间用空格分隔,如果路径包含空格,可以使用双引号将它括起来,默认的行为为追加到当前头文件搜索路径的后面。有如下两种方式可以控制搜索路径添加的位置：

- CMAKE_INCLUDE_DIRECTORIES_BEFORE,通过SET这个cmake变量为on,可以将添加的头文件搜索路径放在已有路径的前面
- 通过AFTER或BEFORE参数,也可以控制是追加还是置前

### **LINK_DIRECTORIES**

指令原型：LINK_DIRECTORIES(dir1 dir2 …)

指令功能：添加非标准的共享库搜索路径

### TARGET_LINK_LIBRARIES

指令原型：TARGET_LINK_LIBRARIES(target lib1 lib2 …)

指令功能：为target添加需要链接的共享库

### **ADD_DEFINITIONS**

指令原型：ADD_DEFINITIONS(-DENABLE_DEBUG -DABC),参数之间用空格分隔

指令功能：向C/C++编译器添加-D定义

### **INCLUDE_DIRECTORIES**

指令原型：INCLUDE_DIRECTORIES([AFTER | BEFORE] [SYSTEM] dir1 dir2 … )

指令功能：向工程添加多个特定的头文件搜索路径,路径之间用空格分隔,如果路径包含空格,可以使用双引号将它括起来,默认的行为为追加到当前头文件搜索路径的后面。有如下两种方式可以控制搜索路径添加的位置：
  - CMAKE_INCLUDE_DIRECTORIES_BEFORE,通过SET这个cmake变量为on,可以将添加的头文件搜索路径放在已有路径的前面
  - 通过AFTER或BEFORE参数,也可以控制是追加还是置前

### **LINK_DIRECTORIES**

指令原型：LINK_DIRECTORIES(dir1 dir2 …)

指令功能：添加非标准的共享库搜索路径

### **TARGET_LINK_LIBRARIES**

  指令原型：TARGET_LINK_LIBRARIES(target lib1 lib2 …)

  指令功能：为target添加需要链接的共享库

### AUX_SOURCE_DIRECTORY

指令原型：AUX_SOURCE_DIRECTORY(dir VAR)

指令功能：发现一个目录下所有的源代码文件并将列表存储在一个变量中，把当前目录下的所有源码文件名赋给变量DIR_HELLO_SRCS

### **EXEC_PROGRAM**

指令原型：EXEC_PROGRAM(Executable [dir where to run] [ARGS <args>][OUTPUT_VARIABLE <var>] [RETURN_VALUE <value>])

指令功能：用于在指定目录运行某个程序（默认为当前CMakeLists.txt所在目录）,通过ARGS添加参数,通过OUTPUT_VARIABLE和RETURN_VALUE获取输出和返回值,如下示例





## cmake构建多个版本hello word项目

### 单目录单文件

项目结构

```C++
.
├── CMakaLists.txt
└── main.cpp

```

main.cpp



```C++
  1 #include<iostream>
  2 using namespace std;
  3 int main(){
  4   cout<<"hello word"<<endl;                                                                                                                                              
  5 }

```

CMakeLists.txt



```C++
  1 #指定cmake的最低版本
  2 CMAKE_MINIMUM_REQUIRED(version 2.8)
  3 #设置项目名称 *                                                                                                                                                           
  4 project(main)                       
  5 #生成可执行文件                     
  6 add_executable(main main.cpp)*
```

执行步骤：



```C++
cmake .   #链接到当前目录
[root@iZ8vbhcpwdmnwpx91dy1h8Z demo1]# cmake .
-- The C compiler identification is GNU 4.8.5
-- The CXX compiler identification is GNU 4.8.5
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/linux/cmake/demo1
[root@iZ8vbhcpwdmnwpx91dy1h8Z demo1]# make   #make构建项目
Scanning dependencies of target main
[ 50%] Building CXX object CMakeFiles/main.dir/main.cpp.o
[100%] Linking CXX executable main
[100%] Built target main
[root@iZ8vbhcpwdmnwpx91dy1h8Z demo1]# ./main
hello word


```

### 单目录多文件

项目结构



```C++
.
├── CMakeLists.txt
├── main1.cpp
├── main2.cpp
├── main.cpp
└── main.h

```

main.h



```C++
#ifndef __MAIN_H__
#define __MAIN_H__
int add(int a,int b);

#endif

```

main1.cpp



```C++
#include<iostream>
#include"main.h"
int add(int a,int b){
  return a+b;
}

```

main2.cpp



```C++
#include<iostream>
#include"main.h"
int main(){
  cout<<add(1,2)<<endl;
}
```

CMakeLists.txt



```C++
#指定cmake的最低版本
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
#设置项目名称                                                                                                                                                            
project(main)                       
#生成可执行文件                     
add_executable(main main1.cpp main2.cpp)

```

执行



```C++
[root@iZ8vbhcpwdmnwpx91dy1h8Z demo2]# cmake .
-- The C compiler identification is GNU 4.8.5
-- The CXX compiler identification is GNU 4.8.5
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/linux/cmake/demo2
[root@iZ8vbhcpwdmnwpx91dy1h8Z demo2]# make
Scanning dependencies of target main
[ 33%] Building CXX object CMakeFiles/main.dir/main1.cpp.o
[ 66%] Building CXX object CMakeFiles/main.dir/main2.cpp.o
[100%] Linking CXX executable main
[100%] Built target main
[root@iZ8vbhcpwdmnwpx91dy1h8Z demo2]# ./main
3

```

执行完项目结构



```C++
.
├── CMakeCache.txt
├── CMakeFiles
│   ├── 3.9.0-rc3
│   │   ├── CMakeCCompiler.cmake
│   │   ├── CMakeCXXCompiler.cmake
│   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   ├── CMakeSystem.cmake
│   │   ├── CompilerIdC
│   │   │   ├── a.out
│   │   │   ├── CMakeCCompilerId.c
│   │   │   └── tmp
│   │   └── CompilerIdCXX
│   │       ├── a.out
│   │       ├── CMakeCXXCompilerId.cpp
│   │       └── tmp
│   ├── cmake.check_cache
│   ├── CMakeDirectoryInformation.cmake
│   ├── CMakeOutput.log
│   ├── CMakeTmp
│   ├── feature_tests.bin
│   ├── feature_tests.c
│   ├── feature_tests.cxx
│   ├── main.dir
│   │   ├── build.make
│   │   ├── cmake_clean.cmake
│   │   ├── CXX.includecache
│   │   ├── DependInfo.cmake
│   │   ├── depend.internal
│   │   ├── depend.make
│   │   ├── flags.make
│   │   ├── link.txt
│   │   ├── main1.cpp.o
│   │   ├── main2.cpp.o
│   │   └── progress.make
│   ├── Makefile2
│   ├── Makefile.cmake
│   ├── progress.marks
│   └── TargetDirectories.txt

```

### 多文件多目录

项目结构



```C++
.
├── build
├── CMakeLists.txt
├── code
│   ├── CMakeLists.txt
│   └── main.cpp
└── lib
    ├── add.cpp
    ├── CMakeLists.txt
    ├── del.cpp
    └── main.h

```

顶级CMakeLists.txt



```C++
#添加当前目录存放源文件的目录
aDD_SUBDIRECTORY(./code)
ADD_SUBDIRECTORY(./lib)

```

code 目录下文件

main.cpp



```C++
#include"../lib/main.h"
#include<iostream>
using namespace std;
int main(){
  cout<<add(1,2)<<endl;
  cout<<del(2,1)<<endl;
}

```

CMakeLists.txt

```C++

AUX_SOURCE_DIRECTORY(. LIST_ARRAY)

ADD_EXECUTABLE(demo ${LIST_ARRAY})
#为demo链接共享库
TARGET_LINK_LIBRARIES(demo main)

```

lib文件夹下



CMakeLists.txt

```C++
#收集当前目录的所有源文件放在LIST_ARRAY变量中
AUX_SOURCE_DIRECTORY(. LIST_ARRAY)
#将该目录下源文件生成共享库      
ADD_LIBRARY(main SHARED ${LIST_ARRAY})

```

main.h



```C++
#ifndef __MAIN_H__
#define __MAIN_H__

int add(int a,int b);
int del(int a,int b);
#endif

```

add.cpp



```C++
#include"main.h"
int add(int a,int b){
  return a+b;
}

```

del.cpp



```C++
#include"main.h"
int del(int a,int b){
  return a-b;
}

```

项目执行过程



```C++
[root@iZ8vbhcpwdmnwpx91dy1h8Z demo3]# cd build
[root@iZ8vbhcpwdmnwpx91dy1h8Z build]# ls
[root@iZ8vbhcpwdmnwpx91dy1h8Z build]# cmake ..   #将项目文件链接到子目录执行
build文件结构
.
├── CMakeCache.txt
├── CMakeFiles
│   ├── 3.9.0-rc3
│   │   ├── CMakeCCompiler.cmake
│   │   ├── CMakeCXXCompiler.cmake
│   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   ├── CMakeSystem.cmake
│   │   ├── CompilerIdC
│   │   │   ├── a.out
│   │   │   ├── CMakeCCompilerId.c
│   │   │   └── tmp
│   │   └── CompilerIdCXX
│   │       ├── a.out
│   │       ├── CMakeCXXCompilerId.cpp
│   │       └── tmp
│   ├── cmake.check_cache
│   ├── CMakeDirectoryInformation.cmake
│   ├── CMakeOutput.log
│   ├── CMakeTmp
│   ├── feature_tests.bin
│   ├── feature_tests.c
│   ├── feature_tests.cxx
│   ├── Makefile2
│   ├── Makefile.cmake
│   ├── progress.marks
│   └── TargetDirectories.txt
├── cmake_install.cmake
├── code
│   ├── CMakeFiles
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── demo.dir
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── CXX.includecache
│   │   │   ├── DependInfo.cmake
│   │   │   ├── depend.internal
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   ├── main.o
│   │   │   └── progress.make
│   │   └── progress.marks
│   ├── cmake_install.cmake
│   ├── demo
│   └── Makefile
├── lib
│   ├── CMakeFiles
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── main.dir
│   │   │   ├── add.o
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── CXX.includecache
│   │   │   ├── del.o
│   │   │   ├── DependInfo.cmake
│   │   │   ├── depend.internal
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   └── progress.make
│   │   └── progress.marks
│   ├── cmake_install.cmake
│   ├── libmain.so
│   └── Makefile
└── Makefile

13 directories, 53 files

[root@iZ8vbhcpwdmnwpx91dy1h8Z build]# cd code   ##这里要切换到build下的code文件下才能执行
[root@iZ8vbhcpwdmnwpx91dy1h8Z code]# ll
total 28
drwxr-xr-x 3 root root 4096 Jun  8 21:04 CMakeFiles
-rw-r--r-- 1 root root  983 Jun  8 21:04 cmake_install.cmake
-rwxr-xr-x 1 root root 9128 Jun  8 21:04 demo
-rw-r--r-- 1 root root 5091 Jun  8 21:04 Makefile
[root@iZ8vbhcpwdmnwpx91dy1h8Z code]# ./demo
3
1

```

