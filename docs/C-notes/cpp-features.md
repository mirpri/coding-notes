# C++ Features
*This section focuses on features unique to c++*

## 传递引用类型的参数
函数原型中，可定义**引用类型**参数。
```cpp
void work(int &x) {
    x = x + 10; // 修改 x 的值
}

int main() {
    int a = 5;
    work(a); // 传递 a 的引用给 work
    cout<<a; // a为15
    return 0;
}
```
引用类型的参数在函数调用时不会创建参数的副本，而是直接引用调用时传递的实参。这样，可以避免拷贝参数带来的开销，且能在函数中会直接修改原变量的值，也无需使用指针间接访问。