---
title: 十分钟搞定pandas
date: 2017-05-23 09:39:20
categories: IT技术
tags: pandas
---
## 转载：http://www.cnblogs.com/chaosimple/p/4153083.html

本文是对pandas官方网站上《10 Minutes to pandas》的一个简单的翻译，原文在[这里](http://pandas.pydata.org/pandas-docs/stable/10min.html)。这篇文章是对pandas的一个简单的介绍，详细的介绍请参考：[*Cookbook*](http://pandas.pydata.org/pandas-docs/stable/cookbook.html#cookbook) 。习惯上，我们会按下面格式引入所需要的包：

![](http://upload-images.jianshu.io/upload_images/1713353-dfd774ed2bebfaca.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 一、            创建对象

可以通过 [Data Structure Intro Setion](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dsintro) 来查看有关该节内容的详细信息。

1、可以通过传递一个list对象来创建一个Series，pandas会默认创建整型索引

![](http://upload-images.jianshu.io/upload_images/1713353-ddb42b747b1b79e3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、通过传递一个numpy array，时间索引以及列标签来创建一个DataFrame

![](http://upload-images.jianshu.io/upload_images/1713353-81aaa38badc895b8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、通过传递一个能够被转换成类似序列结构的字典对象来创建一个DataFrame

![](http://upload-images.jianshu.io/upload_images/1713353-94cf23eb77506117.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、查看不同列的数据类型

![](http://upload-images.jianshu.io/upload_images/1713353-4d2ccb2c538c9135.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、如果你使用的是IPython，使用Tab自动补全功能会自动识别所有的属性以及自定义的列，下图中是所有能够被自动识别的属性的一个子集

![](http://upload-images.jianshu.io/upload_images/1713353-670dd2d89bfca1ee.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 二、            查看数据

详情请参阅：[Basics Section](http://pandas.pydata.org/pandas-docs/stable/basics.html#basics)

 

1、  查看frame中头部和尾部的行

![](http://upload-images.jianshu.io/upload_images/1713353-fbc4a0e5ad615551.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  显示索引、列和底层的numpy数据

![](http://upload-images.jianshu.io/upload_images/1713353-ac3898808f1f973d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、  describe()函数对于数据的快速统计汇总

![](http://upload-images.jianshu.io/upload_images/1713353-9c65e343e671f126.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、  对数据的转置

![](http://upload-images.jianshu.io/upload_images/1713353-d61650bc59ee1ee9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、  按轴进行排序

![](http://upload-images.jianshu.io/upload_images/1713353-796aa72a17b68d2c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6、  按值进行排序

![](http://upload-images.jianshu.io/upload_images/1713353-3e0a8c118906c711.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三、            选择

虽然标准的Python/Numpy的选择和设置表达式都能够直接派上用场，但是作为工程使用的代码，我们推荐使用经过优化的pandas数据访问方式：.at, .iat, .loc, .iloc 和 .ix详情请参阅[Indexing and Selecing Data](http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing) 和 [MultiIndex / Advanced Indexing](http://pandas.pydata.org/pandas-docs/stable/advanced.html#advanced)。

### 获取

1、 选择一个单独的列，这将会返回一个Series，等同于df.A：

![](http://upload-images.jianshu.io/upload_images/1713353-1400e9d502365b00.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、 通过[]进行选择，这将会对行进行切片

![](http://upload-images.jianshu.io/upload_images/1713353-6e0267abc8e1015b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 通过标签选择

1、 使用标签来获取一个交叉的区域

![](http://upload-images.jianshu.io/upload_images/1713353-63f0fd2319b82f42.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、 通过标签来在多个轴上进行选择

![](http://upload-images.jianshu.io/upload_images/1713353-2ae02229f13bd18f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、 标签切片

![](http://upload-images.jianshu.io/upload_images/1713353-a929c6c0a280ba21.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、 对于返回的对象进行维度缩减

![](http://upload-images.jianshu.io/upload_images/1713353-4b83fbb3abbce743.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、 获取一个标量

![](http://upload-images.jianshu.io/upload_images/1713353-f9c7d391f0209914.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6、 快速访问一个标量（与上一个方法等价）

![](http://upload-images.jianshu.io/upload_images/1713353-19e09829443aa48b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 通过位置选择

1、 通过传递数值进行位置选择（选择的是行）

![](http://upload-images.jianshu.io/upload_images/1713353-cf434591f4cb29bf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、 通过数值进行切片，与numpy/python中的情况类似

![](http://upload-images.jianshu.io/upload_images/1713353-466470876fb9dc8c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、 通过指定一个位置的列表，与numpy/python中的情况类似

![](http://upload-images.jianshu.io/upload_images/1713353-f73c2d384d211480.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、 对行进行切片

![](http://upload-images.jianshu.io/upload_images/1713353-9804069e3af205e7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、 对列进行切片

![](http://upload-images.jianshu.io/upload_images/1713353-7bc2f3d0b65e2a11.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6、 获取特定的值

![](http://upload-images.jianshu.io/upload_images/1713353-e2d47762b46e283e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 布尔索引

1、 使用一个单独列的值来选择数据

![](http://upload-images.jianshu.io/upload_images/1713353-9de1220192dc9bb4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、 使用where操作来选择数据

![](http://upload-images.jianshu.io/upload_images/1713353-7323aae0cf830405.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、 使用isin()方法来过滤

![](http://upload-images.jianshu.io/upload_images/1713353-6f275ea1bf339f52.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

### 设置

1、 设置一个新的列

![](http://upload-images.jianshu.io/upload_images/1713353-b6c874f1f619c85c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、 通过标签设置新的值

![](http://upload-images.jianshu.io/upload_images/1713353-74d758b132cb8e20.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、 通过位置设置新的值

![](http://upload-images.jianshu.io/upload_images/1713353-0ea35ded04b6bedb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、 通过一个numpy数组设置一组新值

![](http://upload-images.jianshu.io/upload_images/1713353-35a73ff0eb880e00.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上述操作结果如下：

![](http://upload-images.jianshu.io/upload_images/1713353-ee4bacac40e9fe44.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、 通过where操作来设置新的值

![](http://upload-images.jianshu.io/upload_images/1713353-9302c44bdb45f033.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 四、            缺失值处理

在pandas中，使用np.nan来代替缺失值，这些值将默认不会包含在计算中，详情请参阅：[Missing Data Section](http://pandas.pydata.org/pandas-docs/stable/missing_data.html#missing-data)。

1、  reindex()方法可以对指定轴上的索引进行改变/增加/删除操作，这将返回原始数据的一个拷贝：

![](http://upload-images.jianshu.io/upload_images/1713353-95f9c35ef832b06f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  去掉包含缺失值的行

![](http://upload-images.jianshu.io/upload_images/1713353-ae4d2e8ba2cd41ed.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、  对缺失值进行填充

![](http://upload-images.jianshu.io/upload_images/1713353-06564a4322d30232.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、  对数据进行布尔填充

![](http://upload-images.jianshu.io/upload_images/1713353-7fd698c9d9a95490.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 五、            相关操作

详情请参与 [Basic Section On Binary Ops](http://pandas.pydata.org/pandas-docs/stable/basics.html#basics-binop)

### 统计（相关操作通常情况下不包括缺失值）

1、  执行描述性统计

![](http://upload-images.jianshu.io/upload_images/1713353-879b119d3a4d3490.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  在其他轴上进行相同的操作

![](http://upload-images.jianshu.io/upload_images/1713353-5ca87404a20c51bf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、  对于拥有不同维度，需要对齐的对象进行操作。Pandas会自动的沿着指定的维度进行广播

![](http://upload-images.jianshu.io/upload_images/1713353-14adfdc648d61296.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Apply

1、  对数据应用函数

![](http://upload-images.jianshu.io/upload_images/1713353-ecf8c1f85e3f93a7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 直方图

具体请参照：[*Histogramming and Discretization*](http://pandas.pydata.org/pandas-docs/stable/basics.html#basics-discretization)

![](http://upload-images.jianshu.io/upload_images/1713353-a293657c64c9a288.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

### 字符串方法

Series对象在其str属性中配备了一组字符串处理方法，可以很容易的应用到数组中的每个元素，如下段代码所示。更多详情请参考：[*Vectorized String Methods*](http://pandas.pydata.org/pandas-docs/stable/text.html#text-string-methods).

![](http://upload-images.jianshu.io/upload_images/1713353-9376cb8ecd849f6f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 六、            合并

Pandas提供了大量的方法能够轻松的对Series，DataFrame和Panel对象进行各种符合各种逻辑关系的合并操作。具体请参阅：[*Merging section*](http://pandas.pydata.org/pandas-docs/stable/merging.html#merging)

### Concat

![](http://upload-images.jianshu.io/upload_images/1713353-327b3bc6654dc123.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Join 
类似于SQL类型的合并，具体请参阅：[*Database style joining*](http://pandas.pydata.org/pandas-docs/stable/merging.html#merging-join)

![](http://upload-images.jianshu.io/upload_images/1713353-2dceed5445496414.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Append 
将一行连接到一个DataFrame上，具体请参阅[*Appending*](http://pandas.pydata.org/pandas-docs/stable/merging.html#merging-concatenation)：

![](http://upload-images.jianshu.io/upload_images/1713353-f78e2dbf7459edca.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 七、            分组groupby

对于”group by”操作，我们通常是指以下一个或多个操作步骤：

- （Splitting）按照一些规则将数据分为不同的组；

- （Applying）对于每组数据分别执行一个函数；

- （Combining）将结果组合到一个数据结构中；

详情请参阅：[*Grouping section*](http://pandas.pydata.org/pandas-docs/stable/groupby.html#groupby)

![](http://upload-images.jianshu.io/upload_images/1713353-e8d0e83216a73434.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1、  分组并对每个分组执行sum函数

![](http://upload-images.jianshu.io/upload_images/1713353-897742e63f1a516c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  通过多个列进行分组形成一个层次索引，然后执行函数

![](http://upload-images.jianshu.io/upload_images/1713353-9e6ccb9bf1d84670.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

统计每个分组的数量
![深度截图20170523093706.png](http://upload-images.jianshu.io/upload_images/1713353-cce4da70255aca29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 八、            Reshaping

详情请参阅 [*Hierarchical Indexing*](http://pandas.pydata.org/pandas-docs/stable/advanced.html#advanced-hierarchical) 和 [*Reshaping*](http://pandas.pydata.org/pandas-docs/stable/reshaping.html#reshaping-stacking)。

###  Stack

![](http://upload-images.jianshu.io/upload_images/1713353-fb8289469618264a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1713353-2c93f7da169b27be.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1713353-dec24a06730e707c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 数据透视表
详情请参阅：[*Pivot Tables*](http://pandas.pydata.org/pandas-docs/stable/reshaping.html#reshaping-pivot).

![](http://upload-images.jianshu.io/upload_images/1713353-085763f337ceb2df.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以从这个数据中轻松的生成数据透视表：

![](http://upload-images.jianshu.io/upload_images/1713353-a36a387c2b2b4671.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 九、            时间序列

Pandas在对频率转换进行重新采样时拥有简单、强大且高效的功能（如将按秒采样的数据转换为按5分钟为单位进行采样的数据）。这种操作在金融领域非常常见。具体参考：[*Time Series section*](http://pandas.pydata.org/pandas-docs/stable/timeseries.html#timeseries)。

![](http://upload-images.jianshu.io/upload_images/1713353-69e1450be5c56505.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1、  时区表示：

![](http://upload-images.jianshu.io/upload_images/1713353-f024087929c6ac35.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  时区转换：

![](http://upload-images.jianshu.io/upload_images/1713353-1919bf0f7feb9012.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、  时间跨度转换：

![](http://upload-images.jianshu.io/upload_images/1713353-c6ef92ab2625707f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、  时期和时间戳之间的转换使得可以使用一些方便的算术函数。

![](http://upload-images.jianshu.io/upload_images/1713353-109643399f55db0d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 十、            Categorical

从0.15版本开始，pandas可以在DataFrame中支持Categorical类型的数据，详细 介绍参看：[*categorical introduction*](http://pandas.pydata.org/pandas-docs/stable/categorical.html#categorical)和[*API documentation*](http://pandas.pydata.org/pandas-docs/stable/api.html#api-categorical)。

![](http://upload-images.jianshu.io/upload_images/1713353-de952ff4417c61ca.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1、  将原始的grade转换为Categorical数据类型

![](http://upload-images.jianshu.io/upload_images/1713353-81d1a1e2ea849ffd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  将Categorical类型数据重命名为更有意义的名称

![](http://upload-images.jianshu.io/upload_images/1713353-b9e976b63d40f377.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、  对类别进行重新排序，增加缺失的类别

![](http://upload-images.jianshu.io/upload_images/1713353-54938c689f3a5d22.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、  排序是按照Categorical的顺序进行的而不是按照字典顺序进行

![](http://upload-images.jianshu.io/upload_images/1713353-541f26ba6f0c1831.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、  对Categorical列进行排序时存在空的类别

![](http://upload-images.jianshu.io/upload_images/1713353-82bf487d4f77ba58.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 十一、           画图

具体文档参看：[*Plotting*](http://pandas.pydata.org/pandas-docs/stable/visualization.html#visualization) docs

![](http://upload-images.jianshu.io/upload_images/1713353-8227cd7e4c0a6391.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对于DataFrame来说，plot是一种将所有列及其标签进行绘制的简便方法：

![](http://upload-images.jianshu.io/upload_images/1713353-810d399bc1fa6961.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1713353-9399e8796578cf14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 十二、           导入和保存数据

### CSV，参考：[*Writing to a csv file*](http://pandas.pydata.org/pandas-docs/stable/io.html#io-store-in-csv)

1、  写入csv文件

![](http://upload-images.jianshu.io/upload_images/1713353-2af8d6b333da49c9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  从csv文件中读取

![](http://upload-images.jianshu.io/upload_images/1713353-1127cb23ed5154a9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###  HDF5
参考：[*HDFStores*](http://pandas.pydata.org/pandas-docs/stable/io.html#io-hdf5)

1、  写入HDF5存储

![](http://upload-images.jianshu.io/upload_images/1713353-7a59a38fc659cb04.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  从HDF5存储中读取

![](http://upload-images.jianshu.io/upload_images/1713353-f6052a47257f81fd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Excel
参考：[*MS Excel*](http://pandas.pydata.org/pandas-docs/stable/io.html#io-excel)

1、  写入excel文件

![](http://upload-images.jianshu.io/upload_images/1713353-2acee68edb072926.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、  从excel文件中读取

![](http://upload-images.jianshu.io/upload_images/1713353-ba06500c20590760.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
