## Types of Learning
**Supervised learning**: training a model on labeled pairs of input and output data to learn how to predict the output from the input

**Unsupervised learning**: using unlabeled data to find patterns or clusters without having predefined labels

**Semi-supervised learning**: training a model using partially labeled data, where the unlabeled data can either have labels inferred or inform the model about clusters, decision boundaries, or other properties

**Online learning**: training a model as data arrives rather than all at once, efficiently using space and allowing the model to adapt to changes over tome

**Federated learning**: a distributed approach to training models where models are trained separately on different devices or datasets before being pooled

**Reinforcement learning**: training a model that acts as an agent and is rewarded when working properly towards a goal and punishment when not

**Self-supervised learning**: learning where the model generates its own labels from artificially constructed data

**Transfer learning**: adapting a model made for one task to a new task without additional training

**Fine-tuning**: a type of transfer learning involving adapting a model that was previously trained on one dataset for a new purpose, training it on a smaller subset of domain-specific data, reducing the amount of time and data required to build a model

## Decision Trees
Decision trees segregate data based on rules until a stopping condition is met, usually that subgroups are pure or a maximum depth is reached. Trees can predict categorical data (classification trees) and numerical data (regression trees). Advantages include fast predictions, intelligibility, and low compute requirements. Disadvantages include being prone to overfitting and instability (i.e., adding more data can cause the entire tree to be reconstructed).

Branches are decided based on splitting criteria. Examples of splitting criteria include:

**Gini impurity**: measure how frequently a randomly chosen entry would be mislabeled based on the branch rule

**Information gain**: compared the entropy before and after splitting to see how much information is gained after the split

**Mean squared error** (regression trees): rules are chosen minimize variance in predicted values

Trees can be “regularized” via pruning to prevent overfitting. This can happen in two ways:

**Pre-pruning** (early stopping): preventing new branches from being formed after a certain threshold or depth

**Post-pruning** (cost-complexity pruning): post-box removal of branches that don’t produce any gains in validation data

### Random Forests
Random forests solve a single decision tree’s problem with overfitting by producing an ensemble of decision trees. Decision trees are fit for random subsets of the data, then averaged (regression) or vote on the outcome (classification) during prediction. The process of averaging trees that are trained in parallel is called “bagging”.

### Boosted Trees
Boosted trees compensate for the weakness of an initial tree by building sequential trees to correct errors in previous trees. The process of averaging trees that are trained sequentially is called boosting. Some key algorithms related to boosting: 

**AdaBoost**: increases the weight of incorrectly predicted data in subsequent trees

**Gradient boosting**: a specific type of boosting where a differentiable loss function is used to fit sequential models on the residuals of the previous model

**XGBoost**: a popular library implementing gradient-boosted decision trees that is optimized for efficiency

