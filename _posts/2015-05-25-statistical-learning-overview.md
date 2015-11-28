---
published: true
layout: post
title: 统计学习方法概述
date: 2015-05-25
---

## 统计学习三要素
### 模型(model)
模型即为所要学习的条件概率分布或决策函数。
```
由决策函数表示的模型为非概率模型。
由条件概率表示的模型为概率模型。
```

### 策略(strategy)
按照什么样的准则学习或选择最优模型。
```
经验风险最小化(ERM)会产生过拟合问题
结构风险最小化(SRM)等价于正则化
```

### 算法(algorithm)
学习模型的具体计算方法。

<!-- more -->

## 模型选择的方法
### 正则化(regularization)
正则化即为在经验风险上加一个正则化项，正则化项一般是模型复杂度的单调递增函数，模型越复杂，正则化项就越大。它一般是参数向量的L2范数，作用是选择经验风险与模型复杂度同时较小的模型。

### 交叉验证(cross-validation)
将数据集随机分为三部分：
```
训练集(training set): 训练模型，占比60%
交叉验证集(cross validation set): 模型的选择，占比20%
测试集(test set): 最终对学习方法的评估，占比20%
```

## 泛化能力
该方法学习到的模型对未知数据的预测能力。

## 二类分类问题的常用评价指标F1值
```
TP(True Positive): 将正类预测为正类数
FN(False Negative): 将正类预测为负类数
FP(False Positive): 将负类预测为正类数
TN(True Negative): 将负类预测为负类数
```

精确率(precision): $$P = \frac{TP}{TP + FP}$$

召回率(recall): $$R = \frac{TP}{TP + FN}$$

F1值为P和R的调和均值: $$\frac{2}{F1}=\frac{1}{P} + \frac{1}{R}$$

$$F1 = \frac{2TP}{2TP + FP + FN} = \frac{2PR}{P + R}$$

## 参考资料
[《统计学习方法》](https://book.douban.com/subject/10590856/)
