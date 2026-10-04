# Lecture 9 - Gradient Descent II

How to know a single variable smooth function convex or not (here is a typo: a smooth "single variable"...):
![single variable convex](./static/09-single_variable_convex.png)
Multi variables:
![multi variable](./static/09-multi_variable_smooth.png)

![convex function](./static/09-convex_function.png)

## Smoothness

In this part, we will forget about convexity. Let's focus on "smooth". We will quantify smoothness: L-smooth

### Smooth descent

Why care about L-smooth? -> if it is L-smooth, and we choose a step: $η_t < 1/L$, then it must decrease in each step! It is called **smooth descent**:

$f(w^{(t+1)}) \leq f(w^{(t)}) - \frac{\eta}{2}||\nabla f(w^{(t)})||^2$
![L-smooth](./static/09-l_smooth.png)

You can try to prove it. Here we just use this conclusion and Lemma to prove a convergence theory:

1. [Prove Lemma](#prove-lemma), this part only uses L-smooth, it don't need convexity
2. [Prove it](#prove-convergence), this part combine Lemma and convexity to prove this convergence statement.

### Visualize L-smooth

For every point w, if the whole $\nabla f(w)$ lies in the cone (the region between these 2 lines: slope = L and slope = -L), then L is satisfied.
![example L-smooth](./static/09-example_l_smooth.png)

If we choose a too small L (let's write it as "M"), then it won't satisfy.
![example not L-smooth](./static/09-example_not_l_smooth.png)

> L is just a constant, and we need to find the L (for some functions, L doesn't exist)\
> Small L means the function is smooth, so you can choose a greater step.

### Lemma

You can try to prove it (or read the [following part](#prove-lemma)): if $η_t < 1/L$, then we can make progress every step (it is called *Lemma*).
![Lemma](./static/09-lemma.png)
> "Lemma" is just the name of this statement

Lemma means, the whole function lies between the tangent line $\pm$ a quadratic function $\frac{L}{2} ||w' - w||^2$

![visualize Lemma](./static/09-visualize_lemma.png)

### Prove Lemma

To prove it, we just need to consider a 1-D version:
![proof: goal](./static/09-proof_lemma.png)

![proof 1](./static/09-proof_lemma_1.png)
Why last step is possible

1. $\nabla f(w) \cdot d$ is irrelevant to t
2. integral is linear operation
3. $a \cdot c + b \cdot c = (a + b) \cdot c$

![proof 2](./static/09-proof_lemma_2.png)
Why second line is true: it is Cauchy–Schwarz inequality, or more intuitively, $||a \cdot b|| = ||a|| \times ||b|| \times \cos(θ) <= ||a|| \times ||b||$; Third line is definition of L-smooth; Forth line is just simple math.

Combine them together:
![proof 3](./static/09-proof_lemma_3.png)

Proof done.

## Convergence

> Why we care about convexity? Because a random function may stuck in a local minimum (when we do gradient descent). But for convex function, local minimum must also be a global minimum.

If we combine L-smooth and convex, then we know how fast it converge (will prove it later):

![convergence](./static/09-convergence.png)

> [!Note]
> Notice it shows how many steps are needed to converge in the worst case. And it depends on your initial guess: $||w^0 - w^*||^2$

### Prove convergence

#### Step 1

Step 1: combining convexity and [smooth descent](#smooth-descent), we get formula 1:\
$f(w^{(t+1)}) - f(w^*) \leq \nabla f(w^{(t)}) \cdot (w^{(t)} - w^*) - \frac{\eta}{2}||\nabla f(w^{(t)})||$

![proof 1](./static/09-proof_convergence_1.png)

#### Step 2

Step 2: using the definition of gradient descent updates, we get formula 2:\
$||w^{(t)} - w^*||^2 - ||w^{(t+1)} - w^*||^2 = 2 \eta \nabla f(w^{(t)}) \cdot (w^{(t)} - w^*) - \eta^2||\nabla f(w^{(t)})||^2$

![proof 2](./static/09-proof_convergence_2.png)

#### Step 3

Combining the formula 1 and 2, we get formula 3:

![proof 3](./static/09-proof_convergence_3.png)
> [!Note]
> In the last line dropped a term $\frac{||w^{(t)} - w^*||}{2\eta}$, because dropping it doesn't influence $\leq$.

#### Step 4

Lastly, combining formula 3 and [smooth descent](#smooth-descent): we get the conclusion:

![proof 4](./static/09-proof_convergence_4.png)

### Conclusion

It tells us that if we have a **L-smooth** **convex** function, after T steps, $f(w^{(T)})$ can converge to $f(w^*) + O(1/T)$. (It cannot be less than $f(w^*)$, because, by definition, $w^*$ is a minima).

## Nesterov Momentum

Can we make it faster? -> Yes, with **Nesterov Momentum**:
![Nesterov Momentum](./static/09-nesterov_momentum.png)

The $y^t$ line contains the information about the gradient descent of last time, which is called momentum. (you can regard $y^{(t)}$ as $w^{(t+0.5)}$)

![gradient descent vs momentum](./static/09-gradient_descent_vs_momentum.png)

**Gradient descent: O(1/T)**\
**Nesterov Momentum: O(1/T^2)**\
This is a huge improvement!
![convergence of momentum](./static/09-momentum_convergence.png)

## AdaGrad + momentum = Adam

By combining AdaGrad and momentum algorithm, we can get **Adam algorithm**!
![Adam algorithm](./static/09-adam_algorithm.png)

> I guess Adam means ADAptive Momentum.
