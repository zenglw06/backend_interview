+   **<font size = 5>修饰器</font>**

    要了解修饰器，需要先了解闭包，指在函数中返回其内部函数，这样当外部函数的生命周期结束后，其内部函数使用的资源都还会被保存下来。
    ```
    def multiply(number):
        def in_func(value):
            return value * number
        return in_func

    double = multiply(2)
    triple = multiply(3)
    print double(5)
    print triple(3)

    ```
    

    在multiply函数生命周期结束后，其内部函数in_func使用的参数变量number不会被销毁，它会存在in_funce.\_\_closure\_\_。比如double函数要使用它的时候，就会从double.\_\_closure\_\_中找到之前保存的number值

    上面的例子是通过外部函数传入数值类型的变量，来获取不同的内部函数实现，我们也可以传入函数对象。

    