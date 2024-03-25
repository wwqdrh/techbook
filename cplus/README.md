# c++

## effective c++

- 把对象移动构造和移动赋值替代复制操作

```c++
class String {
public:
    String(const String& other) { ... }             // 拷贝构造函数
    String& operator=(const String& other) { ... }  // 拷贝赋值运算符
    String(String&& other) noexcept { ... }         // 移动构造函数
    String& operator=(String&& other) noexcept { ... } // 移动赋值运算符
    // ...
};
```

- 优先考虑auto声明变量而非显式类型声明

```c++
std::vector<int> v = {1, 2, 3};
auto iter = v.begin(); // iter的类型被推断为std::vector<int>::iterator
```

- 使用模板别名缩短模板代码

```c++
template <typename T, typename U>
class Data { /* ... */ };

typedef Data<std::string, int> DataStr; // 使用typedef定义别名
using DataInt = Data<int, std::vector<double>>; // 使用using定义别名
```

- 使用省略号捕获多余参数

```c++
template <typename T, typename... Args>
std::unique_ptr<T> make_unique(Args&&... args) {
    return std::unique_ptr<T>(new T(std::forward<Args>(args)...));
}
```

- 将成员函数看作this指针在调用

```c++
class Foo {
public:
    void bar() {
        this->baz(); // 显式使用this指针
        baz();       // 隐式使用this指针
    }
    void baz() { /* ... */ }
};
```

- 用nullptr区分空指针和整数0

```c++
int* p = nullptr;     // 空指针
int x = 0;            // 整数0
if (p == nullptr) {   // 检查空指针
    // ...
}
```

- 为swap专门重载移动语义

```c++
class String {
public:
    String& operator=(String&& rhs) noexcept {
        swap(rhs);
        return *this;
    }

    void swap(String& other) noexcept {
        std::swap(data, other.data);
    }

private:
    char* data;
};
```

- 仔细区分()和{}初始化

圆括号()进行直接初始化,而花括号{}进行列表初始化。两者在某些情况下会有不同的行为,尤其是涉及到窄化转换和聚合类型时。

```c++
int x(3.14);  // 直接初始化,x = 3
int y = 3.14; // 复制初始化,y = 3
int z{3.14};  // 列表初始化,编译错误
```

- 优先考虑使用别名声明替代typedef

```c++
typedef void (*func_ptr)(int, int); // 使用typedef
using func_ptr = void(*)(int, int); // 使用别名声明
```

- 优先考虑限制构造/析构的作用范围

为了提高代码的效率和安全性,应该尽量缩小构造和析构函数的作用范围,避免不必要的资源分配和释放操作。

```c++
void process_data() {
    std::vector<int> data;
    // 处理data...
} // data在这里被自动析构
```

- 多态应该对它开放,继承应该对它关闭

```c++
class Animal {
public:
    virtual void make_sound() = 0; // 抽象基类
};

class Dog : public Animal {
public:
    void make_sound() override { /* 狗叫... */ }
};
```

- 通过override阻止意外的重载和仿函数

```c++
class Base {
public:
    virtual void foo(int);
};

class Derived : public Base {
public:
    void foo(int) override; // OK
    void foo(double) override; // 编译错误,Base没有该函数
};
```

- 把对象指针或引用成员优先于复制数据成员

将资源拥有权转移给对象指针或引用成员,而不是复制整个对象。这样可以避免不必要的拷贝,提高效率和灵活性。

```c++
class Person {
public:
    Person(const std::string& name) : name(name) {}
    // std::string name; // 不推荐,会导致不必要的拷贝
    std::shared_ptr<std::string> name; // 推荐
};
```

- 学习右值引用时遵循的准则

例如仅覆盖operator=而不覆盖拷贝构造函数、使用std::move而不直接创建右值引用等。

```c++
class String {
public:
    String(String&& rhs) noexcept { /* 移动构造 */ }
    String& operator=(String&& rhs) noexcept {
        /* 移动赋值 */
        return *this;
    }
};
```

- 在移动操作中使用移动语义

对于管理资源的类,应该为移动构造函数和移动赋值运算符提供高效的移动语义实现,而不是简单地复制资源。

```c++
class Vector {
public:
    Vector(Vector&& rhs) noexcept : data(rhs.data) {
        rhs.data = nullptr;
    }
    // ...
private:
    int* data = nullptr;
};
```

- 设计一致性支持移动语义

如果提供了移动构造函数,就应该也提供移动赋值运算符。

```c++
class Widget {
public:
    Widget(Widget&&); // 移动构造
    Widget& operator=(Widget&&); // 移动赋值
    // ...
};

```

- 了解特殊的成员函数函数合成做出的工作

编译器会为某些类自动合成默认的特殊成员函数(如构造函数、析构函数等),但如果手动定义了部分特殊成员函数,编译器就不会自动生成其他的了。因此,需要了解这些函数的工作机制,并确保手动定义所需要的特殊成员函数。

- 使用std::unique_ptr适时替代裸指针

std::unique_ptr是一种独占式的智能指针,可以自动管理动态分配的资源,防止内存泄漏。它比使用裸指针更加安全和方便。

```c++
std::unique_ptr<Widget> pw(new Widget);
// 不需要手动delete pw
```

- 使用std::shared_ptr分享数据

```c++
std::shared_ptr<Widget> pw1(new Widget);
std::shared_ptr<Widget> pw2(pw1); // pw1和pw2共享同一对象
```

- 使用std::weak_ptr 扩展std::shared_ptr的生命周期

std::weak_ptr是一种不控制对象生存期的智能指针,它是为了扩展std::shared_ptr的生命周期而设计的。它可以从一个std::shared_ptr或另一个std::weak_ptr对象那里观测资源的存在。

```c++
std::shared_ptr<Widget> pw1(new Widget);
std::weak_ptr<Widget> wpw(pw1); // wpw不影响引用计数
if (std::shared_ptr<Widget> pw2 = wpw.lock()) { /* 访问对象 */ }
```

- 优先考虑Lambda表达式而非std::bind创建函数对象

Lambda表达式是C++11引入的一种创建匿名函数对象的简化语法,比使用std::bind更加直观和简洁。

```c++
auto f = [](int x, int y) { return x + y; };
std::cout << f(1, 2) << std::endl; // 输出 3
```

- 使用Lambda表达式当作std::function对象的替代品

```c++
std::function<int(int, int)> f = [](int x, int y) { return x + y; };
std::cout << f(3, 4) << std::endl; // 输出 7
```

- 优先考虑使用std::make_unique和make_shared替代new

```c++
// 替代 std::unique_ptr<Widget> pw(new Widget);
auto pw = std::make_unique<Widget>();

// 替代 std::shared_ptr<Widget> ps(new Widget);  
auto ps = std::make_shared<Widget>();
```

- 分配数组时使用std::make_unique更好些

std::make_unique是C++14引入的函数,它可以方便地创建std::unique_ptr对象,用于管理动态分配的数组。

```c++
std::unique_ptr<int[]> p = std::make_unique<int[]>(10); // 创建一个长度为10的动态数组
```

- 对指针使用delete优先于delete[]

```c++
Widget* pw = new Widget;
// ...
delete pw; // 而不是 delete[] pw
```

- 使用vector代替非标准数组

非标准数组在C++中存在一些局限性,例如无法获取大小、无法执行构造和析构等。建议使用std::vector代替非标准数组。

```c++
std::vector<int> v = {1, 2, 3, 4, 5}; // 替代 int arr[] = {1, 2, 3, 4, 5};
```

- 优先考虑使用std::make_pair 和 类std::pair的返回值,而不直接新建std::pair象

```c++
auto p = std::make_pair(1, 2.0); // 替代 std::pair<int, double> p(1, 2.0);

std::pair<int, double> makePair() {
    return std::make_pair(1, 2.0); // 直接返回std::pair对象
}
```

- 理解引用折叠

```c++
int x;
int& r1 = x; // 非引用折叠
int&& r2 = 1; // 非引用折叠
int&& r3 = std::move(r1); // r3是一个右值引用,引用折叠
```

- 假设拷贝移动操作不能抛出异常
- 统一使用回避异常保证物理常量表达式

```c++
class Widget {
public:
    Widget() noexcept : 
        data(std::make_unique<int>(0)) // 不会抛出异常
    {}

private:
    std::unique_ptr<int> data;
};
```

- 防御攻击小信号量

在多线程编程中,应该谨防信号量饥饿(semaphore starvation)问题,即某些线程长期无法获得信号量而被饿死。可以通过公平信号量(fair semaphore)等机制来防止这种情况发生。

- 使用使用带命名空间的std版本

优先使用带有std::命名空间的标准库版本,而不是使用旧的non-std版本,以确保可移植性和符合标准。

```c++
// 使用std::cout而不是cout
std::cout << "Hello, World!" << std::endl;
```

- 为内联函数模版化

如果一个函数体足够小,可以考虑将其设置为内联函数,以提高性能。对于模板函数,也应该将其设置为内联,以避免代码膨胀。

```c++
template <typename T>
inline T square(T x) {
    return x * x;
}
```

- 考虑std::string_view而非std::string

std::string_view是一种不拥有字符串数据所有权的字符串视图类,相比std::string更加轻量级和高效。在不需要修改字符串时,应该优先考虑使用std::string_view。

```c++

```

- 使用初始化捕获lambda表达式的成员

```c++
void print(std::string_view sv) {
    std::cout << sv << std::endl;
}

print("Hello"); // 不需要构造临时std::string对象
```

- 在lambda表达式中合理靠用*this

在lambda表达式中,可以使用初始化捕获(initialization capture)语法来捕获成员变量的副本或引用。

```c++
class Widget {
    int x;
public:
    auto getCopier() {
        return [x = x]{ return x; }; // 复制捕获
    }
    auto getRef() {
        return [&x]{ return x; }; // 引用捕获
    }
};
```

- 在lambda表达式中对默认捕获访问成员时求助`*this`

如果使用默认捕获`[=]`或`[&]`访问当前对象的成员,也需要使用*this指针。

```c++
class Widget {
    int x;
public:
    auto getMem() {
        return [*this]{ return x; }; // 使用*this访问x
        // return [=]{ return (*this).x; }; // 使用*this访问x
    }
};
```

- 学习有关std::bind的新陷阱

std::bind是一个用于创建函数对象的函数模板,但在C++11中引入了一些新陷阱和局限性,例如不支持移动语义、可能产生代码膨胀等。在可能的情况下,应该优先使用lambda表达式。

- 向右移动lambda捕获的实参

当lambda表达式捕获了右值引用时,应该将其移动到lambda体内,而不是按值复制。

```c++
auto getMover() {
    auto s = std::make_unique<std::string>("hello");
    return [s = std::move(s)]{ return *s; }; // 移动s到lambda体内
}
```

- 使用nullptr检查提供自定义析构代码

在编写自定义析构函数时,应该检查指针是否为nullptr,以避免对空指针进行错误操作。

```c++
class Widget {
public:
    ~Widget() {
        delete [] data; // 应该先检查data是否为nullptr
    }
private:
    int* data = nullptr;
};
```

- 认清std::move和std::forward 的区别

std::move用于将左值转换为右值引用,而std::forward则用于完美转发,即保留实参的左值/右值属性。理解二者的区别和使用场景很重要。

- 考虑学习std::make_unique的绵绵不绝

std::make_unique在C++14中引入,用于更方便地创建std::unique_ptr对象。在C++17中,它还引入了一些新的重载版本,例如支持数组的版本。需要持续学习该函数的最新用法。

- 合理地学习泛型Lambda表达式

C++14引入了泛型lambda表达式(generic lambda expressions),允许lambda函数的参数类型和返回类型被自动推导。这是一种更加灵活和简洁的编码方式

- 优先使用auto类型推导而非显式类型初始化

使用auto关键字可以让编译器自动推导变量的类型,这种做法比显式类型初始化更加简洁和安全。

```c++
std::vector<int> v = {1, 2, 3};
// 替代 std::vector<int>::iterator iter = v.begin();
auto iter = v.begin();
```

- 完美转发参数

在编写函数模板时,应该使用std::forward来完美转发参数,以保留实参的左值/右值属性,并支持移动语义。

```c++
template <typename T>
void wrapper(T&& arg) {
    foo(std::forward<T>(arg)); // 完美转发arg到foo
}
```

# 参考文档

- [c++那些事](https://light-city.github.io/stories_things/)