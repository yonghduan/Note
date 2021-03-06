### C++Note

#### type-cast

static_cast相当于C语言中的强制类型转换，静态转换，也就是编译期间转换，转换失败会抛出一系列错误，它主要有以下四种用法：

> + 用于继承关系中子类和父类的引用和指针之间的转换，其中上下转换的安全性需要开发人员自己保证，没有动态类型检查
> + 用于基本数据类型之间的转换
> + 用于void类型指针和其他具体类型指针之间的互相转换
> + 把任何类型的表达式转换成void类型

注意：static-cast不能去掉表达式的const和volatile属性，所以不能支持两者之间的转换，并且，static-cast不能支持两个具体类型指针之间的转换，因为转换后是不安全的；也不能将一个具体的值转换为指针类型，因为该值指向的内存可能还没有分配，这是非常危险的。



const-cast用于将const/volatile类型转换为非const/volatile类型

reinterpret_cast这种转换是对二进制位的重新解释，不会借助已有的转换规则对数据进行调整

#### 小知识点

+ cout << char类型的指针实则是打印出了指针所指向的字符串，将char类型指针转换为void * 类型可以打印出char型值的地址
+ auto自动类型推断发生在编译器，所以使用它并不会降低程序执行的效率
+ C++中的临时变量是指由编译器根据需要在栈上产生的没有名字的变量，其主要用途是函数返回值和类型转换产生的中间值，一般来说，临时变量在表达式完成之后它就会被销毁，但是当临时变量关联一个右值引用时，其生命周期就被延长，直到此引用过期，并且原来不可修改的临时变量可以通过引用修改
+ std::move()函数的作用是将一个左值变为右值，当我们给一个函数中传入一个右值引用时，它在实参中有了命名，这个参数变成了一个左值，那么接下来的调用中不会调用它的右值属性，引入std::forward（）实现完美转发，即若原来传入的参数是个左值，转出的值就为左值，若是右值，则转出的就为右值，这样就转发了原有参数的左右值属性，不会造成一些不必要的拷贝

#### lambda表达式

仅当lambda表达式的完全由一条语句构成时，自动类型推断才管用，否则，需要使用新增的返回类型后置法

为何使用lambda？

> 1. 函数内部不能定义函数，因此决定了函数的调用和它的定义是分离的，可以使用函数符来实现函数内部定义函数，因为在函数内部可以定义类，但是使用lambda表达式更加简洁。让函数定义和调用在一起，可以很方便的对其进行修改
> 2. 但并非每次使用lambda表达式的时候都需要定义，可以给它一个名字，如auto mod3 = [] (int x){......};mod3的类型随实现而异，它取决于编译器使用什么类型来跟踪lambda。
> 3. 函数指针的方法通常阻止内联，因为编译器通常不会内联其地址被捕获的函数，函数地址的概念意味着非内联函数，而函数符和lambda通常不会阻止内联
> 4. lambda可访问作用域内的任何变量

#### stringsstream

其对象调用str（）函数可以得到string对象，这个类的作用：

> 数据类型转换，将其他类型转换为string类型
>
> 多个字符串的拼接
>
> stringsstream的清空，有两种方法，第一种为str（“”）方法，第二种为调用clear方法，在进行多次类型转换前，必须使用clear方法进行清空，否则得不到正确的结果

#### inline变量

c++中，类结构中不允许初始化非const静态成员变量，在被包含在多个cpp文件中的头文件中定义类结构之外的变量也是一种错误，因为此头文件被包含了多次，c++17允许在头文件中定义一个内联的变量，这个变量被多个编译单元使用，但他们都指向同一个唯一的对象

#### condition_variable成员函数wait

第一个参数是锁，第二个参数是可调用的对象，如果第二个参数返回true，则直接返回；如果第二个参数为返回false，则将解锁互斥量，并且堵塞线程直到某个某个线程调用motify_one函数为止，如果第二个参数缺省，则和false结果一样

另一个线程调用notify_one（）函数，实则是将堵塞的线程的wait状态唤醒，唤醒后，它将做以下几件事：

> 1. 不断尝试重新获取锁，如果获取不到，就一直卡在这里，如果获取到了，就执行2
> 2. 获取到了锁然后上锁，如果wait有第二个参数，则判断第二个参数。如果为false，wait又解锁，然后堵塞线程
> 3. 如果为true，继续执行流程
> 4. 如果没有参数，wait返回，流程走下去

#### 函数内部static静态变量

函数内部static变量在函数第一次执行的时候创建初始化，之后不会再进行初始化，它和普通函数变量的区别是他在函数结束后不会消亡，并且下次调用这个函数的的时候不会再进行初始化操作

#### nodiscard

[[nodiscard]]是C++17引入的，这个一般用来标记函数返回值或某个类，其含义为不应舍弃。对于函数返回值，当使用弃值表达式而不是cast to void调用时，编译器会发出相关警告。对于被这个标记标记了的类或枚举时，当一个函数返回这个类或者枚举时，使用弃值调用函数时编译器会发出相关警告，但是当函数返回引用或指针时，不会发出相关警告

#### dynamic_cast和dynamic_pointer_cast

前者用于将基类指针或引用转换成派生类指针或引用，它会根据基类指针是否真正指向派生类来做处理，并用派生类指针或引用调用非虚函数。当指针为智能指针时，需要后者进行转换

#### shared_ptr

使用动态内存时，要确保在正确的时间释放内存是很难的，有时候会忘记释放内存，这种情况下会造成内存泄漏；有时候会在尚有指针引用内存的时候就释放了它，这种情况下会产生指针指向非法内存的情况。智能指针则负责自动释放动态内存，shared_ptr允许多个指针指向同一个对象，而unique_ptr则独占一个对象

enable_shared_from_this是一个模板类，定义于头文件memory中。若有一个类T继承std::enable_shared_from_this<T>，该基类为T类提供一个成员函数shared_from_this,假设T类对象t被shared_ptr<T> pt管理，调用T::shared_from_this函数可以产生一个shared_ptr与pt共享对象t，共享对t的所有权。这种使用场合主要是假设A类对象被智能指针管理，当在A类的成员函数里使用到当前对象时，就需要传入当前对象的指针，如果传入this，则一会儿使用普通指针，一会儿使用智能指针会造成语义错误，从而产生各种错误，因此可以通过这种方法产生一个共享的智能指针

#### std::optional<>

* 在函数返回值中，我们可能返回一个具体的值，对象或其他，也可能什么也不返回。可以定义一个对象，包含具体的值，同时包含一个bool类型的值来表示这个值是否存在，而std::optional<>正提供了这样的对象，并且以一种类型安全的方式

#### vector的emplace_back()和push_back()

他们的功能都是给容器末尾添加一个元素，区别在于底层机制的不同。push_back（）是先创建这个元素，然后将它复制或者移动到容器中（如果为复制则销毁先前创建的元素）；而后者是直接在容器尾部创建这个元素，省去了拷贝和移动的麻烦