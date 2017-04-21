---
title: 用knn算法实现手写字符识别
date: 2016-09-19 13:46:25
tags: 
- 机器学习
categories: 机器学习
---

# 概述

kNN算法又称为k近邻分类(k-nearest neighbor classification)算法。kNN算法是从训练集中找到和新数据最接近的k条记录，然后根据这k挑记录的主要分类来决定新数据的类别。该算法涉及3个主要因素：训练集、距离或相似的衡量、k的大小。

# 优缺点及适用范围

- 优点

精度高，对异常值不敏感，无数据输入假定

- 缺点

计算复杂度高，空间复杂度高

- 适用范围

数值型，标称型



# 相似度计算方法

相似度有两种最简单的计算方法：欧氏距离和余弦值

A=[1,2,3], B=[0,0,0]

1. 欧氏距离

`dist(A,B) = numpy.linalg.norm(A-B)`

或

`dist(A-B) = (((A-B)**2).sum())**0.5`

2. 余弦值

`cos(A,B) = A*B/|A||B|`

# 代码

- 通过欧氏距离来计算

```

def knn_distance(self, test_data, train_data, labels, k):
   data_size = train_data.shape[0]
   diff_mat = np.tile(test_data, (data_size, 1)) - train_data
   sq_diff_mate = diff_mat ** 2
   distance = sq_diff_mate.sum(axis=1) ** 0.5
   # distance = np.linalg.norm(diff_mat, axis=1)
   sorted_dist_indicies = np.argsort(distance)
   class_count = {}
   for i in range(k):
       vote_label = labels[sorted_dist_indicies[i]]
       class_count[vote_label] = class_count.get(vote_label, 0) + 1
   sorted_class_count = sorted(class_count.items(), key=operator.itemgetter(1), reverse=True)
   return sorted_class_count[0][0]

```

- 通过余弦值来计算

```

def knn_cos(self, train_x, train_y, test_x):
    """
    归一化处理
    linalg.norm(x), return sum(abs(xi)**2)**0.5
    """
    norms_train = np.apply_along_axis(np.linalg.norm, 1, train_x) + 1.0e-7
    norms_test = np.apply_along_axis(np.linalg.norm, 1, test_x) + 1.0e-7
    train_x = train_x / np.expand_dims(norms_train, -1)
    test_x = test_x / np.expand_dims(norms_test, -1)

    # cosine
    corr = np.dot(test_x, np.transpose(train_x))
    argmax = np.argmax(corr, axis=1)
    preds = train_y[argmax]
    return preds
```

- 通过scikit-learn包来计算

```

def knn_sklearn(self, train_data, train_label, test_data):
    pca = PCA(n_components=0.8)
    train_x = pca.fit_transform(train_data)
    test_x = pca.transform(test_data)
    # knn regression
    neighbor = KNeighborsClassifier(n_neighbors=4)
    neighbor.fit(train_x, train_label)
    preds = neighbor.predict(test_x)
    return preds
```

# 运行结果
![](http://oceas72q5.bkt.clouddn.com/knn%E6%89%A7%E8%A1%8C%E7%BB%93%E6%9E%9C.png)


# 完整代码

[knn算法](https://github.com/jllan/ml-knn)
