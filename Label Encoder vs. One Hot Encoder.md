# Label Encoder vs. One Hot Encoder in Machine Learning

credit:https://towardsdatascience.com/columntransformer-in-scikit-for-labelencoding-and-onehotencoding-in-machine-learning-c6255952731b

如果您是机器学习的新手，则可能会对这两者（标签编码器和热编码器）感到困惑。 这两个编码器是Python的SciKit Learn库的一部分，用于将分类数据或文本数据转换为数字，我们的预测模型可以更好地理解它们。 今天，让我们通过一个简单的例子来了解两者之间的区别。

## Label Encoding


在此示例中，第一列是国家列，即所有文本。 如您现在所知，如果要在模型上运行任何类型的模型，则数据中都不能包含文本。 因此，在运行模型之前，我们需要为模型准备好这些数据。

并将此类分类文本数据转换为模型可理解的数值数据，我们使用Label Encoder类。 因此，要做的就是对第一列进行标签编码，就是从sklearn库中导入LabelEncoder类，对数据的第一列进行拟合和转换，然后将现有的文本数据替换为新的编码数据。 让我们看一下代码。

```html
from sklearn.preprocessing import LabelEncoder
labelencoder = LabelEncoder()
x[:, 0] = labelencoder.fit_transform(x[:, 0])
```

我们假设数据位于名为“ x”的变量中。 运行这段代码后，如果检查x的值，则会看到第一列中的三个国家/地区已被数字0、1和2代替。

