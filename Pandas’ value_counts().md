# Getting more value from the Pandas’ value_counts()

Credit:https://towardsdatascience.com/getting-more-value-from-the-pandas-value-counts-aa17230907a6

数据探索是机器学习管道的重要方面。 在决定要训练哪种模型以及要训练多少模型之前，我们必须对数据包含的内容有所了解。 为此，Pandas库配备了许多有用的功能，其中value_counts是其中之一。 此函数返回熊猫数据框中唯一项目的计数。 但是，大多数情况下，我们最终会使用带有默认参数的value_counts。 因此，在这篇简短的文章中，我将向您展示如何通过更改默认参数来实现更多目标。

## value_counts（）
value_counts（）方法返回一个包含唯一值计数的Series。 这意味着，对于数据帧中的任何列，此方法都会返回该列中唯一条目的计数。

#### 样式
Series.value_counts()

#### 参数
