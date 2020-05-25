# Label Encoder vs. One Hot Encoder in Machine Learning

credit:
https://towardsdatascience.com/columntransformer-in-scikit-for-labelencoding-and-onehotencoding-in-machine-learning-c6255952731b
https://zhuanlan.zhihu.com/p/36804348

如果您是机器学习的新手，则可能会对这两者（标签编码器和热编码器）感到困惑。 这两个编码器是Python的SciKit Learn库的一部分，用于将分类数据或文本数据转换为数字，我们的预测模型可以更好地理解它们。 今天，让我们通过一个简单的例子来了解两者之间的区别。

## 1.Label Encoding

![LE](https://user-images.githubusercontent.com/61290493/82826087-0ecd0900-9e72-11ea-8d61-fd52839c4029.png)

在此示例中，第一列是国家列，即所有文本。 如您现在所知，如果要在模型上运行任何类型的模型，则数据中都不能包含文本。 因此，在运行模型之前，我们需要为模型准备好这些数据。

并将此类分类文本数据转换为模型可理解的数值数据，我们使用Label Encoder类。 因此，要做的就是对第一列进行标签编码，就是从sklearn库中导入LabelEncoder类，对数据的第一列进行拟合和转换，然后将现有的文本数据替换为新的编码数据。 让我们看一下代码。

```html
from sklearn.preprocessing import LabelEncoder
labelencoder = LabelEncoder()
x[:, 0] = labelencoder.fit_transform(x[:, 0])
```

我们假设数据位于名为“ x”的变量中。 运行这段代码后，如果检查x的值，则会看到第一列中的三个国家/地区已被数字0、1和2代替。

这就是标签编码的全部内容。 但是根据数据，标签编码会引入新的问题。 例如，我们已经将一组国家/地区名称编码为数字数据。 这实际上是分类数据，行之间没有任何形式的关系。

这里的问题是，由于同一列中的数字不同，因此该模型会误解数据的排列顺序，即0 <1 <2。但这不是事实。 为了克服这个问题，我们使用一个热编码器。

## 2.One Hot Encoder
热编码所做的是，它获取一列具有分类数据的列，该列已被标签编码，然后将该列拆分为多个列。数字将由1和0代替，具体取决于哪一列的值。在我们的示例中，我们将获得三个新列，每个国家/地区分别是法国，德国和西班牙。

对于第一列值为法国的行，“法国”列将具有“ 1”，而其他两列将具有“ 0”。同样，对于第一列值为德国的行，“德国”列将为“ 1”，其他两列将为“ 0”。
```html
from sklearn.preprocessing import OneHotEncoder
onehotencoder = OneHotEncoder(categorical_features = [0])
x = onehotencoder.fit_transform(x).toarray()
```
如您在构造函数中看到的，我们指定哪一列必须是一个热编码的，在这种情况下为[0]。 然后，使用刚刚创建的onehotencoder对象拟合并转换数组“ x”。 就是这样，我们现在在数据集中有了三个新列：

## 3.总结
A类模型：有些模型的损失函数对数值大小是敏感的，即变量间的数值大小本身是有比较意义的，如逻辑回归，SVM等

B类模型：有些模型本身对数值变化不敏感，数值存在的意义更多的是为了排序，即0.1,0.2,0.3与1,2,3是没有区别的，这部分模型绝大部分是树模型，暂将其称为B类模型。

类别变量：
细分可以分成有序变量和无序变量，典型的有序变量就是学历，如博士研究生，硕士研究生，本科生等在业务含义上本身是有高低之分的。而无序变量在业务含义上是无序的，如品牌等。

label encoding是将类别变量中每一类别赋一数值，从而转换成数值型。比如有一列 [dog,cat,dog,mouse,cat]，我们把其转换为[1,2,1,3,2]。这里就产生了一个奇怪的现象：dog和mouse的平均值是cat，所以label encoding最直观的缺点就是赋值难以解释，适用场景更窄。

one hot encoding的优点就是它的值只有0/1，不同的类型存储在垂直的空间。缺点就是，当类别的数量很多时，特征空间会变得非常大。

## 4.如何选择？
#### A类模型

如果使用的是A类模型，所有的类别变量必须做one hot encoding，因为label encoding的赋值是没有数值含义的。

但是对于类别很多的变量，做one hot encoding会使得生成的变量过于稀疏，所以这里有一些经验上的处理方式。优先考虑的是有没业务上类别合并的方法，如城市变量，可以依发展程度分为一线城市，二线城市等等；另外一种方法是只one hot出现次数最多的前n个类别，其他类别放在其他类的变量中；也可以利用y值（训练集）中positive rate做合并，不过容易出现过拟合的现象。

#### B类模型

如果使用的是B类模型，并且是有序变量，则优先使用label encoding，且赋值要与顺序一致。

如果是无序变量，则两种方法在很多情况下差别不大，但是在实际使用中label encoding的效果一般要比one hot encoding要好。这是因为在树模型中，label encoding至少可以完成one hot encoding同样的效果，而多出来的那部分信息则是label encoding后的数值本身是有排序作用的，它可以起到类别变量合并的效果，这种效果在类别较多的变量中更明显。
