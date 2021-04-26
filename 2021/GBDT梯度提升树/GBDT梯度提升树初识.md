##### [参考文献:详解GBDT梯度提升树原理，看完再也不怕面试了](https://www.cnblogs.com/techflow/p/13445042.html)

### 1. GBDT基础概念
- GBDT(Gradient Boosting Decision Tree)即梯度提升决策树。
  1. 所以GBDT的基础还是决策树。
    - 在GBDT当中用到的主要是决策树的CART算法
      - 在CART算法当中，我们每次都会选择`一个特征`并且寻找一个`阈值`进行二分。
        - 将样本根据阈值分成**小于等于**阈值的以及**大于**阈值的两个部分。
      - 在CART树当中，同一个特征可以重复使用，其他类似的ID3和C4.5都没有这个性质
  2. Boosting表示一种**集成模型的训练方法**.
    - 最大的特点就是会训练多个模型，通过不断地迭代来降低整体模型的偏差。
      - 比如在Adaboost模型当中，
        - 会设置多个弱分类器，根据这些分类器的表现我们会给他们不同的权值。
        - 通过这种设计尽可能让效果好的分类器拥有高权重，从而保证模型的拟合能力。
      - GBDT的Boosting方法与众不同，它是由**多棵CART决策回归树**构成的加法模型
        - 我们可以简单理解成最后整个模型的预测结果是**所有回归树预测结果的和，而每一棵回归树的训练目标都是之前模型的残差。**
  3. GBDT预测公式
    - <img src="https://latex.codecogs.com/svg.image?f_{M}&space;\left&space;(&space;x&space;\right&space;)=&space;\sum_{i=1}^{M}T\left&space;(&space;x,\theta_i&space;\right)" title="f_{M} \left ( x \right )= \sum_{i=1}^{M}T\left ( x,\theta_i \right)" />  
      - M：CART树的个数
      - T(x,θi)表示第i棵回归树对于样本的预测结果，其中的θ表示每一棵回归树当中的参数。

### 2.梯度和残差
- 和大部分的回归模型不同(计算梯度的目的是为了调整参数)，GBDT**计算梯度是为了下一轮的迭代**
  - 大部分的回归，比如线性回归y = θx+b拟合一个值，目标y=20。当前的θ得到的𝑦̂ 是10，那么我们应该计算梯度来调整参数θ，明显应该将它调大一些从而降低偏差。
  - GBDT与之不同。
    - 同样假设第一棵回归树得到的结果也是10，和真实结果相差了10.假设我们使用均方差MSE作为损失函数,如下
      - <img src="https://latex.codecogs.com/svg.image?L&space;=&space;\frac{1}{2}(y_i&space;-f(x_i))^2" title="L = \frac{1}{2}(y_i -f(x_i))^2" />
    - 用r_mi表示第m棵回归树对于样本i的训练目标，它的公式为
      - r_mi = y_i -f(x_i)
    - 因此我们要预测的结果是20，第一棵树预测了10，相差还剩10，于是我们用第二棵树来逼近。第二棵树预测了5，相差变成了5，继续创建第三棵树、、、一直到我们已经逼近到了非常**接近小于我们设定的阈值**的时候，或者**子树的数量达到了上限**，模型的训练就停止了。
  - 需要注意的是，不能把残差简单地理解成目标值和f(x_i)的差值，他本质是由损失函数计算负梯度得到的。

### 3.训练过程
- M：决策树的数量，f_m(x_i)表示第m轮训练之后的整体，f_m(x_i)即为最终输出的GBDT模型
1. 初始化
  - 首先，创建第一棵回归树即f_1(x)，在回归问题当中，它是直接用回归树拟合目标值的结果。
  - <img src="https://latex.codecogs.com/svg.image?\;&space;f_1(x)&space;=&space;\underset{c}{argmin}\sum_{i=1}^{N}L(y_i,c)" title="\; f_1(x) = \underset{c}{argmin}\sum_{i=1}^{N}L(y_i,c)" /> 
2. 迭代
    - 对于第2到第m棵回归树，我们要计算出每一棵树的训练目标，也就是前面结果的残差:
    - <img src="https://latex.codecogs.com/svg.image?r_{mi}&space;=&space;-\left&space;[&space;\frac{\partial&space;L(y_i,f(x_i))}{\partial&space;f(x_i)}&space;\right&space;]f(x)&space;=&space;f_{m-1}(x)" title="r_{mi} = -\left [ \frac{\partial L(y_i,f(x_i))}{\partial f(x_i)} \right ]f(x) = f_{m-1}(x)" />
    - 对于当前第m棵子树而言，我们需要遍历它的可行的切分点以及阈值，找到最优的预测值c对应的参数，使得尽可能逼近残差。
    - <img src="https://latex.codecogs.com/svg.image?c_{mj}&space;=&space;\underset{c}{argmin}\sum_{x_i\in&space;R_{mj}}L(y_i,f{m-1}(x_i)&plus;c)" title="c_{mj} = \underset{c}{argmin}\sum_{x_i\in R_{mj}}L(y_i,f{m-1}(x_i)+c)" />
      - 这里的R_mj指的是第m棵子树所有的划分方法中叶子节点预测值的集合。也就是**第m棵回归树可能达到的预测值。** 
    
    - 接着更新 <img src="https://latex.codecogs.com/svg.image?f_m(x)&space;=&space;f_{m-1}(x)&space;&plus;&space;\sum_{j=1}^{J}&space;c_{mj}I(x\in&space;R_{mj})" title="f_m(x) = f_{m-1}(x) + \sum_{j=1}^{J} c_{mj}I(x\in R_{mj})" />
      - 这里的I是一个函数，如果样本落在了R_mj节点上，那么I=1，否则I=0
     
3.最后得到回归树
  - <img src="https://latex.codecogs.com/svg.image?F(x)&space;=&space;f_{M}^{(x)}&space;=&space;\sum_{m=1}^{M}f_m^{(x)}&space;=&space;\sum_{m=1}^{M}\sum_{j=1}^{J}c_{mj}I(x\in&space;R_{mj})" title="F(x) = f_{M}^{(x)} = \sum_{m=1}^{M}f_m^{(x)} = \sum_{m=1}^{M}\sum_{j=1}^{J}c_{mj}I(x\in R_{mj})" />
  - 上述公式解释一下就是我们借助c_mj和I把回归树的情况表示了出来。
  - 因为我们训练模型最终希望得到的其实是模型的参数。 
