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

![xgb-result](https://user-images.githubusercontent.com/61290493/84827486-28cdb780-afea-11ea-963c-b7d94a34d55d.png)

在分类误差图上：直到350次迭代，我们的模型似乎学习了很多东西，然后误差才非常缓慢地减小。 这反映在测试集上，当迭代次数从350增加时，我们不一定会看到性能。有了这一点，您已经可以考虑砍掉350棵树，并节省以后的参数调整时间。

如果您不使用scikit-learn api，而是使用纯XGBoost Python api，那么可以使用Early Stop参数，该参数可以帮助您自动减少树的数量。

## 2.开始微调模型
```html
model = XGBClassifier(silent=False, 
                      scale_pos_weight=1,
                      learning_rate=0.01,  
                      colsample_bytree = 0.4,
                      subsample = 0.8,
                      objective='binary:logistic', 
                      n_estimators=1000, 
                      reg_alpha = 0.3,
                      max_depth=4, 
                      gamma=10)
```

如果您还没有运行任何模型，从哪里开始呢？

#### 2.1 为关键输入填写合理的值：
```html
learning_rate: 0.01
n_estimators: 100 if the size of your data is high, 1000 is if it is medium-low
max_depth: 3
subsample: 0.8
colsample_bytree: 1
gamma: 1
```

#### 2.2 
运行model.fit（eval_set，eval_metric）并诊断您的第一次运行，特别是n_estimators参数

#### 2.3 
优化max_depth参数。 它表示每棵树的深度，这是每棵树中使用的不同特征的最大数量。 我建议从较低的max_depth（例如3）开始，然后将其递增1，并在无法提高其性能的情况下停止。 这将有助于简化模型并避免过度拟合

#### 2.4
现在尝试学习率和避免过度拟合的功能：

learning_rate：通常在0.1到0.01之间。 如果您专注于性能并且有很多时间在前面，那么在增加树木数量的同时逐步降低学习率。

subsample：这是每棵树的百分比，用于构建树。 我建议不要占用太多行，因为性能会下降很多。 取0.8到1。

colsample_bytree：每棵树使用的列数。 为了避免某些色谱柱在预测中花费过多的精力（例如，在推荐系统中，当您推荐购买量最大的产品时，会忘记长尾巴），因此，请选择较大比例的色谱柱。 如果您有很多列（尤其是进行一站式编码），则值为0.3到0.8；如果只有几列，则值为0.8到1。

gamma：通常被误解的参数，它充当正则化参数。 0、1或5。
