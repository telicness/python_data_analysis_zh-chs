
[*第三章:NumPy库*](./README.md)


# 3.11. 读写文件中的数组数据

NumPy的一个非常重要的方面还没有讨论，那就是读取文件中包含的数据的过程。这个过程非常有用，特别是当您必须处理数组中收集的大量数据时。这是一种非常常见的数据分析操作，因为要分析的数据集的大小几乎总是很大，因此不建议甚至不可能手动管理它。

NumPy提供了一组函数，允许数据分析师将计算结果保存在文本或二进制文件中。类似地，NumPy允许您读取并将文件中的写入数据转换为数组。


## 在二进制文件中加载和保存数据

NumPy提供了一对名为save()和load()的函数，使您能够保存并稍后检索以二进制格式存储的数据。

有了要保存的数组(例如包含数据分析处理结果的数组)后，只需调用save()函数并将文件和数组的名称指定为参数。文件将自动被赋予.npy扩展名。

```python
>>> data=([[ 0.86466285,  0.76943895,  0.22678279],
       [ 0.12452825,  0.54751384,  0.06499123],
       [ 0.06216566,  0.85045125,  0.92093862],
       [ 0.58401239,  0.93455057,  0.28972379]])
>>> np.save('saved_data',data)
```

当您需要恢复存储在.npy文件中的数据时，您可以通过指定文件名作为参数来使用load()函数，这次添加扩展名.npy。

```python
>>> loaded_data = np.load('saved_data.npy')
>>> loaded_data
array([[ 0.86466285,  0.76943895,  0.22678279],
       [ 0.12452825,  0.54751384,  0.06499123],
       [ 0.06216566,  0.85045125,  0.92093862],
       [ 0.58401239,  0.93455057,  0.28972379]])
```

## 读取文件中的表格数据

很多时候，您想要读取或保存的数据都是文本格式(例如TXT或CSV)。您可以以这种格式保存数据，而不是二进制文件，因为如果您使用NumPy或任何其他应用程序，那么可以在外部独立访问这些文件。以CSV(逗号分隔值)格式的一组数据为例，其中数据以表格格式收集，值之间用逗号分隔(参见清单3-1)。


```
id,value1,value2,value3
1,123,1.4,23
2,110,0.5,18
3,164,2.1,19

```
>> Listing 3-1. ch3_data.csv
>> 清单3-1。CH3_data.csv

为了能够读取文本文件中的数据并将值插入数组中，NumPy提供了一个名为genfromtxt()的函数。通常，该函数使用三个参数——包含数据的文件名称、分隔值的字符(在本例中是逗号)，以及数据是否包含列标头。

```python
>>> data = np.genfromtxt('ch3_data.csv', delimiter=',', names=True)
>>> data
array([(1.0, 123.0, 1.4, 23.0), (2.0, 110.0, 0.5, 18.0),
       (3.0, 164.0, 2.1, 19.0)],
      dtype=[('id', '<f8'), ('value1', '<f8'), ('value2', '<f8'), ('value3', '<f8')])
```

从结果中可以看到，您得到了一个结构化数组，其中列标题已成为字段名。

这个函数隐式地执行两个循环:第一个循环一次读取一行，第二个循环分离并转换其中包含的值，插入专门创建的连续元素。这个特性的一个积极方面是，如果缺少一些数据，函数可以处理它们。

以前面的文件(参见清单3-2)为例，删除了一些项。保存为data2.csv。

```python
id,value1,value2,value3
1,123,1.4,23
2,110,,18
3,,2.1,19
```
>> Listing 3-2. ch3_data2.csv
>> 清单3-2.CH3_data2.csv

启动这些命令，您可以看到genfromtxt()函数如何用nan值替换文件中的空格。

```python
>>> data2 = np.genfromtxt('ch3_data2.csv', delimiter=',', names=True)
>>> data2
array([(1.0, 123.0, 1.4, 23.0), (2.0, 110.0, nan, 18.0),
       (3.0, nan, 2.1, 19.0)],
      dtype=[('id', '<f8'), ('value1', '<f8'), ('value2', '<f8'), ('value3', '<f8')])
```

在数组的底部，可以找到文件中包含的列标题。这些标头可以被认为是作为索引按列提取值的标签：

```python
>>> data2['id']
array([ 1.,  2.,  3.])
```
相反，通过以典型的方式使用数字索引，您将提取与行相对应的数据。

```python
>>> data2[0]
(1.0, 123.0, 1.4, 23.0)
```
