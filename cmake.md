## cmake

### find_package使用

当需要使用一个第三方库时，需要知道三件事，去哪儿找头文件，去哪儿找库文件，需要链接的库文件的名字，借助cmake的finder，可以帮我们简化这件事，例如当要使用一个第三方库curl时

``` 
find_package(CURL REQUIRED)
include_directories(${CURL_INCLUDE_DIR})
target_link_library(curltest ${CURL_LIBRARY})
```

find_package()命令会在模块路径中查找Find.cmake，查找路径通常是cmake中变量${CMAKE_MODULE_PATH}中所有的目录，这个变量初始没有值，然后查看它自己的模块目录/share/cmake-x.y/Modules/($CMAKE_ROOT的具体值可以通过cmake中的message()输出)

为了能够支持各种常见的库，cmake自带了常见库的模块，可通过cmake -help-module-list查看

在使用find_package()之后，会自动将路径等变量赋给cmake中的变量值，然后就可以在cmake脚本中使用它们了。变量的列表可以查看cmake模块文件，也可以使用命令cmake --help-module library_name查看

其查找支持module模式和config模式，默认module模式查找，如果此模式没有找到，则搜索XXX_DIR路径下的XXXConfig.cmake文件，执行该文件从而找到库，由这个文件给对应的变量赋值。

find_package()选项，有些库不是一个整体，比如Qt，其中还包含QtOpenGL和QtXml组件，当我们需要使用库的组件的时候，就需要使用COMPONENTS这个选项，如fing_package(Qt COMPONENTS QtOoenGL QtXml REQUIRED)

可以在脚本中加入是否找到对应模块的判断，可以使我们很容易的找到错误

``` 
find_package(CURL REQUIRED)
if(CURL_FOUND)
	include_directiories(${CURL_INCLUDE_DIR})
	target_link_libraries(${CURL_LIBRARY})
endif(CURL_FOUND)
```

### cmake命令简介

* add_definitions命令现在已经被add_compile_definitions(-DENABLED)取代，可以使用它定义一些宏，在代码里面使用#ifdef ENABLED #endif事这一块的代码生效
* option命令，设置自定义的宏,option(ENABLED "this is my message" ON),编译时可以使用cmake -DENABLED=ON设置该宏的值为有效

* target_include_directories和include_directories都表示添加头文件的查找路径，不过前者是对特定的目标添加，而后者是对所有要编译的目标进行添加。同样，target_link_libraries是为特定的目标链接需要使用的库，顺序应为被依赖的库放在依赖它的库后面，否则编译会出错；link_directories是给当前工程链接所需要的库，它与前者的不同在于它必须写明库的全路径
* 控制目标属性：

``` 
set_target_properties(target1 target2
	PROPERTIES
	LINK_FALGS	-static
	LINK_FLAGS_RELEASE	-s
    )
```

此设置会使目标在所有的情况下都使用上述的属性值，其他属性COMPILE_FLAGS，EXECUTABLE_OUTPUT_PATH，LIBRARY_OUTPUT_PATH等

* cmake脚本相当于一个函数，第一个执行的cmake脚本相当于一个主函数，正常设置的变量不能跨越cmake脚本文件，可用过set（VARIABLE "value"）设置变量，通过${VARIABLE}访问变量。还有一种变量为缓存变量，就是cache变量，相当于全局变量，都是放在第一个cmake脚本中设置的，不过子cmake脚本依然可以对其作出修改，设置缓存变量格式为set(MY_CACHE_VALUE	"cache_value" 	CACHE 	INTERNAL	"对变量说明部分")
* 设置环境变量：set（ENV{variable_name} value)，访问环境变量：$ENV{variable_name}
* cmake里面包含大量的内置变量，和自定义的变量相同，如CMAKE_CXX_COMPILER,EXECUABLE_OUTPUT_PATH等
* -fPIC选项作用于编译阶段，告诉编译器产生与位置无关的代码，则产生的代码中，没有绝对地址，全部使用相对地址，故而代码可以被加载都内存的任意位置，这真是共享库的要求，共享库被加载时，其被加载到的内存位置是不固定的。