# Lecture 4 - Perceptrons

How to improve a classifier? (training)

![negative](./static/04-negative_misclassified.png)
![positive](./static/04-positive_misclassified.png)
![overall](./static/04-overall.png)

## Perception

Based on the algorithm above, we can design perception:
![perception](./static/04-perception.png)

Here is an example of perception:
![example perception](./static/04-perception_example.png)

### Changing data order

What if we start with another point? -> It may make the convergence faster or slower. And it may lead to a different classification.
![changing order](./static/04-changing_data_order.png)

### Margin

![margin](./static/04-margin.png)

Q: What if the data is NOT separable? May go into infinite loop.

So, ***perception assumes that the data it handles must be separable.*** (Keep this in your mind! Otherwise, the following proof cannot hold)

### How fast to converge

How to proof will be covered in next section
![convergence rate](./static/04-convergence_rate.png)

### Prove it

Assumptions:

1. Data is separable
2. $w^*$ is normalized

![proof outline](./static/04-proof_outline.png)

- Step 1: proof $<w^*, w^{(t)}>$ grows in each iteration (i.e. angle between $w^*$ and $w^{(t)}$ becomes smaller and smaller). ***We assume that $w^*$ is normalized.***
- Step 2: We have to control that $||w^{i}||$ won't be too large (otherwise, $<w^*, w^{(t)}>$ can be only because by increasing the length of $w^{(t)}$, rather than decrease the angle!)
- Sept 3: Combine and draw the conclusion.

### Step 1

![step 1](static/04-proof_step_1.png)
![step 1](static/04-proof_step_1_1.png)

### Step 2

![step 2](./static/04-proof_step_2.png)
Why in 3 to 4, the middle term is 0?  
-> Because $(x_i, y_i)$ is a misclassified data, then $y_i(w^{(t-1)} * x_i)$ must be less than 0. So, we can replace it with 0 in the `<=` case.

![step 2](./static/04-step_2_2.png)

### Step 3

Combine Step 1 and 2:

![step 3](./static/04-step_3.png)

Q: what if we perform one more step after $R^2 / γ^2$?  
->There will be no misclassified data after $R^2 / γ^2$. So it won't renew $w^{(t)}$

### Remarks on convergence

Notice that R and γ are just geometry properties of data set. So, you can conclude how fast perception converge just by looking at data set!
![remarks](./static/04-remarks.png)
