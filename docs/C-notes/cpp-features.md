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

## 基于范围的 for 循环
简洁的遍历数组和其它容器 (C++ 11)
```cpp
int arr[]={1,2,3,4,5};

for(int i:arr){
    cout<<i<<' ';
}
```
这种方式默认是值传递，意味着循环内对 i 的修改不会影响原数组中的元素。如果需要修改原数组元素，可以使用引用：
```cpp
for (int &i : arr) {
    i++; // 这会修改 arr 中的元素
}
```
基于范围的 for 循环适用于：
- 普通数组（如 int arr[5]）
- 标准容器（如 vector, list, map 等）
- 任何提供了 begin() 和 end() 方法的类型