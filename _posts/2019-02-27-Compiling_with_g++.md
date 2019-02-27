---
layout:     post
title:      Compiling with g++
subtitle:   由debug版本比release版本跑得快引发的血案
date:       2019-02-27
author:     Jie
header-img: img/post-bg-debug.png
catalog: true
tags:
    - C++
---

今天跑实验的时候遇到了一件怪事，release版本的executable竟然比debug版本的要慢大概50%左右

```
cmake .
cmake . -DCMAKE_BUILD_TYPE=Release
# 第一个要明显快于第二个（release）
# 原CMakeLists.txt如下：
SET ( CMAKE_CXX_FLAGS  "-Ofast -lrt -DNDEBUG -std=c++11 -DHAVE_CXX0X -march=native -fpic -w -fopenmp -ftree-vectorizer-verbose=0" )
```

多谢Kelvin帮我解决了这个问题，问题的关键在于 `-Ofast`。我们可以先用 `make VERBOSE=1` 来观察两个版本在编译的时候的区别，release 的是这样：
```

/data/opt/brew/bin/c++   -I/data/liujie/all_hnsw/standard-ip-nsw  -Ofast -lrt -DNDEBUG -std=c++11 -DHAVE_CXX0X -march=native -fpic -w -fopenmp -ftree-vectorize -ftree-vectorizer-verbose=0 -O3 -DNDEBUG   -o CMakeFiles/main.dir/main.cpp.o -c /data/liujie/all_hnsw/standard-ip-nsw/main.cpp

```

debug 的是这样：


```

/data/opt/brew/bin/c++   -I/data/liujie/all_hnsw/standard-ip-nsw  -Ofast -lrt -DNDEBUG -std=c++11 -DHAVE_CXX0X -march=native -fpic -w -fopenmp -ftree-vectorize -ftree-vectorizer-verbose=0   -o CMakeFiles/main.dir/main.cpp.o -c /data/liujie/all_hnsw/standard-ip-nsw/main.cpp

```

区别在于release版本的 `-O3` overwrite 之前的 `-Ofast`，一般来说 `-O3` 把能做的优化都做了，但是 `-Ofast` 在 `-O3` 的基础上，enable optimizations that are not valid for all standard-compliant programs，比如 `-ffast-math`，举个例子，比如我们要计算 `a/b/c/d`，我们可以换个顺序`a/(b*c*d)`，但这样输出可能和原来不一致，但由于乘法比除法快，这样会使运行速度更快。Repo 的原作者把CMAKE_CXX_FLAGS写这么一长串其实是不推荐的，更合适的写法是 

```
#### config debug and release separately
set(CMAKE_CXX_FLAGS_DEBUG   "-O0 -g3")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")
```

借这个机会顺便回顾一下常用的g++ compling options。

```
-g 			-turn on debugging (so GDB gives more friendly output)
-Wall 		-turns on most warnings
-O or -O2 	-turn on optimizations
-o <name> 	-name of the output file
-c 			-output an object file (.o)
-I<include path> 	-specify an include directory
-L<library path> 	-specify a lib directory
-l<library> 		-link with library lib<library>.a
```

近期打算多上手写一些project，看一些优秀开源代码（例如[leveldb](https://github.com/google/leveldb)），辅以[google c++ style guide](https://google.github.io/styleguide/cppguide.html)，知道dos and don'ts，然后如果对cmake有问题可以参照[awesome-cmake](https://github.com/onqtam/awesome-cmake)，模仿里面的project的写法，总之希望自己写代码的能力能不断精进。