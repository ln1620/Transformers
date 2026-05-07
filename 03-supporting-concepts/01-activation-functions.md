# 1. Activation Functions

## Definition

An **activation function** introduces **non-linearity** into a neural network, enabling it to learn and model complex patterns. Without it, no matter how many layers you stack, the entire network would behave like a single linear transformation.

In one line: *activation functions are what make deep networks "deep".*

---

## Why non-linearity matters

```
no activation:   y = W2 . W1 . x  =  (W2 . W1) . x  =  W' . x   <- still linear
with activation: y = W2 . f(W1 . x)                              <- non-linear, can model anything
```

Real-world data is rarely linear. Activation functions let the model bend, fold, and reshape its decision surface.

---

## The three classic activations

### Sigmoid

```
sigmoid(x) = 1 / (1 + e^(-x))
```

- Range: (0, 1)
- Useful for binary classification output (probability).
- Suffers from **vanishing gradients** for large `|x|`.

```
shape:      1 ┤      ____________
              │     /
            0 ┤____/
              └──────────────►  x
```

### Tanh

```
tanh(x) = (e^x - e^(-x)) / (e^x + e^(-x))
```

- Range: (-1, 1)
- Zero-centered (better than sigmoid for hidden layers).
- Still suffers from vanishing gradients for large `|x|`.

```
shape:      1 ┤      ____________
              │     /
            0 ┤    /
              │   /
           -1 ┤___/
              └──────────────►  x
```

### ReLU - Rectified Linear Unit

```
ReLU(x) = max(0, x)
```

- Range: [0, ∞)
- Cheap to compute.
- Doesn't saturate for positive `x`.
- Default choice for hidden layers in deep networks.
- Watch out for the "dying ReLU" problem (neurons stuck at 0).

```
shape:        ┤             /
              │            /
              │           /
            0 ┤__________/
              └──────────────►  x
```

---

## Quick comparison

| function | range          | zero-centered | gradient saturation | typical use                      |
|----------|----------------|:-------------:|:-------------------:|----------------------------------|
| Sigmoid  | (0, 1)         |       no      |        yes          | binary output layer              |
| Tanh     | (-1, 1)        |      yes      |        yes          | RNN hidden layers (older)        |
| ReLU     | [0, ∞)         |       no      |   no (positive)     | default for hidden layers        |

---

## Modern variants used in Transformers

- **GELU** - smooth approximation of ReLU. Used in BERT and GPT.
- **SwiGLU** - gated variant. Used in LLaMA and many recent LLMs.
- **Leaky ReLU** - small slope for negative inputs to avoid dying ReLU.

---

## Key takeaways

- Activation functions add **non-linearity**.
- **ReLU** is the modern default for hidden layers.
- **Sigmoid** and **tanh** are limited by vanishing gradients.
- Modern Transformers commonly use **GELU** or **SwiGLU**.

---

| Section README | Next -&gt; |
|---|---|
| [03-supporting-concepts](./) | [Gradient Descent](02-gradient-descent.md) |

[Back to root README](../README.md)
