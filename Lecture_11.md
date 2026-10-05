# Lecture 11 - Model Selection

Hyper-parameters, validation, and how far a validation score can be trusted. [Slides](https://karthik-sridharan.github.io/3780fa26/Model_selection#1).

## Training lifecycle

![training lifecycle 1](./static/11-training_lifecycle_1.png)
> i.i.d. independent and identically distributed

![training lifecycle 2](./static/11-training_lifecycle_2.png)
![training lifecycle 3](./static/11-training_lifecycle_3.png)
![training lifecycle 4](./static/11-training_lifecycle_4.png)

We should set test set aside (it should never be seen by model) because we want to make sure test error is a good estimation of deployment error. -> **Test is only used once, to report error.** (The error of your model is represented by **test error**).

## Expected/deployment error

![test error as expected error](./static/11-test_error_as_expected_error.png)
We can use test error to represent real error, but why?

Please notice that on the slide, the error is represented as average misclassification rate, but in more general case, it is **empirical loss on the training set (without regularization)**:

- $\epsilon(h)$: exception of error = $\epsilon(h) = E[\ell(h(x), y)]$.
- $\epsilon_{te}(h)$: test error = $\epsilon_{te}(h) = \frac{1}{n_{test}} \sum_{i=0}^{n_{test}} \ell(h(x_i), y_i)$

Notice: **error doesn't include regularization.**

### Hoeffding Bound

[Hoeffding bound](https://karthik-sridharan.github.io/3780fa26/Model_selection#4) tells us how good test error can estimate deployment error:
![Hoeffding bound](./static/11-hoeffding_bound.png)

If your test set is large enough: n_test is large, then there is a very small chance that $​|\epsilon(h)−\epsilon_{te}(h) > \gamma|$.

### Failure probability

Let's assume $1 - \delta$ is the probability that test error is a good estimation of expected error, then $\delta$ is the probability that it is a bad estimation. We can call this $\delta$ as "failure probability".

When $n_{test}$ is small: the deviation (crossing point of line $y = 2e^{2γ^2n_{test}}$ and line $y = \delta$) is large
![small n](./static/11-hoeffding_bound_small_n.png)
When $n_{test}$ is large: the deviation (or "slack") is small.
![large n](./static/11-hoeffding_bound_large_n.png)

## How to choose hyperparameters

![how to choose hyperparameters](./static/11-how_to_choose_hyperparameters.png)

### Quiz

![quiz](./static/11-quiz.png)

1. This is a bad idea, it is only trained on training set and you are choosing hyperparameter based on training error, you will always choose a super complex overfitted model (training error is close to 0) with $\lambda = 0$ (no penalty to model complexity).
2. When we increase p, we have more and more features, then we can make training error 0. But under such cases, the model is usually overfitted:
![overfitting](./static/11-overfitting.png)

### Correct way to choose hyperparameters

This is how you should do to decide hyperparameters:
![choose hyperparameters](./static/11-choose_hyperparameters.png)

### Why it works

Why we can use validation error to choose hyperparameter (why we think validation error is a good estimation of expected error) -> Because of Hoeffding bound:
![why use validation error for choosing hyperparameters](./static/11-why_use_validation_error_for_choosing_hyperparameters.png)

Prove it -> **union bound** (i.e. $P(A \cup B) <= P(A) + P(B)$):
![why use validation error for choosing hyperparameters 2](./static/11-why_use_validation_error_for_choosing_hyperparameters_2.png)
Here, $P(A_i)$ means the probability that the validation error of hyperparameter set i is far from expected error (i.e. the probability of $|\epsilon(h_{\lambda_i}) - \hat{\epsilon}_{val}(h_{\lambda_i})| > \gamma$)\
So, $P(A_1 \cup A_2 \dots \cup A_k)$ means: the probability that **any** one of validation error is far from expected error.

#### Step 1: rewrite union bound

![proof step 1](./static/11-proof_1.png)
> Here hat ($\hat{\ }$) means “estimated from observed data.”

#### Step 2: use $\delta$ to represent the upper bound of probability

![proof step 2](./static/11-proof_2.png)
Remember: $\delta$ is the [failure probability](#failure-probability), and $1 - \delta$ is the lower bound of probability that all validation errors are good estimations.

> log is ln

i.e. P(all validation errors are good estimations) $\ge 1 - \delta$, or P(not all validation errors are good estimation) $\le \delta = 2ke^{-2\gamma^2n_{val}}$

#### Step 3: after some magic steps

$\widehat{\lambda}$ is the $\lambda$ that minimizes the validation error, which is also the set of hyperparameters we will choose.
![proof step 3](./static/11-proof_3.png)

A important conclusion of it is: **there is almost no cost of adding more sets of hyperparameters**

For example, if you choose a $\delta = 0.05$. And you add hyperparameters sets from 10 to 100,000, then the slack you gain is only $\frac{\sqrt{\ln(2 * 100,000 / 0.05)}}{\sqrt{\ln(2 * 10 / 0.05)}} = 1.59$, so the slack only gains 59%, but you can test 10000x more hyperparameters!

## You cannot use validation set as test set

![validation set cannot be used as test set](./static/11-validation_set_cannot_as_test_set.png)

## Cross validation

When we don't have enough data, cross validation is very useful:
![why cross validation](./static/11-why_cross_validation.png)

It can also be use to get *a more stable estimate of model or hyperparameter performance*

So, how to do a cross-validation? -> Read next section:

### K-fold cross-validation

If I have m sets of hyperparameters ($h_1, \dots, h_m$), then I train k models for each set. And we get k\*m models with k\*m validation errors. We compare the **average validation error** of each set, choose a hyperparameter set with highest average validation error.

> [!important]
> For each set, we need to train k models (k sets of w and b), and calculate k validation errors, then average them.\
> For each set, we have a averaged validation error, we choose the set of lowest validation error\
> Then, to get the w and b, we do **NOT** average w and b from k-fold validation.\
> But instead, we **retrain a new model from scratch using the selected hyperparameters on the full training + validation data**.

![k-fold cross-validation](./static/11-k_fold_cross_validation.png)

## Search hyperparameter

- Random search: randomly choose hyperparameters in the hyperparameter space
- Grid search: choose hyperparameters with fixed steps.

Counterintuitively, a research shows that random search is always better than grid search: [demo](https://karthik-sridharan.github.io/3780fa26/Model_selection#16):
![searching hyperparameter space](./static/11-searching_hyperparameter_space.png)

Because in real-world machine learning, only a small subset of hyperparameters are important.

## Early stopping

![early stopping](./static/11-early_stopping.png)
When you stop in the middle of this graph, it is called early stopping. Early stopping can also be regarded as a regularizer (regularizer is used to prevent overfitting).

## Summary

![summary](./static/11-summary.png)
