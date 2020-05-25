# Train Test Split and Cross Validation

## 1.什么是模型的过拟合/欠拟合？
如前所述，在统计和机器学习中，我们通常将数据分为两个子集：训练数据和测试数据（有时分为三个子集：训练，验证和测试），并在训练数据上拟合我们的模型，以便对测试数据。

### 过度拟合
过度拟合意味着我们训练的模型训练得“太好了”，现在太适合训练数据集了。这通常在模型过于复杂时发生（即与观察数量相比，特征/变量太多）。该模型在训练数据上将非常准确，但是在未训练或新数据上可能会非常不准确。这是因为此模型未通用化.基本上，当发生这种情况时，模型将学习或描述训练数据中的“噪声”，而不是数据中变量之间的实际关系。显然，这种噪声不是任何新数据集的一部分，也无法对其应用。

### 欠拟合
与过度拟合相反，当模型拟合不足时，这意味着该模型不适合训练数据，因此会错过数据中的趋势。这也意味着该模型无法推广到新数据。这通常是一个非常简单的模型（没有足够的预测变量/独立变量）导致的。例如，当我们将线性模型（如线性回归）拟合到非线性数据时，也会发生这种情况。不用说，该模型的预测能力很差（在训练数据上，不能推广到其他数据）

值得注意的是，拟合不足不如拟合过度常见。 但是，我们希望避免在数据分析中同时遇到这两个问题。 您可能会说我们正在尝试寻找模型不足和过度拟合之间的中间点。 如您所见，训练/测试拆分和交叉验证有助于避免过度拟合而不是欠拟合。

## 2.训练/测试拆分
正如我之前所说，我们使用的数据通常分为训练数据和测试数据。 训练集包含一个已知的输出，并且模型在此数据上学习，以便稍后推广到其他数据。 我们具有测试数据集（或子集），以便测试我们对该子集的模型预测。
```html
import pandas as pd
from sklearn import datasets, linear_model
from sklearn.model_selection import train_test_split
from matplotlib import pyplot as plt
```
训练/测试拆分确实有其危险-如果我们进行的拆分不是随机的怎么办？ 如果我们数据的一个子集仅包含来自某个州的人员，具有一定收入水平的员工，而没有其他收入水平，只有女性或只有特定年龄的人员，该怎么办？ （想象一个由这些命令之一排序的文件）。 即使我们试图避免过度拟合，也会导致过度拟合！这是交叉验证的来源。

## 3.交叉验证
在上一段中，我提到了训练/测试拆分方法中的注意事项。 为了避免这种情况，我们可以执行称为交叉验证的操作。 它与训练/测试拆分非常相似，但适用于更多子集。 意思是，我们将数据分为k个子集，并在k-1个子集中训练。 我们要做的是保留最后一个子集进行测试。 我们能够为每个子集做到这一点。

有很多交叉验证方法，我将介绍其中的两种：第一种是K-folds交叉验证，第二种是“留出一个”交叉验证（LOOCV）

### K折交叉验证
在K折交叉验证中，我们将数据分为k个不同的子集（或折）。 我们使用k-1个子集来训练我们的数据，并保留最后一个子集（或最后一个折叠）作为测试数据。 然后，我们针对每个折痕对模型进行平均，然后最终确定模型。 之后，我们针对测试集对其进行测试。
```html
>>> import numpy as np
>>> from sklearn.model_selection import KFold
>>> X = np.array([[1, 2], [3, 4], [1, 2], [3, 4]])
>>> y = np.array([1, 2, 3, 4])
>>> kf = KFold(n_splits=2)
>>> kf.get_n_splits(X)
2
>>> print(kf)
KFold(n_splits=2, random_state=None, shuffle=False)
>>> for train_index, test_index in kf.split(X):
...     print("TRAIN:", train_index, "TEST:", test_index)
...     X_train, X_test = X[train_index], X[test_index]
...     y_train, y_test = y[train_index], y[test_index]
TRAIN: [2 3] TEST: [0 1]
TRAIN: [0 1] TEST: [2 3]
```

### 留一法（LOOCV，Leave One Out Cross Validation）
留下一个交叉验证，在这种类型的交叉验证中，折叠（子集）的数量等于我们在数据集中观察到的数量。 然后，我们对所有这些折叠进行平均，并使用平均值构建模型。 然后，我们针对最后的折叠测试模型。 由于我们将获得大量的训练集（等于样本数量，K=n），因此该方法的计算量非常大，应在小型数据集上使用。 如果数据集很大，则最好使用其他方法，例如kfold。
```html
>>> import numpy as np
>>> from sklearn.model_selection import KFold
>>> X = np.array([[1, 2], [3, 4], [1, 2], [3, 4]])
>>> y = np.array([1, 2, 3, 4])
>>> kf = KFold(n_splits=2)
>>> kf.get_n_splits(X)
2
>>> print(kf)
KFold(n_splits=2, random_state=None, shuffle=False)
>>> for train_index, test_index in kf.split(X):
...     print("TRAIN:", train_index, "TEST:", test_index)
...     X_train, X_test = X[train_index], X[test_index]
...     y_train, y_test = y[train_index], y[test_index]
TRAIN: [2 3] TEST: [0 1]
TRAIN: [0 1] TEST: [2 3]
```
当K值增大时，我们将减少由于偏差引起的误差，但由于方差导致的误差增加； 很明显，计算成本也会上涨-折叠的次数越多，计算时间就越长，并且将需要更多的内存
。
当K值时，我们会减少由于方差引起的误差，但由于偏斜引起的误差会更大。 在计算上也更便宜。 因此，在大数据集中，通常建议k = 3。 如前所述，在较小的数据集中，最好使用LOOCV。

_____
让我们看看我以前使用的示例，这次是使用交叉验证。 我将使用cross_val_predict函数返回每个数据点在测试切片中的预测值。
```html
# Necessary imports: 
from sklearn.model_selection import cross_val_score, cross_val_predict
from sklearn import metrics

# Perform 6-fold cross validation
scores = cross_val_score(model, df, y, cv=6)
print “Cross-validated scores:”, scores
Cross-validated scores: [ 0.4554861   0.46138572  0.40094084  0.55220736  0.43942775  0.56923406]
```
