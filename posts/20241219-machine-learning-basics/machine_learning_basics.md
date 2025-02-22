This is a running list of machine learning architectures, terms, and concepts. I write them down here for quick reference in the future. If you see an issue, please let me know!

## Types of Learning

**Supervised learning**: training a model on labeled pairs of input and output data to learn how to predict the output from the input

**Unsupervised learning**: using unlabeled data to find patterns or clusters without having predefined labels

**Semi-supervised learning**: training a model using partially labeled data, where the unlabeled data can either have labels inferred or inform the model about clusters, decision boundaries, or other properties

**Online learning**: training a model as data arrives rather than all at once, efficiently using space and allowing the model to adapt to changes over tome

**Federated learning**: a distributed approach to training models where models are trained separately on different devices or datasets before being pooled

**Reinforcement learning**: training a model that acts as an agent and is rewarded when working properly towards a goal and punishment when not

**Self-supervised learning**: learning where the model generates its own labels from artificially constructed data

**Transfer learning**: adapting a model made for one task to a new task

**Fine-tuning**: a type of transfer learning involving adapting a model that was previously trained on one dataset of general data for a new purpose, training it on a smaller subset of domain-specific data, reducing the amount of time and data required to build a model

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

## Regularization
Regularization is the process by which we penalize model complexity, done to reduce overfitting and make models more generalizable. This can be split into two broad categories, explicit regularization—where a regularizing term, be it a penalty, prior, or constraint, is added to directly the optimization problem, and implicit regularization, which broadly encompasses other methods of preventing overfitting.

These are some of the most commonly types of explicit regularization:

**Lasso** (L1 regularization): adds a penalty proportional to the absolute values of the model parameters, making some parameters go to zero (get dropped out) during optimization. It can be used to perform feature selection if you suspect there are irrelevant variables.

**Ridge regression** (L2 regularization): adds a penalty proportional to the sum of squares of the model parameters, shrinking their coefficients to zero but generally without dropping them out of the optimization altogether. It can be used to reduce the weights of highly correlated variables in tandem, whereas L1 regularization might select a single feature and drop the others. It is generally more stable than L1 regularization because it doesn’t remove parameters.

**Elastic Net**: combines L1 and L2 regularization, simultaneously shrinking coefficients while performing feature selection. The relative weight of the L1 and L2 contributions to regularization are controlled by the $\alpha$ parameter.


## Deep Learning
Deep learning uses multi-layer neural networks to learn representations from data. Each layer is made up of neurons which that compute a weighted sum of inputs and then apply an activation function. Differences in deep learning modalities can arise from differences in activation functions, connectivity, or learning objective.

Deep learning relies on multi-layer (deep) neural networks to learn hierarchical representations from data. Each layer consists of neuron-like units that compute a weighted sum of inputs and then apply a nonlinear activation function. Different deep learning “modalities” emerge from variations in network connectivity (e.g., convolutional layers vs. recurrent layers) and activation functions, as well as other factors like the nature of the input data or the learning objective.

These are some of the most common activation functions: 

**ReLU** (Rectified Linear Unit): calculates max(0, x), which is simple, fast, and generally achieves good results. However, it can produce “dead” neurons if inputs are negative.

**Sigmoid**: outputs on range (0, 1) and is commonly used for binary outputs. Suffers from the vanishing gradient problem at large absolute inputs.

**Softmax**: sigmoid but for multi-class data

**Tanh**: outputs on range (-1, 1) rather than (0,1). Still suffers from the vanishing gradient problem at large absolute inputs.

**Leaky ReLU**: returns x if x > 0, else returns $\alpha$x where $\alpha$ is manually tunable. This change mitigates the dead neuron issue.

These are some common types of layers:

**Dense**: connects all neurons in the previous layer to all neurons in current layer. They are used in feed forward networks or as the final layer in classification, but in the latter case can be replaced by global average pooling and softmax.

**Pooling layer**: reduces dimensions by summarization (e.g., taking the max or average value)

**Convolutional layer**: performs a convolution between the input and a kernel (weight matrix which is learned during training). 1D convolutions can be used for sequences, 2D for images, and 3D for spatial data or videos. One variant uses dilated kernels, which has gaps between kernel elements to widen its receptive field.

**Recurrent layer**: process sequential data by maintaining a hidden state across steps

