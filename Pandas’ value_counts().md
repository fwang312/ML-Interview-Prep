# Getting more value from the Pandas’ value_counts()

Credit:https://towardsdatascience.com/getting-more-value-from-the-pandas-value-counts-aa17230907a6

数据探索是机器学习管道的重要方面。 在决定要训练哪种模型以及要训练多少模型之前，我们必须对数据包含的内容有所了解。 为此，Pandas库配备了许多有用的功能，其中value_counts是其中之一。 此函数返回熊猫数据框中唯一项目的计数。 但是，大多数情况下，我们最终会使用带有默认参数的value_counts。 因此，在这篇简短的文章中，我将向您展示如何通过更改默认参数来实现更多目标。

## value_counts（）
value_counts（）方法返回一个包含唯一值计数的Series。 这意味着，对于数据帧中的任何列，此方法都会返回该列中唯一条目的计数。

### 样式
Series.value_counts()

### 参数
<img width="751" alt="value_count_parameter" src="https://user-images.githubusercontent.com/61290493/83193499-c2e1c480-a0fc-11ea-8e28-9673ab92f66a.png">

### 基本用法
让我们通过数据集了解此方法的基本用法。 我将使用Titanic数据集进行演示。 我还在Kaggle上发布了一个随附的笔记本，以防您想直接接触这些代码。

#### 1. value_counts() with default parameters
Let’s call the value_counts() on the Embarked column of the dataset. This will return the count of unique occurrences in this column.
```html
train['Embarked'].value_counts()
-------------------------------------------------------------------
S      644
C      168
Q       77
```
The function returns the count of all unique values in the given index in descending order without any null values. We can quickly see that the maximum people embarked from Southampton, followed by Cherbourg and then Queenstown.

#### 2.value_counts() with relative frequencies of the unique values.
Sometimes, getting a percentage is a better criterion then the count. By setting normalize=True, the object returned will contain the relative frequencies of the unique values. The normalizeparameter is set to False by default.
```html
train['Embarked'].value_counts(normalize=True)
-------------------------------------------------------------------
S    0.724409
C    0.188976
Q    0.086614
```
Knowing that 72% of people embarked from Southampton is a better metric than saying 644 people embarked from Southampton.

#### 3. value_counts() in ascending order
The series returned by value_counts() is in descending order by default. We can reverse the case by setting the ascending parameter to True.
```html
train['Embarked'].value_counts(ascending=True)
-------------------------------------------------------------------
Q     77
C    168
S    644
```
#### 4. value_counts() displaying the NaN values
By default, the count of null values is excluded from the result. But, the same can be displayed easily by setting the dropna parameter to False .
```html
train['Embarked'].value_counts(dropna=False)
-------------------------------------------------------------------
S      644
C      168
Q       77
NaN      2
```
We can easily see that there are two null values in the column.

#### 5. value_counts() to bin continuous data into discrete intervals
This is one of my favorite uses of the value_counts() function and an underutilized one too. value_counts() can be used to bin continuous data into discrete intervals with the help of the bin parameter. This option works only with numerical data. It is similar to the pd.cut function. Let’s see how it works using the Fare column.
```html
applying value_counts on a numerical column without the bin parameter
train['Fare'].value_counts()
```
![vc_nobin](https://user-images.githubusercontent.com/61290493/83197984-08ee5680-a104-11ea-835d-bcc674f2ff29.png)

This doesn’t convey much information as the output contains a lot of categories for every value of Fare. Instead, let’s group them into seven bins.
```html
train['Fare'].value_counts(bins=7)
```
![vc_bin](https://user-images.githubusercontent.com/61290493/83197914-eb20f180-a103-11ea-936f-dc78fe3f5000.png)

Binning makes it easy to understand the idea being conveyed. We can easily see that most of the people out of the total population paid less than 73.19 for their ticket. Also, we can see that having five bins serves our purpose since no passenger falls into the last two bins
