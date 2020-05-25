# Bias-Variance Tradeoff(偏差与方差）

In statistics and machine learning, the bias–variance tradeoff is the property of a set of predictive models whereby models with a lower bias in parameter estimation have a higher variance of the parameter estimates across samples, and vice versa. The bias–variance dilemma or bias–variance problem is the conflict in trying to simultaneously minimize these two sources of error that prevent supervised learning algorithms from generalizing beyond their training set.

在统计和机器学习中，偏差－方差折衷是一组预测模型的属性，因此参数估计中偏差较小的模型在样本之间的参数估计方差较大，反之亦然。偏差-方差困境或偏差-方差问题是试图同时最小化这两个误差源的冲突，这会阻止监督学习算法推广到其训练范围之外

* The bias error is an error from erroneous assumptions in the learning algorithm. High bias can cause an algorithm to miss the relevant relations between features and target outputs (underfitting).

偏差：描述的是预测值的期望与真实值之间的差距。偏差越大，越偏离真实数据，如下图第二行所示。

偏差误差是来自学习算法中的错误假设的误差。高偏差可能会导致算法错误估计特征与目标输出之间的相关关系（欠拟合）。

* The variance is an error from sensitivity to small fluctuations in the training set. High variance can cause an algorithm to model the random noise in the training data, rather than the intended outputs (overfitting).

方差：描述的是预测值的变化范围，离散程度，也就是离其期望值的距离。方差越大，数据的分布越分散，如下图右列所示。

方差是由于训练集中数据集波动导致的敏感性产生的误差。高方差可能导致算法对训练数据中的随机噪声进行建模，而不是对预期的输出进行建模（过度拟合）。



The bias–variance decomposition is a way of analyzing a learning algorithm's expected generalization error with respect to a particular problem as a sum of three terms, the bias, variance, and a quantity called the irreducible error, resulting from noise in the problem itself.

This tradeoff applies to all forms of supervised learning: classification, regression (function fitting),and structured output learning, though, it does not apply in all learning algorithms.[5][6] It has also been invoked to explain the effectiveness of heuristics in human learning.

It is important to note that the bias-variance tradeoff is not universal. For example, both bias and variance decrease when increasing the width of a neural network.[8] This means that it is not necessary to control the size of a neural network to control variance. This does not contradict the bias-variance decomposition because the bias-variance decomposition does not imply a bias-variance tradeoff.
