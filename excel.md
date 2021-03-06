### 1.BINOM.DIST
>一元二项式分布的概率:n次中k次成功的概率<br>
***=BINOM.DIST(number_s, trials, probability_s, cumulative)***
```
Number_s 必需。 试验的成功次数。
Trials 必需。 独立试验次数。
Probability_s 必需。 每次试验成功的概率。
cumulative 必需。 决定函数形式的逻辑值。
  如果为 TRUE，则返回累积分布函数，即最多存在 number_s 次成功的概率；
  如果为 FALSE，则返回概率密度函数，即存在 number_s 次成功的概率。
```

### 2.BINOM.DIST
>返回泊松分布的结果
***=POISSON(x, mean, cumulative)***
```
X 必需。 需要计算其泊松分布的数值。
Mean 必需。期望值。
Cumulative 必需。 决定函数形式的逻辑值。
  如果为 TRUE，则返回累积分布函数；
  如果为 FALSE，则返回概率密度函数。
```

### 3.NORM.S.DIST
>返回标准正态分布的结果
***=NORM.S.DIST(z,cumulative)***
```
Z 必需。 需要计算其正态分布的数值。
Cumulative 必需。 决定函数形式的逻辑值。
  如果为 TRUE，则返回累积分布函数；
  如果为 FALSE，则返回概率密度函数。
```
### 4.NORM.DIST
>返回正态分布的结果
***=NORM.DIST(x,mean,standard_dev,cumulative)***
```
X 必需。 需要计算其正态分布的数值。
Mean 必需。 正态分布的算术平均值。
standard_dev 必需。 正态分布的标准偏差。
Cumulative 必需。 决定函数形式的逻辑值。
  如果为 TRUE，则返回累积分布函数；
  如果为 FALSE，则返回概率密度函数。
```


