# Bias-Variance Tradeoff (偏差与方差）

In statistics and machine learning, the bias–variance tradeoff is the property of a set of predictive models whereby models with a lower bias in parameter estimation have a higher variance of the parameter estimates across samples, and vice versa. The bias–variance dilemma or bias–variance problem is the conflict in trying to simultaneously minimize these two sources of error that prevent supervised learning algorithms from generalizing beyond their training set.

在统计和机器学习中，偏差－方差折衷是一组预测模型的属性，因此参数估计中偏差较小的模型在样本之间的参数估计方差较大，反之亦然。偏差-方差困境或偏差-方差问题是试图同时最小化这两个误差源的冲突，这会阻止监督学习算法推广到其训练范围之外

## What is Bias?
Bias is the difference between the average prediction of our model and the correct value which we are trying to predict. Model with high bias pays very little attention to the training data and oversimplifies the model. It always leads to high error on training and test data.(UnderFitting)

偏差是我们模型的平均预测与我们要预测的正确值之间的差。 具有高偏差的模型很少关注训练数据，从而简化了模型。高偏差的模型会错误估计特征与目标值之间的相关关系， 导致训练集和测试集的数据的错误率很高。（欠拟合）

## What is Variance?
Variance is the variability of model prediction for a given data point or a value which tells us spread of our data. Model with high variance pays a lot of attention to training data and does not generalize on the data which it hasn’t seen before. As a result, such models perform very well on training data but has high error rates on test data.(OverFitting)

方差描述的是预测值的变化范围，离散程度，也就是离其期望值的距离。方差越大，数据的分布越分散。 具有高方差的模型对训练数据中的随机噪声进行建模，泛化能力不足。 结果，这样的模型在训练数据上表现很好，但是在测试数据上有很高的错误率。（过拟合）

## How to Balance

The bias–variance decomposition is a way of analyzing a learning algorithm's expected generalization error with respect to a particular problem as a sum of three terms, the bias, variance, and a quantity called the irreducible error, resulting from noise in the problem itself.

This tradeoff applies to all forms of supervised learning: classification, regression (function fitting),and structured output learning, though, it does not apply in all learning algorithms.[5][6] It has also been invoked to explain the effectiveness of heuristics in human learning.

It is important to note that the bias-variance tradeoff is not universal. For example, both bias and variance decrease when increasing the width of a neural network.[8] This means that it is not necessary to control the size of a neural network to control variance. This does not contradict the bias-variance decomposition because the bias-variance decomposition does not imply a bias-variance tradeoff.
