# c++

## effective c++

- 把对象移动构造和移动赋值替代复制操作
- 优先考虑auto声明变量而非显式类型声明
- 使用模板别名缩短模板代码
- 使用省略号捕获多余参数
- 将成员函数看作this指针在调用
- 用nullptr区分空指针和整数0
- 为swap专门重载移动语义
- 仔细区分()和{}初始化
- 优先考虑使用别名声明替代typedef
- 优先考虑限制构造/析构的作用范围
- 多态应该对它开放,继承应该对它关闭
- 通过override阻止意外的重载和仿函数
- 把对象指针或引用成员优先于复制数据成员
- 学习右值引用时遵循的准则
- 在移动操作中使用移动语义
- 设计一致性支持移动语义
- 了解特殊的成员函数函数合成做出的工作
- 使用std::unique_ptr适时替代裸指针
- 使用std::shared_ptr分享数据
- 使用std::weak_ptr 扩展std::shared_ptr的生命周期
- 优先考虑Lambda表达式而非std::bind创建函数对象
- 使用Lambda表达式当作std::function对象的替代品
- 优先考虑使用std::make_unique和make_shared替代new
- 分配数组时使用std::make_unique更好些
- 对指针使用delete优先于delete[]
- 使用vector代替非标准数组
- 优先考虑使用std::make_pair 和 类std::pair的返回值,而不直接新建std::pair对- 象
- 理解引用折叠
- 假设拷贝移动操作不能抛出异常
- 统一使用回避异常保证物理常量表达式
- 防御攻击小信号量
- 使用使用带命名空间的std版本
- 为内联函数模版化
- 考虑std::string_view而非std::string
- 使用初始化捕获lambda表达式的成员
- 在lambda表达式中合理靠用*this
- 在lambda表达式中对默认捕获访问成员时求助- *this
- 学习有关std::bind的新陷阱
- 向右移动lambda捕获的实参
- 使用nullptr检查提供自定义析构代码
- 认清std::move和std::forward 的区别
- 考虑学习std::make_unique的绵绵不绝
- 合理地学习泛型Lambda表达式
- 优先使用auto类型推导而非显式类型初始化
- 完美转发参数
- 查阅refresh引物令其获取结果

# 参考文档

- [c++那些事](https://light-city.github.io/stories_things/)