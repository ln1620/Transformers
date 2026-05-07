# 2. Gradient Descent

## Definition

**Gradient Descent (GD)** is an optimization algorithm used to minimize the loss function of a model by **iteratively updating the weights in the direction of the negative gradient**.

In one line: *take a small step downhill, repeat until you reach the bottom.*

---

## Intuition

Imagine you are standing on a foggy mountain and want to reach the lowest point. You cannot see the bottom, but you can feel which way is steepest down. You take a small step in that direction, then check again, then step again. Eventually, you settle at a valley (a minimum of the loss).

```
loss
  ^
  |   *
  |    \
  |     \
  |      \  <- step in direction of -gradient
  |       \
  |        *
  |         \
  |          *.
  |           '*--*--*    <- minimum
  +---------------------------> weights
```

The size of each step is the **learning rate**.

---

## The update rule

For a parameter `w` and loss `L`:

```
w_new = w_old - learning_rate * dL/dw
```

- `dL/dw` is the gradient (slope) of the loss with respect to the weight.
- The minus sign means we move **opposite** to the gradient.
- The learning rate decides how big each step is.

---

## The three variants

| Variant         | Updates after          | Pros                          | Cons                                |
|-----------------|------------------------|-------------------------------|-------------------------------------|
| **Batch GD**    | full dataset           | stable, smooth convergence    | slow on large datasets, memory-heavy|
| **Stochastic GD (SGD)** | each training example   | fast, can escape shallow minima | very noisy, oscillates           |
| **Mini-batch GD** | a small batch (e.g. 32, 64, 256) | balanced - fast and stable    | requires picking batch size      |

### Batch Gradient Descent

Updates weights after computing the gradient over the **entire** dataset.

```
for epoch in epochs:
    grad = compute_gradient(all_data)
    w = w - lr * grad
```

### Stochastic Gradient Descent (SGD)

Updates weights for **each training example** individually.

```
for epoch in epochs:
    for x, y in shuffle(all_data):
        grad = compute_gradient(x, y)
        w = w - lr * grad
```

### Mini-batch Gradient Descent

Updates weights after computing the gradient over a **small batch** of training examples.

```
for epoch in epochs:
    for batch in get_mini_batches(all_data, batch_size=32):
        grad = compute_gradient(batch)
        w = w - lr * grad
```

This is what almost all modern deep learning uses.

---

## In Transformers

Training a Transformer is just gradient descent on hundreds of billions of parameters. Real systems use enhanced optimizers built on mini-batch SGD:

- **Adam / AdamW** - SGD with adaptive learning rates per parameter (uses momentum and variance estimates).
- **Learning rate schedules** - warm-up phase + decay (the original "Attention Is All You Need" paper uses a warmup-then-inverse-square-root schedule).

---

## Key takeaways

- Gradient descent finds the minimum of a loss function by stepping downhill.
- Three variants differ in **how many examples** are used per update.
- **Mini-batch GD** is the practical default.
- Modern training uses **Adam / AdamW** with a learning-rate schedule.

---

| &lt;- Previous | Section README | Next -&gt; |
|---|---|---|
| [Activation Functions](01-activation-functions.md) | [03-supporting-concepts](./) | [Evaluation Metrics](03-evaluation-metrics.md) |

[Back to root README](../README.md)
