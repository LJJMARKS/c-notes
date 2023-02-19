### find() 函数

```c++
 vector<int>vec={1,2,4,4,5};
 auto it=find(vec.begin(),vec.end(),5)
 if（it==vec.end()）
 {
    std::cout<<"not found"<<std::endl;
 }
 else {
    std::cout<<"find it "<<std::endl
 }
 /**
  * find() 返回迭代器
  * 找到了返回 第一个给定值的元素的迭代器 
  * 没找到 返回第二个参数 vec.end() 
  * 因此我们可以用返回值和第二个参数进行比较来判断是否找到
  * /
```

```c++
//c++标准库 find 函数的底层实现
template<class InputIterator, class T>
InputIterator find (InputIterator first, InputIterator last, const T& val)
{
    while (first!=last) {
        if (*first==val) return first;
        ++first;
    }
    return last;
}
/**
 * 其中，first 和 last 为输入迭代器，[first, last) 用于指定该函数的查找范围；val 为要查找的目标元素。
```