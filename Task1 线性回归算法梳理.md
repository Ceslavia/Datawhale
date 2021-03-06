﻿# Task1 线性回归算法梳理
##### 1.机器学习的一些概念
 **机器学习**：简单的讲就是，通过输入数据，称为数据集，经由学习算法训练模型，输出相应的模型，并用来解决实际问题。

根据训练方法的不同，机器学习可分为：**监督学习**，**无监督学习**，半监督学习，强化学习。
* **监督学习**：监督学习是从标记的训练数据来判断一个功能的机器学习任务，可分为“回归”和“分类”问题。
	*
			定量输出称为回归，比如根据房屋的地理位置，房屋面积大小，以及房屋周边的配套设施等因素，来预测下给定房屋的价格，这就是典型的回归问题。
	*
			定性输出称为分类，根据图片识别出图片中的物体是猫还是狗，这就是典型的分类问题。
			
* **无监督学习**：无监督学习和监督学习最大的不同在于，事先没有任何训练样本，而需要直接对数据进行建模。无监督学习只能默默的读取数据，自己寻找数据的模型和规律，比如聚类（把相似数据归为一组）和异常检测（寻找出一组数据的不同一个），在无监督学习中给定的数据没有任何标签或者说只有同一种标签。

	* 		比如小时候我们还不认识钱币的时候，看到一堆纸币和硬币，会很自然的把纸币和硬币分开，这就是聚类的最简单原理。

 **泛化能力**：是指一个模型应用到新样本的能力。这里的新样本是指没有出现在训练集中的数据。

**过拟合**：就是训练时的结果很好，但是在预测时结果不好的情况。
**欠拟合**:模型没有很好地捕捉到数据特征，不能够很好地拟合数据的情况，就是欠拟合。
**Variance(方差)**：Variance反映的是模型每一次输出结果与模型输出期望之间的误差，即模型的稳定性。反应预测的波动情况。
**Bias(偏差)**：Bias反映的是模型在样本上的输出与真实值之间的误差，即模型本身的精准度，即算法本身的拟合能力。

* **过拟合会出现高方差问题**
	* 产生过拟合的原因：
	（1）模型的复杂度太高。比如：网络太深
	（2）过多的变量（特征）
	（3）训练数据非常少
	* 如何解决过拟合？
	（1）增大数据集合—–使用更多的数据，噪声点比重减少
	（2）减少数据特征—–减小数据维度，高维空间密度小
	（3）正则化方法—–即在对模型的目标函数（objective function）或代价函数（cost function）加上正则项
	（4）交叉验证方法
* **欠拟合会出现高偏差问题**
	* 产生欠拟合原因：
		模型不够复杂而无法捕捉数据基本关系，导致模型错误的表示数据。
		
			比如：（1）如果对像是按照颜色和形状进行分类的，但是模型只能按照颜色来区分对象和将对象分类，因而一直会错误的分类对象。
					（2）我们的模型可能是多项式的形式，但是训练出来的模型却只能表示线性关系。	
	* 如何解决欠拟合？
	（1）寻找更好的特征—–具有代表性的
    （2）用更多的特征—–增大输入向量的维度

**交叉验证(Cross-Validation)**:有时亦称循环估计， 是一种统计学上将数据样本切割成较小子集的实用方法。于是可以先在一个子集上做分析， 而其它子集则用来做后续对此分析的确认及验证。 一开始的子集被称为训练集。而其它的子集则被称为验证集或测试集。[WIKI](https://zh.wikipedia.org/zh-sg/%E4%BA%A4%E5%8F%89%E9%A9%97%E8%AD%89)

交叉验证对于人工智能，机器学习，模式识别，分类器等研究都具有很强的指导与验证意义。
基本思想是把在某种意义下将原始数据(dataset)进行分组,一部分做为训练集(train set),另一部分做为验证集(validation set or test set),首先用训练集对分类器进行训练,在利用验证集来测试训练得到的模型(model),以此来做为评价分类器的性能指标.

[https://blog.csdn.net/MrLevo520/article/details/53128297](https://blog.csdn.net/MrLevo520/article/details/53128297)
*******************************************************************
##### 2.线性回归的原理
线性回归使用最佳的拟合直线在因变量和一个或多个自变量之间建立一种关系。
$$h_\theta(x_1,x_2,...x_n)=\theta_0+\theta_1x_1+...+\theta_nx_n$$
增加一个特征$x_0=1$,这样$$h_\theta(x_1,x_2,...x_n)=\sum_{i=0}^n\theta_ix_i$$
##### 3.线性回归损失函数、代价函数、目标函数
**损失函数（Loss Function ）**:是定义在单个样本上的，算的是一个样本$i$的损失/误差。$$J(\theta)=\sum_{i=0}^n(h_\theta(x_i)-y_i)^2$$

**代价函数（Cost Function ）**:是定义在整个训练集上的，是所有m个样本误差综合的平均，也就是损失函数的平均,再除以2便于计算。$$J(\theta)=\frac{1}{2m}\sum_{j=1}^m(h_\theta(x^{(j)}_i)-y^{(j)}_i)^2$$

**目标函数（Object Function）**：定义为最终需要优化的函数。等于经验风险+结构风险（也就是Cost Function + 正则化).

许多机器学习算法，往往建立目标函数（损失函数+正则项），通过优化方法进行优化，根据训练样本训练出满足要求的模型。常见的优化方法有梯度下降法、牛顿法拟牛顿法、共轭梯度法、拉格朗日乘数法等等。$$J(\theta)=\frac{1}{2m}\sum_{j=1}^m(h_\theta(x^{(j)}_i)-y^{(j)}_i)^2+正则化项$$

##### 4.优化方法
一、**梯度下降法**
  梯度下降法（Gradient Descent）是一种常用的一阶优化方法，是求解无约束优化问题的经典方法。通过反复迭代，沿目标函数梯度的反方向逼近目标函数的最优解（当目标函数为凸函数时，才能得到全局最优）。目标函数极值点附近，收敛速度变慢。
  梯度下降方法包括批量梯度下降法（Batch Gradient Descent，BGD）以及随机梯度下降法（Stochastic Gradient Descent，SGD）。
  
二、**牛顿法、拟牛顿法**
  牛顿法（Newton Method）、拟牛顿法（Quasi Newton Method）是常用的二阶优化方法，是求解无约束优化问题的经典方法。由于用到了二阶导数信息，因此相比梯度下降法收敛速度更快。牛顿法也是通过反复迭代，求解目标函数的最优解。但是，牛顿法每次迭代需要求解目标函数的海塞矩阵的逆矩阵，因此计算相对复杂。拟牛顿法通过正定矩阵近似海塞矩阵的逆矩阵或者海塞矩阵，简化了计算过程。

三、**共轭梯度法**
  共轭梯度法（Conjugate Gradient）是介于梯度下降法与牛顿法之间的一个方法，它仅需利用一阶导数信息，但克服了梯度下降法收敛速度慢的缺点，又避免了牛顿法需要存储和计算海塞矩阵并求逆的缺点。共轭梯度法优点是所需存储量小，具有步收敛性，稳定性高，而且不需要任何外来参数。

四、**拉格朗日乘数法**——解决含有约束条件的优化问题
  拉格朗日乘数法（Lagrange Multipliers）是一种寻找多元函数在一组约束条件下的极值的方法。通过引入拉格朗日乘子，将含有d个变量与k个约束条件的最优化问题转化为具有d+k个变量的无约束优化问题的求解。
##### 5.线性回归的评估指标
* 平均绝对误差（MAE	）$$MAE=\frac{1}{n}\sum_{i=1}^n|f_i-y_i|$$
* 均方误差（MSE）$$MSE=\frac{1}{n}\sum_{i=1}^n(f_i-y_i)^2$$
* 方均根差（RMSE）$$RMSE=\sqrt{MSE}$$
* 平均绝对百分比误差（MAPE）$$MAPE=\frac{100}{n}\sum{i=1}^n|\frac{y_i-f_i}{y_i}|$$
* R平方$$r^2=1-\frac{SS_{res}}{SS_{tot}}=1-\frac{\sum(y_i-f_i)^2}{\sum(y_i-\overline{y})^2}$$

##### 6.sklearn 线性回归LinearRegression()参数参数详解
class **sklearn.linear_model.LinearRegression**(fit_intercept=True, normalize=False, copy_X=True, n_jobs=1)

*普通最小二乘线性回归*
**参数**：

* **fit_intercept**:   布尔型，默认为true

说明：是否对训练数据进行中心化。如果该变量为false，则表明输入的数据已经进行了中心化，在下面的过程里不进行中心化处理；否则，对输入的训练数据进行中心化处理

* **normalize**：布尔型，默认为false

说明：是否对数据进行标准化处理

* **copy_X**:布尔型，默认为true

说明：是否对X复制，如果选择false，则直接对原数据进行覆盖。（即经过中心化，标准化后，是否把新数据覆盖到原数据上）

* **n_jobs**:  整型， 默认为1

说明：计算时设置的任务个数(number of jobs)。如果选择-1则代表使用所有的CPU。这一参数的对于目标个数>1（n_targets>1）且足够大规模的问题有加速作用。

**返回值**
* **coef_**: 数组型变量， 形状为(n_features,)或(n_targets, n_features)

说明：对于线性回归问题计算得到的feature的系数。如果输入的是多目标问题，则返回一个二维数组(n_targets, n_features)；如果是单目标问题，返回一个一维数组                               (n_features,)。

* **intercept_**: 数组型变量

说明：线性模型中的独立项。


*注：该算法仅仅是scipy.linalg.lstsq经过封装后的估计器。*

**几个常用函数**
* **decision_function(X)**  对训练数据X进行预测
* **fit(X, y[, n_jobs])**         对训练集X, y进行训练。是对scipy.linalg.lstsq的封装
* **get_params([deep])** 得到该估计器(estimator)的参数。
* **predict(X)** 使用训练得到的估计器对输入为X的集合进行预测（X可以是测试集，也可以是需要预测的数据）。
* **score(X, y[,]sample_weight)** 返回对于以X为samples，以y为target的预测效果评分。
* **set_params(** ****params)** 设置估计器的参数





