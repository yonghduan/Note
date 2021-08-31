### cmake

#### find_package使用

当需要使用一个第三方库时，需要知道三件事，去哪儿找头文件，去哪儿找库文件，需要链接的库文件的名字，借助cmake的finder，可以帮我们简化这件事，例如当要使用一个第三方库curl时

``` 
find_package(CURL REQUIRED)
include_directories(${CURL_INCLUDE_DIR})
target_link_library(curltest ${CURL_LIBRARY})
```

find_package()命令会在模块路径中查找Find.cmake，查找路径通常是cmake中变量${CMAKE_MODULE_PATH}中所有的目录，然后查看它自己的模块目录/share/cmake-x.y/Modules/($CMAKE_ROOT的具体值可以通过cmake中的message()输出)

为了能够支持各种常见的库，cmake自带了常见库的模块，可通过cmake -help-module-list查看

在使用find_package()之后，会自动将路径等变量赋给cmake中的变量值，然后就可以在cmake脚本中使用它们了。变量的列表可以查看cmake模块文件，也可以使用命令cmake --help-module library_name查看

可以在脚本中加入是否找到对应模块的判断，可以使我们很容易的找到错误

``` 
find_package(CURL REQUIRED)
if(CURL_FOUND)
	include_directiories(${CURL_INCLUDE_DIR})
	target_link_libraries(${CURL_LIBRARY})
endif(CURL_FOUND)
```





