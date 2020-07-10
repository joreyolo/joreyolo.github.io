### 1.numpy.random.choice 从给定的一次元array数组里随机生成一个样本
<https://docs.scipy.org/doc//numpy-1.15.0/reference/generated/numpy.random.choice.html>
<https://blog.csdn.net/ImwaterP/article/details/96282230>
```
numpy.random.choice(a, size=None, replace=True, p=None)
```
参数|说明
-|-
a : 1-D array-like or int|如果a是int时，结果同numpy.arange→前闭后开<br>，同时除了array，Python中的list和tuple也可以使用
replace：True or False|True表示可以取相同数字，False表示不可以取相同数字
数组p|与数组a相对应，表示取数组a中每个元素的概率，默认为选取每个元素的概率相同。
