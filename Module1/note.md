# 学习笔记
## 头文件宏定义
```c++
#ifndef _MATRIX_HPP
#define _MATRIX_HPP
……
#endif
```
在大的软件工程里面，可能存在多个文件同时包含一个头文件。
这种写法可以在生成可执行文件时避免头文件的重定义。

## 模板
* 函数模板
    ``` c++
    template <typename type> ret-type func-name(parameter list)
    {
    // 函数的主体
    }
    ```
  例子
    ```c++
    template <typename T>
    inline T const& Max (T const& a, T const& b)
    {
    return a < b ? b:a;
    }
    //使用
    int i = 39;
    int j = 20;
    cout <<Max(i, j) << endl;
    ```
* 类模板
    ```c++
    template <class type> class class-name {
    .
    .
    .
    }
    ```
  例子
    ```c++
    template <class T>
    class Stack {
    private:
    vector<T> elems;     // 元素
    
    public:
    void push(T const&);  // 入栈
    void pop();               // 出栈
    T top() const;            // 返回栈顶元素
    bool empty() const{       // 如果为空则返回真。
    return elems.empty();
    }
    };
    
    template <class T>
    void Stack<T>::push (T const& elem)
    {
    // 追加传入元素的副本
    elems.push_back(elem);    
    }
    
    template <class T>
    void Stack<T>::pop ()
    {
    if (elems.empty()) {
    throw out_of_range("Stack<>::pop(): empty stack");
    }
    // 删除最后一个元素
    elems.pop_back();         
    }
    
    template <class T>
    T Stack<T>::top () const
    {
    if (elems.empty()) {
    throw out_of_range("Stack<>::top(): empty stack");
    }
    // 返回最后一个元素的副本
    return elems.back();      
    }
    //使用
    Stack<int> intStack
    intStack.push(7); 
    cout << intStack.top() <<endl; 
    ```

## 虚拟函数 virtual
* 虚函数  
  c++重写中涉及到
  ```c++
  base *p = new inheriter;
  ```
  假如使用类中的virtual函数，则是派生类inheriter中的函数。
  假如使用正常函数，则会使用基类vase的函数。
* 虚基类
  共享基类不出问题
  ```c++
  //间接基类A
  class A{
  protected:
  int m_a;
  };
  //直接基类B
  class B: virtual public A{  //虚继承
  protected:
  int m_b;
  };
  //直接基类C
  class C: virtual public A{  //虚继承
  protected:
  int m_c;
  };
  //派生类D
  class D: public B, public C{
  public:
  void seta(int a){ m_a = a; }  //正确
  void setb(int b){ m_b = b; }  //正确
  void setc(int c){ m_c = c; }  //正确
  void setd(int d){ m_d = d; }  //正确
  private:
  int m_d;
  };
  ```
  假如不适用虚基类，D中的seta会因为有B和C两个seta而矛盾报错。
* 纯虚函数
  有纯虚函数的基类只能被继承，而不能实例化，需要在派生类中实现。
  ```c++
  class Base {
  public:
  virtual void pureVirtualFunction() = 0;
  };
  
  class Derived : public Base {
  public:
  void pureVirtualFunction() override {
  // 实现纯虚函数
  // ...
  }
  };
  ```
  
## 友元函数
```c++
class Box{
    double a;
public:
    friend void printWidth(Box box);
};

void printWidth(Box box){
    cout << box.width <<endl;
}
```
通过这种方法可以访问Box类中的所有变量。

## 运算符重载
一些常见的运算符和它们在C++中的重载用途：

* Arithmetic Operators (算术运算符):
+, -, *, /, % 等用于重载加、减、乘、除和取模运算符。
* Comparison Operators (比较运算符):
==, !=, <, >, <=, >= 等用于重载相等、不相等、小于、大于、小于等于和大于等于运算符。
* Assignment Operators (赋值运算符):
=, +=, -=, *=, /=, %= 等用于重载赋值和复合赋值运算符。
* Increment and Decrement Operators (自增和自减运算符):
++, -- 用于重载前缀和后缀自增和自减运算符。
* Indexing Operator (索引运算符):
[] 用于重载类对象的索引运算符，使其可以像数组一样访问对象的元素。
* Function Call Operator (函数调用运算符):
() 用于重载函数调用运算符，使对象可以像函数一样被调用。
* Member Access Operators (成员访问运算符):
-> 用于重载成员访问运算符，使对象可以像指针一样访问成员。
* Stream Insertion and Extraction Operators (流插入和提取运算符):
<<, >> 用于重载流插入和提取运算符，使自定义类型可以通过流进行输入和输出。
```c++
// 正常
Box operator+(const Box&);
//类的非成员函数
Box operator+(const Box&, const Box&);
```

## std::all_of
```c++
bool all_students_passed(const std::vector <Student> &students,
                         double pass_threshold) {
    return std::all_of(students.begin(),
                       students.end(),
                       [pass_threshold](Student s) {
                           double hw = s.homework * HOMEWORK_WEIGHT;
                           double mt = s.midterm * MIDTERM_WEIGHT;
                           double fe = s.final_exam * FINAL_EXAM_WEIGHT;
                           return hw + mt + fe >= pass_threshold;
                       }
    );
}
```

[pass_threshold] 是 lambda 函数中的一个捕获列表（capture list），用于指定 lambda 函数所捕获的外部变量。Lambda 函数可以通过捕获列表捕获外部变量，并在函数体内使用这些变量。

在这个特定的 lambda 函数中，[pass_threshold] 指明了 lambda 函数捕获了名为 pass_threshold 的外部变量。这意味着 lambda 函数可以在其函数体内访问并使用 pass_threshold 这个外部变量。

Lambda 函数的捕获列表有两种方式：
* 按值捕获: [var1, var2, ...] - 按值捕获指定变量。Lambda 函数拷贝这些变量的值，可以在函数体内读取但不能修改这些值。
* 按引用捕获: [&var1, &var2, ...] - 按引用捕获指定变量。Lambda 函数通过引用访问这些变量，可以在函数体内读取和修改这些变量。
 

