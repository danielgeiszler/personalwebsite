
## Decision Trees
Decision trees segregate data based on rules until a stopping condition is met, usually that subgroups are pure or a maximum depth is reached. Trees can predict categorical data (classification trees) and numerical data (regression trees). Advantages include fast predictions, intelligibility, and low compute requirements. Disadvantages include being prone to overfitting and instability (i.e., adding more data can cause the entire tree to be reconstructed).

Branches are decided based on splitting criteria. Examples of splitting criteria include:
1. **Gini impurity**: measure how frequently a randomly chosen entry would be mislabeled based on the branch rule
2. **Information gain**: compared the entropy before and after splitting to see how much information is gained after the split
3. **Mean squared error** (regression trees): rules are chosen minimize variance in predicted values

Trees can be “regularized” via pruning to prevent overfitting. This can happen in two ways:
1. **Pre-pruning** (early stopping): preventing new branches from being formed after a certain threshold or depth
2. **Post-pruning** (cost-complexity pruning): post-box removal of branches that don’t produce any gains in validation data
### Random Forests
Random forests solve a single decision tree’s problem with overfitting by producing an ensemble of decision trees. Decision trees are fit for random subsets of the data, then averaged (regression) or vote on the outcome (classification) during prediction. The process of averaging trees that are trained in parallel is called “bagging”.

### Boosted Trees
Boosted trees compensate for the weakness of an initial tree by building sequential trees to correct errors in previous trees. The process of averaging trees that are trained sequentially is called boosting. Some key algorithms related to boosting: 
1. **AdaBoost**: increases the weight of incorrectly predicted data in subsequent trees
2. **Gradient boosting**: a specific type of boosting where a differentiable loss function is used to fit sequential models on the residuals of the previous model
3. **XGBoost**: a popular library implementing gradient-boosted decision trees that is optimized for efficiency

