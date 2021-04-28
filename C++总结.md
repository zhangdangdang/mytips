# C++总结

## C++派生与继承

### 1 继承名字遮蔽：

子函数有与基类同名函数会造成名字遮蔽，不会造成函数重载http://c.biancheng.net/view/2271.html

### 2 子类访问基类成员函数：

子类与基类同名函数，在子类对象调用中默认调用子类函数，子类调基类函数 

```c++
d.Base::func();
```

通过类名和域解析符。链接同上

### 3 子类访问基类的私有变量：

通过在子类中的构造函数中调用基类构造来访问基类的私有成员变量。http://c.biancheng.net/view/2275.html

 借助指针突破访问权限的限制，访问private、protected属性的成员变量，借用内存布局，可以访问https://blog.csdn.net/songbai220/article/details/106271210/

### 4 通过派生类创建对象时必须要调用基类的构造函数,直接基类

这是语法规定。换句话说，定义派生类构造函数时最好指明基类构造函数；如果不指明，就调用基类的默认构造函数（不带参数的构造函数）；如果没有默认构造函数，那么编译失败

### 5 菱形继承防止奇异可以指明来自哪个类：

```c++
void seta(int a){ B::m_a = a; }
```

### 6 虚继承 构造函数首先虚基类的再构造直接基：

虚继承时构造函数的执行顺序与普通继承时不同：在最终派生类的构造函数调用列表中，不管各个构造函数出现的顺序如何，编译器总是先调用虚基类的构造函数，再按照出现的顺序调用其他的构造函数；而对于普通继承，就是按照构造函数出现的顺序依次调用的。

### 7 C++将派生类赋值给基类（向上转型）

类其实也是一种数据类型，也可以发生数据类型转换，不过这种转换只有在基类和派生类之间才有意义，并且只能将派生类赋值给基类，包括将派生类对象赋值给基类对象、将派生类指针赋值给基类指针、将派生类引用赋值给基类引用，这在 C++ 中称为向上转型（Upcasting）。相应地，将基类赋值给派生类称为向下转型（Downcasting）。编译器通过指针来访问成员变量，指针指向哪个对象就使用哪个对象的数据；编译器通过指针的类型来访问成员函数，指针属于哪个类的类型就使用哪个类的函数。向上转型后通过基类的对象、指针、引用只能访问从基类继承过去的成员（包括成员变量和成员函数），不能访问派生类新增的成员。 http://c.biancheng.net/view/2284.html

## 多态和虚函数

### 1 只有派生类的虚函数覆盖基类的虚函数（函数原型相同）才能构成多态

派生类没有基类同名函数则调用基类函数

```c++
#include <iostream>
using namespace std;
//基类Base
class Base{
public:
    virtual void func();
    virtual void func(int);
};
void Base::func(){
    cout<<"void Base::func()"<<endl;
}
void Base::func(int n){
    cout<<"void Base::func(int)"<<endl;
}
//派生类Derived
class Derived: public Base{
public:
    void func();
    void func(char *);
};
void Derived::func(){
    cout<<"void Derived::func()"<<endl;
}
void Derived::func(char *str){
    cout<<"void Derived::func(char *)"<<endl;
}
int main(){
    Base *p = new Derived();
    p -> func();  //输出void Derived::func()
    p -> func(10);  //输出void Base::func(int)
    p -> func("http://c.biancheng.net");  //compile error
    return 0;
}
```

