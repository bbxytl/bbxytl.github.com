[**GitHubBlog**](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
#[C++Primer学习笔记（二）](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/150008_C++Primer学习笔记（二）.md#githubblog-)
---
- **虚函数**:因为直到运行时才能知道到底调用了哪个版本的虚函数，所以所有虚函数都必须有定义，而不管它是否被用到，这是因为连编译器也无法确定到底会使用哪个虚函数。
- **动态绑定**只有当我们通过指针或引用调用虚函数时才会发生。当通过一个具有普通类型（非引用非指针）的表达式调用虚函数时，在编译时就会将调用的版本确定下来。
- **C++的多态性**:具有继承关系的多个类型称为*多态类型*，因为我们能使用这些类型的“多种形式”而无须在意它们的差异。引用或指针的静态类型与动态类型不同这一事实正是C++语言支持多态性的根本所在。
- 当且仅当对通过指针或引用调用虚函数时，才会在运行时解析该调用，也只有在这种情况下对象的动态类型才有可能与静态类型不同。
- 一旦某个函数被声明为虚函数，则在所有派生类中它都是虚函数。所以在派生类中，可以不使用 `virtual` 关键字修饰该函数，但为了更好的可视性，建议加上！
- 派生类中虚函数的形参类型必须保持和基类中完全一样，同时返回类型也必须与基类函数匹配，但当类的虚函数返回类型是类本身的指针或引用时，返回类型可以不同。如：类D 派生自类B，则基类的虚函数可以返回`B*` 而派生类的对应函数可以返回 `D*` ，只不过这样的返回类型要求从 D 到 B 的类型转换是可访问的！
- 为防止在派生类中覆盖虚函数时形参等出错，C++11允许使用关键字 **`override`** 来说明派生类中的虚函数，如果我们使用 `override` 标记了某个函数，但基类中没有这个虚函数，编译器将报错。
- 如果将某个函数指定为 **`final`** ，则任何尝试覆盖该函数的操作都将引发错误。
- 关键字 **`override 、final` ** 说明符出现在形参列表（包括任何 `const` 或引用修饰符）以及尾置返回类型之后。
- **虚函数的默认实参**:虚函数也可有默认实参。如果某次函数调用使用了默认实参，则该实参值由本次调用的静态类型决定。即，如果通过基类的引用或指针调用函数，则使用基类中定义的默认实参，即使实际运行的是派生类中的函数版本也是如此！此时，传入派生类函数的将是基类函数定义的默认实参。注意，这段话说明：调用的仍然是派生类中对应的函数，但函数使用的默认实参却是基类中对应函数的默认实参！所以，如果虚函数使用默认实参，则基类和派生类中定义的默认实参最好一致。
- **什么时候需要回避虚函数的默认机制呢？**通常是当一个派生类的虚函数调用它覆盖的基类的虚函数版本时。此时，需要使用作用域运算符来进行回避。











##**附录**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)



