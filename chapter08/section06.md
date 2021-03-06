
# 第8章：机器学习与科学工具包scikit-learn


# 8.6. 糖尿病数据集

在scikit-learn库中可用的各种数据集中，有一个是糖尿病数据集。这个数据集在2004年首次被使用(Efron, Hastie, Johnston和Tibshirani合著的统计学年鉴)。从那以后，它成为广泛用于研究各种预测模型及其有效性的一个例子。

要上载此数据集中包含的数据，在您必须导入scikit-learn库的数据集模块之前，您需要调用load_diabetes()函数将数据集加载到一个名为diabetes的变量中。

```python
In [ ]: from sklearn import datasets
   ...: diabetes = datasets.load_diabetes()
```

该数据集包含了442例患者的生理数据，作为相应的目标，是一年后疾病进展的指标。生理数据占据前10列，值分别表示如下:

* 年龄
* 性别
* 身体质量指数
* 血压
* S1、S2、S3、S4、S5和S6(六项血清测量)

这些度量值可以通过调用data属性获得。但是，如果要检查数据集中的值，您会发现值与您预期的非常不同。例如，我们观察第一个病人的10个值。

```python
diabetes.data[0]
Out[ ]:
array([ 0.03807591,  0.05068012,  0.06169621,  0.02187235, -0.0442235 ,
       -0.03482076, -0.04340085, -0.00259226,  0.01990842, -0.01764613])
```

这些值实际上是处理的结果。10个值中的每一个都以均值为中心，然后按标准差乘以样本数量进行缩放。检查会发现每一列的平方和等于1。试着用年龄测量来做这个计算;您将获得非常接近1的值。

```python
np.sum(diabetes.data[:,0]**2)
Out[143]: 1.0000000000000746
```

虽然这些值是标准化的，难以阅读，但它们继续表达10个生理特征，因此没有失去它们的价值或统计信息。

至于疾病进展的指标，即必须与你的预测结果相对应的数值，可以通过目标属性获得。

```python
diabetes.target
Out[146]:
array([ 151.,   75.,  141.,  206.,  135.,   97.,  138.,   63.,  110.,
        310.,  101.,   69.,  179.,  185.,  118.,  171.,  166.,  144.,
         97.,  168.,   68.,   49.,   68.,  245.,  184.,  202.,  137
        . . .
```

您将获得一系列介于25和346之间的442个整数值。

