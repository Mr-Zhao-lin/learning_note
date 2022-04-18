#  numpy直接赋值/浅拷贝/深拷贝 浅析
## numpy的储存原理：
>核心：储存与解释分离；也即block（内存块）和view（视图）的分离。  
>数据以类似C语言或者Fortan的方式连续储存在内存块block中，numpy.ndarray通过某种方式（可理解为视图）对其进行解释（例如行优先、列优先）。
```python
# 举例
a=np.arrange(4)  
b=a.reshape(2,2)
```
> 这里的a和b指向了同一块内存，但是使用了不同的解释方式（行优先和列优先），可以认为a和b均为这一内存块的视图。
##     直接赋值：
>无拷贝，相当于两者使用了同样的视图和内存块。因此其中任意一个在结构和内存上的变化都会导致另外一个变化。  


```python
>例子:
a=np.arrange(3)
b=a
a.ctypes.data==b.ctypes.data   # 该属性为对象所对应的data内存位置
# True   -----同一数据块
id(a)==id(b)   #求得对象的内存位置 ，可用来表述视图的存放位置
#True   -----同一视图
```
##     浅拷贝：
>浅拷贝，a.view();&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据仍然一致，但是a,b视图独立。即改变其中一个的数据会引起另外一个变化，但是结构上（视图）的变化不会导致另外一个的变化。。  


```python
例子:
a=np.arrange(3)
b=a,view()
a.ctypes.data==b.ctypes.data   # 该属性为对象所对应的data内存位置
# True   -----同一数据块
id(a)==id(b)   #求得对象的内存位置 ，可用来表述视图的存放位置
#False  -----不同视图
```
##     深拷贝：
>深拷贝，a.copy();&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对内存进行了赋值，此时视图和内存均独立，做任意改变都不会影响到另外一个。


```python
#例子:
a=np.arrange(3)
b=a,view()
a.ctypes.data==b.ctypes.data   # 该属性为对象所对应的data内存位置
# False   -----不同数据块
id(a)==id(b)   #求得对象的内存位置 ，可用来表述视图的存放位置
#False  -----不同视图
```

