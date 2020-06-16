# Tuning XGBoost (XGB模型调参）

Original:https://towardsdatascience.com/fine-tuning-xgboost-in-python-like-a-boss-b4543ed8b1e

XGBoost（或eXtreme Gradient Boosting）不再介绍，仅在太多的数据科学竞赛中被证明是相关的，如果您只是开始使用它，它仍然是一种难以微调的模型。

为什么要微调键？ 因为如果您有大型数据集，并且您对5个不同的参数进行了朴素的网格搜索，并且每个参数都有5个可能的值，那么您将有5次= 3,125次迭代。 如果一次迭代需要10分钟才能运行，那么在获取参数之前，您将有21天以上的等待时间（我不谈论Python崩溃，没有告诉您，并且您等待了很长时间才意识到）。

我在这里假设您首先正确完成了功能工程。 特别是对于分类功能，因为XGBoost在输入中不包含分类功能。

## 1.训练测试集划分，评估指标和提前停止

在进行参数优化之前，首先要花一些时间来设计模型的诊断框架。
XGBoost Python api提供了一种通过增加的树数来评估增加的性能的方法。 它使用两个参数：“ eval_set”（通常是训练和测试集），以及关联的“ eval_metric”来衡量这些评估集上的错误。
```html
eval_set = [(X_train, y_train), (X_test, y_test)]
eval_metric = ["auc","error"]
%time model.fit(X_train, y_train, eval_metric=eval_metric, eval_set=eval_set, verbose=True)
```

结果如下：




