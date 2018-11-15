
[*第五章：pandas：读写数据*](./README.md)


# 5.3. 读取CSV或文本文件中的数据

根据经验，从事数据分析的人最常见的操作是读取CSV文件或至少是文本文件中的数据。

但是在开始处理文件之前，需要导入以下库。

```python
>>> import numpy as np
>>> import pandas as pd
```

为了了解panda如何处理这类数据，我们将从在工作目录中创建一个小CSV文件开始，如清单5-1所示，并将其保存为ch05_01.csv。

```text
white,red,blue,green,animal
1,5,2,3,cat
2,7,8,5,dog
3,3,6,7,horse
2,2,8,3,duck
4,4,2,1,mouse
```
>> 清单5-1。ch05_01.csv

由于该文件以逗号分隔，因此可以使用read_csv()函数读取其内容并将其转换为dataframe对象。

```python
>>> csvframe = pd.read_csv('ch05_01.csv')
>>> csvframe
   white  red  blue  green animal
0      1    5     2      3    cat
1      2    7     8      5    dog
2      3    3     6      7  horse
3      2    2     8      3   duck
4      4    4     2      1  mouse
```

如您所见，读取CSV文件中的数据非常简单。CSV文件是用逗号分隔同一列上的值的表格数据。因为CSV文件被认为是文本文件，所以您还可以使用read_table()函数，但要指定分隔符。

```python
>>> pd.read_table('ch05_01.csv',sep=',')
   white  red  blue  green animal
0      1    5     2      3    cat
1      2    7     8      5    dog
2      3    3     6      7  horse
3      2    2     8      3   duck
4      4    4     2      1  mouse
```

在本例中，您可以在CSV文件中看到，标识所有列的标头位于第一行。但这不是一般情况;表格数据通常直接从第一行开始(参见清单5-2)。

```text
1,5,2,3,cat
2,7,8,5,dog
3,3,6,7,horse
2,2,8,3,duck
4,4,2,1,mouse

>>> pd.read_csv('ch05_02.csv')
   1  5  2  3    cat
0  2  7  8  5    dog
1  3  3  6  7  horse
2  2  2  8  3   duck
3  4  4  2  1  mouse
```
>> 清单5-2。ch05_02.csv

在这种情况下，通过将头选项设置为None，可以确保是pandas为列指定默认名称。

```python
>>> pd.read_csv('ch05_02.csv', header=None)
   0  1  2  3      4
0  1  5  2  3    cat
1  2  7  8  5    dog
2  3  3  6  7  horse
3  2  2  8  3   duck
4  4  4  2  1  mouse
```
另外，可以通过向names选项分配标签列表来直接指定名称。

```python
>>> pd.read_csv('ch05_02.csv', names=['white','red','blue','green','animal'])
   white  red  blue  green animal
0      1    5     2      3    cat
1      2    7     8      5    dog
2      3    3     6      7  horse
3      2    2     8      3   duck
4      4    4     2      1  mouse
```

在更复杂的情况下，您希望通过读取CSV文件来创建具有层次结构的dataframe，您可以通过添加index_col选项来扩展read_csv()函数的功能，并将所有要转换为索引的列分配给它。

为了更好地理解这种可能性，创建一个新的CSV文件，其中两列用作层次结构的索引。然后，将其保存到工作目录ch05_03中。csv(参见清单5-3)。

```text
color,status,item1,item2,item3
black,up,3,4,6
black,down,2,6,7
white,up,5,5,5
white,down,3,3,2
white,left,1,2,1
red,up,2,2,2
red,down,1,1,4
```
>> 清单5-3。ch05_03.csv

```python
>>> pd.read_csv('ch05_03.csv', index_col=['color','status'])
              item1  item2  item3
color status
black up          3      4      6
      down        2      6      7
white up          5      5      5
      down        3      3      2
      left        1      2      1
red   up          2      2      2
      down        1      1      4
```
      
## 使用RegExp解析TXT文件

在其他情况下，解析数据的文件可能不会显示分隔符，这些分隔符被定义为逗号或分号。在这些情况下，正则表达式可以帮助我们。实际上，您可以使用sep选项在read_table()函数中指定regexp。

为了更好地理解regexp并理解如何将其应用为分割值的标准，让我们从一个简单的例子开始。例如，假设TXT文件的值以不可预知的顺序由空格或制表符分隔。在本例中，您必须使用regexp，因为这是考虑这两种分隔符类型的惟一方法。可以使用通配符\s*来实现。\s表示空格或制表符(如果要表示制表符，可以使用/t)，星号表示可能有多个字符(其他常见通配符见表5-1)。也就是说，值可以用更多的空格或制表符分隔。

[]() | []() 
|------ | -----|
.  | 单字符，换行符除外
\d | 数字
\D | 非数字字符
\s | 空白字符
\S | 非空格字符
\n | 新行字符
\t | 制表符
\uxxxx | Unicode字符由十六进制数字xxxx指定

>> 表5-1。元字符

以一个极端的例子为例，其值用按随机顺序排列的空格或制表符分隔(参见清单5-4)。

```text
white red blue green
    1   5    2    3
    2   7    8    5
    3   3    6    7
```
>> 清单5-4。ch05_04.txt

```python
>>> pd.read_table('ch05_04.txt',sep='\s+', engine='python')
   white  red  blue  green
0      1    5     2      3
1      2    7     8      5
2      3    3     6      7
```
如您所见，结果是一个完美的dataframe，其中的值是完全有序的。

现在您将看到一个示例，它可能看起来很奇怪或不寻常，但它并不像看上去那么罕见。这个示例对于理解regexp的高潜力非常有帮助。实际上，您可能通常认为分隔符是特殊字符，如逗号、空格、制表符等，但实际上您可以考虑分隔符，如字母数字字符，或者例如整数，如0。

在本例中，您需要从TXT文件中提取出数字部分，其中有一组具有数字值的字符，且文字字符完全融合。

当TXT文件中没有列标题时，请记住将标题选项设置为None(参见清单5-5)。

```text
000END123AAA122
001END124BBB321
002END125CCC333
```
>> 清单5-5。ch05_05.txt

```python
>>> pd.read_table('ch05_05.txt', sep='\D+', header=None, engine='python')
   0    1    2
0  0  123  122
1  1  124  321
2  2  125  333
```

另一个相当常见的事件是从解析中排除行。实际上，您并不总是希望在文件中包含头信息或不必要的注释(参见清单5-6)。使用skiprows选项，您可以排除所有您想要的行，只需分配一个包含行号的数组，以便在解析时不考虑。

使用此选项时请注意。如果要排除前5行，必须写skiprows = 5，但如果要排除第5行，必须写skiprows =[5]。

```text
########### LOG FILE ############
This file has been generated by automatic system
white,red,blue,green,animal
12-Feb-2015: Counting of animals inside the house
1,5,2,3,cat
2,7,8,5,dog
13-Feb-2015: Counting of animals outside the house
3,3,6,7,horse
2,2,8,3,duck
4,4,2,1,mouse
```
>> 清单5-6。ch05_06.txt


```python
>>> pd.read_table('ch05_06.txt',sep=',',skiprows=[0,1,3,6])
   white  red  blue  green animal
0      1    5     2      3    cat
1      2    7     8      5    dog
2      3    3     6      7  horse
3      2    2     8      3   duck
4      4    4     2      1  mouse
```    

## 读入TXT文件的一部分

当处理大型文件时，或者您只对这些文件的部分感兴趣时，通常需要将文件读入部分(块)。这既适用于任何迭代，也因为我们对解析整个文件不感兴趣。

例如，如果您只想读取文件的一部分，您可以显式地指定要解析的行数。由于有nrows和skiprows选项，您可以选择起始行n (n = skiprows)和后面要读取的行(nrows = i)。

```python
>>> pd.read_csv('ch05_02.csv',skiprows=[2],nrows=3,header=None)
   0  1  2  3     4
0  1  5  2  3   cat
1  2  7  8  5   dog
2  2  2  8  3  duck
```

另一个有趣且相当常见的操作是将要解析的文本分割成部分。然后，对于每个部分，可以执行一个特定的操作，以获得一个迭代。

例如，您希望每三行添加一列中的值，然后在系列中插入这些和。这个示例很简单，也不实用，但是很容易理解，因此一旦您了解了底层机制，就可以将其应用到更复杂的情况中。

```python
>>> out = pd.Series()
>>> i = 0
>>> pieces = pd.read_csv('ch05_01.csv',chunksize=3)
>>> for piece in pieces:
...    out.set_value(i,piece['white'].sum())
...    i = i + 1
...
0    6
dtype: int64
0    6
1    6
dtype: int64
>>> out
0    6
1    6
dtype: int64
```

## 将数据写入CSV文件

除了读取文件中包含的数据外，把由计算产生的或数据结构中包含的数据写入文件也很普遍。例如，您可能希望将dataframe中的数据写入CSV文件。为了完成这个编写过程，您将使用to_csv()函数，它接受一个 参数设置生成的文件的名称(请参见清单5-7)。

```python
>>> frame = pd.DataFrame(np.arange(16).reshape((4,4)),
               index = ['red', 'blue', 'yellow', 'white'],
               columns = ['ball', 'pen', 'pencil', 'paper'])
>>> frame.to_csv('ch05_07.csv')
```

如果打开由panda库生成的名为ch05_07的csv文件，您将看到如清单5-7所示的数据。

```text
,ball,pen,pencil,paper
0,1,2,3
4,5,6,7
8,9,10,11
12,13,14,15
```
>> 清单5-7。ch05_07.csv

从前面的示例中可以看到，当您将dataframe写入文件时，默认情况下会在文件上标记索引和列。通过将两个选项索引和标头设置为False，可以更改此默认行为(参见清单5-8)。

```python
>>> frame.to_csv('ch05_07b.csv', index=False, header=False)
```

```text
1,2,3
5,6,7
9,10,11
13,14,15
```
>> 清单5-8。ch05_07b.csv

编写文件时要记住的一点是，数据结构中的NaN值显示为文件中的空字段(参见清单5-9)。

```python
>>> frame3 = pd.DataFrame([[6,np.nan,np.nan,6,np.nan],
...           [np.nan,np.nan,np.nan,np.nan,np.nan],
...           [np.nan,np.nan,np.nan,np.nan,np.nan],
...           [20,np.nan,np.nan,20.0,np.nan],
...           [19,np.nan,np.nan,19.0,np.nan]
...           ],
...                  index=['blue','green','red','white','yellow'],
                     columns=['ball','mug','paper','pen','pencil'])
>>> frame3
        ball  mug  paper  pen  pencil
blue     6.0  NaN    NaN  6.0     NaN
green    NaN  NaN    NaN  NaN     NaN
red      NaN  NaN    NaN  NaN     NaN
white   20.0  NaN    NaN 20.0     NaN
yellow  19.0  NaN    NaN 19.0     NaN

>>> frame3.to_csv('ch05_08.csv')
```

```text
,ball,mug,paper,pen,pencil
blue,6.0,,,6.0,
green,,,,,
red,,,,,
white,20.0,,,20.0,
yellow,19.0,,,19.0,
```
>> 清单5-9。ch05_08.csv

但是，您可以使用to_csv()函数中的na_rep选项，用您喜欢的值替换这个空字段。公共值可以为NULL、0或相同的NaN(请参见清单5-10)。

```python
>>> frame3.to_csv('ch05_09.csv', na_rep ='NaN')
```
```text
,ball,mug,paper,pen,pencil
blue,6.0,NaN,NaN,6.0,NaN
green,NaN,NaN,NaN,NaN,NaN
red,NaN,NaN,NaN,NaN,NaN
white,20.0,NaN,NaN,20.0,NaN
yellow,19.0,NaN,NaN,19.0,NaN
```
>> 清单5-10。ch05_09.csv

> 注意，在指定的情况下，dataframe一直是讨论的主题，因为这些是写入文件的数据结构。但是所有这些函数和选项对于本系列也是有效的。

