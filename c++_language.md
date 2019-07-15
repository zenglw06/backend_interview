+ **<font size = 5>c++ 内存分配方式</font>**

    在c++中内存分为五个区，**堆**，**栈**，**自由存储区**，**全局/静态存储区**和**常量存储区**
    
    | c++内存区域| 描述 | 
    | :----------| :--------- | 
    | 堆 | 由malloc分配的内存块，它们的释放编译器不管，需要我们**主动释放**（free）。如果程序结束的时候，分配的内存块都还没有被释放，操作系统会自动回收 | 
    | 栈 | 执行函数时，函数内部的局部变量在栈上创建，函数执行完时，这些存储单元**自动释放**，栈的分配内置于处理器的指令集中，效率高，但是分配的内容容量有限 | 
    | 自由存储区| 是由new分配的内存块，和堆比较相似，但是使用delete释放内存|
    | 全局/静态存储区| 全局变量和静态变量分配到同一内存块中|
    | 常量存储区| 存储常量，不可被修改|

+ **<font size = 5>malloc/free 和 new/delet的区别</font>**

    主要分为以下几个方面的区别
    
    1.  new/delete是c++关键字，malloc/free是库函数，需要头文件支持
    2.  malloc声明的时候需要显式地指出内存所需的大小，new无需指示分配的内存大小，编译器会根据类型自行计算
    3.  malloc内存分配成功时，返回的是void*指针，需要强制类型转换转成需要的类型。new分配内存成功时，返回的指针和对象类型严格匹配
    4.  malloc内存分配失败时，返回NULL指针，new内存分配失败时，会抛出bac_alloc异常
    5.  new/delete允许重载，malloc不允许
    6.  new/delete会调用构造函数和析构函数，而malloc/free不会。使用new时会先分配一块足够大的，原始的，未命名的内存空间以便存储特定类型的对象，然后编译器运行相应的构造函数来构造对象，并为其传入初值，对象构造完成返回该对象指针; delete则是先调用对象的析构函数，编译器再调用delete释放内存空间;使用malloc时不会调用构造函数。
    7.  malloc能够直观地分配内存，再malloc使用过程中发现n内存不足，可以使用realloc函数进行内存重新分配实现内存的扩充。realloc先判断当前指针是否有足够的连续空间，如果有，原地扩大可分配的内存地址，并且返回原来的地址指针，如果空间不够，将原有数据从头到尾拷贝到新分配的内存区域，而后释放原来的内存区域；new没有直观的配套设施来扩充内存


+ **<font size = 5>extern的用法</font>**

    extern可以放在函数或者变量前面，表示变量或者函数的定义在别的文件中，提示编译器在其它模块寻找它的定义。用法主要有两个
    
    1.  和“C”连用，extern “C” void func(int,int),告诉编译器在编译func这个函数时按照C的规则去翻译函数名（C++函数编译时会把函数参数的数据类型也加进去，考虑到函数重载的问题，可能会是func_int_int这种形式，但是C中不会）。
    2.  当extern 用来修饰变量时，比如extern int var, 它表示声明该变量定义在全局中，可以被本模块或者其它模块调用

+ **<font size = 5>static用法</font>**

    1.  static的隐藏作用，当编译多个文件时，未被static修饰的全局变量和函数都有全局可见性，在不同文件中可以通过static对变量隐藏，不同文件中定义同名函数和同名变量，不要担心命名冲突
    2.  static保持内容的持久化
    3.  初始化为0
    4.  类的静态成员函数属于整个类而非类的对象，没有this指针，因而只能访问类的静态成员和静态成员函数

+ **<font size = 5>define和const的区别</font>**

    define是**宏定义**，程序在预处理阶段将用define定义的内容进行替换，因此程序运行时，常量表中并没有用define定义的常量，**系统不为它分配内存**；const定义的常量，在程序运行时在常量表中，系统为它分配内存。define编译时不进行数据类型检查

+ **<font size = 5>引用和指针的区别</font>**

    首先给出引用的定义，引用是对变量起的一个别名，并没有重新定义一个变量；而指针是某个变量的地址，概括如下
    1.  都和地址有关，指针更加自由，引用更加安全
    2.  指针是一个变量，引用是一个变量的别名
    3.  引用只能被初始化一次，而指针变量可以随意改变
    4.  sizeof（引用）是指所指向变量的大小，而指针是**4**字节


+ **<font size = 5>野指针，指针悬挂是什么意思</font>**

    **野指针**：指向不可用内存区域的指针
    
    **指针悬挂**：指向该地址的指针突然指向了别的地方，但是改变这个指向之前并没有对该地址里面的内容撤销，导致该内存区域的数据没法被使用但是却一直存在，造成内存泄漏

+ **<font size = 5>虚函数内部实现</font>**

 



+ **<font size = 5>函数指针</font>**

    函数指针指向某种特定类型，函数的类型由其参数及返回类型共同决定，与函数名无关，可以参考下面这个例子

    1.  作为变量
    
    ```
    //函数定义
    int add(int num1,int num1)
    ```
    这种函数的类型是int (int,int),我们可以声明一个指向该类函数的指针，只要用指针代替函数名就可以

    ```
    int (*func_pointer)(int,int)
    //一定要有括号
    //没有括号 int *func_pointer(int,int)只是表示返回值是int*
    ```
    则func_pointer是指向int (int,int)类型的函数指针，可以将该指针指向add
    ```
    func_pointer = add
    ```
    2.  作为形参
    常见的用法就是我们平时使用sort函数时，定义的一个cmp函数
+ **<font size = 5>重写，重载和重定义分别是什么意思</font>**

    **重载**：函数名相同，函数参数个数、参数类型或者参数顺序三者至少有一种不同
    
    **重定义**：子类重新定义父类中有相同名称的非虚函数（参数列表可以不同），指派生类的函数屏蔽了与其同名的基类函数

    **重写**： 子类重新定义父类中有相同名称的虚函数


+ **<font size = 5>拷贝构造函数为什么要加引用</font>**

    拷贝构造函数要加引用，如果是使用传值的方式的话，会无穷递归的调用拷贝构造函数。
对象如果已经实例化了的话，就只是赋值操作，如果没有实例化的话，调用拷贝构造函数


+ **<font size = 5>内联函数</font>**
    
    在调用内联函数的地方讲内联函数里面的内容直接展开，不用进入函数的步骤，直接执行函数体；和宏比较类似，它会有类型检查，具有真正函数的特性；在类声明中定义的函数，除了虚函数的其它函数都会自动隐式地当成内联函数
    
    **优点如下**：
    1.  内联函数同宏函数一样将在被调用处进行代码展开，省去了参数压栈、栈帧开辟与回收，结果返回等，从而提高程序运行速度。
    2.  内联函数相比宏函数来说，在代码展开时，会做安全检查或自动类型转换（同普通函数），而宏定义则不会。
    3.  在类中声明同时定义的成员函数，自动转化为内联函数，因此内联函数可以访问类的成员变量，宏定义则不能。
    4.  内联函数在运行时可调试，而宏定义不可以
    
    **缺点如下**：
    1.  代码膨胀。内联是以代码膨胀（复制）为代价，消除函数调用带来的开销。如果执行函数体内代码的时间，相比于函数调用的开销较大，那么效率的收获会很少。另一方面，每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。
    2.  inline 函数无法随着函数库升级而升级。inline函数的改变需要重新编译，不像 non-inline 可以直接链接。
    3.  是否内联，程序员不可控。内联函数只是对编译器的建议，是否对函数内联，决定权在于编译器。



+ **<font size = 5>c++智能指针</font>**


    **shared_ptr**: shared_ptr多个指针指向相同的对象。shared_ptr使用引用计数，每一个shared_ptr的拷贝都指向相同的内存。每使用他一次，内部的引用计数加1，每析构一次，内部的引用计数减1，减为0时，自动删除所指向的堆内存。shared_ptr内部的引用计数是线程安全的，但是对象的读取需要加锁。**避免循环引用**

    **shared_ptr的使用**：

    1.  初始化，智能指针是个模板类，可以指定类型，传入指针通过构造函数初始化。也可以使用make_shared函数初始化。不能将指针直接赋值给一个智能指针。

    ```
    std::shared_ptr<int> p = new int(1);//错误的写法
    std::shared_ptr<int> p = std::make_shared<int>(1);//正确的写法
    std::shared_ptr<int> p(new int(1));//正确的写法
    ```
    2.  拷贝和赋值，拷贝时对象引用计数加一，赋值使得被赋值对象引用计数减一；计数为0时，自动释放
    
    3.  一个初始指针不要用来初始化多个shared_ptr,会造成二次释放同一内存

    4.  避免**循环引用**，在parent和child模型中，每个child都有parent，每个parent斗鱼child,构成了一个循环引用，解决方法：
    ~~~
    #include <iostream>
    #include <memory>

    class Child;
    class Parent;

    class Parent {
    private:
        //std::shared_ptr<Child> ChildPtr;
        std::weak_ptr<Child> ChildPtr;
    public:
        void setChild(std::shared_ptr<Child> child) {
            this->ChildPtr = child;
        }

        void doSomething() {
            //new shared_ptr
            if (this->ChildPtr.lock()) {

            }
        }

        ~Parent() {
        }
    };

    class Child {
    private:
        std::shared_ptr<Parent> ParentPtr;
    public:
        void setPartent(std::shared_ptr<Parent> parent) {
            this->ParentPtr = parent;
        }
        void doSomething() {
            if (this->ParentPtr.use_count()) {

        }
    }
        ~Child() {
        }
    };

    int main() {
        std::weak_ptr<Parent> wpp;
        std::weak_ptr<Child> wpc;
        {
            std::shared_ptr<Parent> p(new Parent);
            std::shared_ptr<Child> c(new Child);
            p->setChild(c);
            c->setPartent(p);
            wpp = p;
            wpc = c;
            std::cout << p.use_count() << std::endl; // 2
            std::cout << c.use_count() << std::endl; // 1
        }
        std::cout << wpp.use_count() << std::endl;  // 0
        std::cout << wpc.use_count() << std::endl;  // 0
        return 0;
    }




   ~~~

    
    
    **unique_ptr**: unique_ptr“唯一”拥有其所指对象，同一时刻只能有一个unique_ptr指向给定对象（通过禁止拷贝语义、只有移动语义来实现）。相比与原始指针unique_ptr用于其RAII的特性，使得在出现异常的情况下，动态资源能得到释放。unique_ptr指针本身的生命周期：从unique_ptr指针创建时开始，直到离开作用域。离开作用域时，若其指向对象，则将其所指对象销毁(默认使用delete操作符，用户可指定其他操作)。unique_ptr指针与其所指对象的关系：在智能指针生命周期内，可以改变智能指针所指对象，如创建智能指针时通过构造函数指定、通过reset方法重新指定、通过release方法释放所有权、通过移动语义转移所有权。

    ```
    std::unique_ptr<int> uptr(new int(10));  //绑定动态对象
    //std::unique_ptr<int> uptr2 = uptr;  //不能賦值
    //std::unique_ptr<int> uptr2(uptr);  //不能拷貝
    std::unique_ptr<int> uptr2 = std::move(uptr); //使用移动语义转移所有权
    uptr2.release(); //释放所有权
    ```

    **weak_ptr**: weak_ptr是为了配合shared_ptr而引入的一种智能指针，因为它不具有普通指针的行为，没有重载operator*和->,它的最大作用在于协助shared_ptr工作，像旁观者那样观测资源的使用情况。weak_ptr可以从一个shared_ptr或者另一个weak_ptr对象构造，获得资源的观测权。但weak_ptr没有共享资源，它的构造不会引起指针引用计数的增加。使用weak_ptr的成员函数use_count()可以观测资源的引用计数，另一个成员函数expired()的功能等价于use_count()==0,但更快，表示被观测的资源(也就是shared_ptr的管理的资源)已经不复存在。weak_ptr可以使用一个非常重要的成员函数lock()从被观测的shared_ptr获得一个可用的shared_ptr对象， 从而操作资源。但当expired()==true的时候，lock()函数将返回一个存储空指针的shared_ptr。

    这里再说一下关于自己实现智能指针的要点

    1.  封装一个智能指针并添加一个引用计数，表示该类有多少个对象共享同一个指针，初始化时，引用计数为1
    2.  拷贝构造函数时，引用计数加1，使用赋值操作时，被赋值的指针计数减一，如果为0，则释放内存，可以参考代码如下

    ```
    SharedPtr<T>& operator=(const SharedPtr<T>& ap)
    {
           if (_ptr != ap._ptr)
           {
                 if (--(*_pCount) == 0)
                 {
                      delete _ptr;
                      delete _pCount;
                 }
                 _ptr = ap._ptr;
                 _pCount = ap._pCount;
                 ++(*_pCount);
           }
           return *this;
    }
    ```

    3.  析构函数中，引用计数减一

+ **<font size = 5>STL中vector的resize和reserve分别是什么意思</font>**


    reserve是增加vector的capacity，但是不改变size;resize不仅会改变vector的size还可能会改变capacity，具体可以参考下面这个例子
    ```
    #include<iostream>
    #include<vector>
    using namespace std;
    int main(){
	    vector<int> nums({1,2,3,4});
	    cout<<nums.size()<<endl;//输出4
	    cout<<nums.capacity()<<endl;//输出4
	    nums.push_back(5);
	    cout<<nums.capacity()<<endl;//输出8
	    nums.reserve(100);
	    cout<<nums.size()<<endl;//输出5
	    cout<<nums.capacity()<<endl; //输出100
	    nums.resize(101);
	    cout<<nums.size()<<endl; //输出101
	    cout<<nums.capacity()<<endl; //输出101
	    return 0;
    }
    ```