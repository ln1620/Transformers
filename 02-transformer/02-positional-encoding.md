# 2. Positional Encoding

## Definition

**Positional Encoding (PE)** is a vector added to each word's embedding to inject information about **where** the word appears in the sequence.

In one line: *embeddings give meaning, positional encodings give order.*

---

## Why we need it

A Transformer processes all words in the sentence **simultaneously** (in parallel). That makes it fast - but it also means the model loses the natural left-to-right order that an RNN would have.

```
"Dog bites man"  vs  "Man bites dog"
```

Without positional information, both look the same to a Transformer (same words, same embeddings). PE fixes this.

---

## How it is added

Each input vector that the Transformer sees is the sum of:

1. **Embedding** - meaning of the word.
2. **Positional encoding** - position of the word.

```
final_input[i] = embedding[i] + position_encoding[i]
```

---

## Worked example

Sentence: `"The cat sat on the mat"`.

```
Tokens     : [the, cat, sat, on, the, mat]
Embeddings : [E1,  E2,  E3,  E4, E5,  E6 ]

After positional encoding:
Final input: [E1+P1, E2+P2, E3+P3, E4+P4, E5+P5, E6+P6]
```

> Notice: the two `the` tokens have the **same** embedding `E1 = E5`, but **different** positions `P1 != P5`, so their final input vectors are different. That is exactly what we want.

---

## Visual - the addition

Picture each word as a column. Embedding stacked on top, positional encoding below, summed to get the final input.

```
position           1            2            3      ...     6
                ┌────┐       ┌────┐       ┌────┐         ┌────┐
                │  • │       │  • │       │  • │         │  • │
embedding   E:  │  • │       │  • │       │  • │   ...   │  • │
                │  ⋮ │       │  ⋮ │       │  ⋮ │         │  ⋮ │
                └────┘       └────┘       └────┘         └────┘
                   +            +            +              +
                ┌────┐       ┌────┐       ┌────┐         ┌────┐
positional  P:  │ p1 │       │ p2 │       │ p3 │   ...   │ p6 │
encoding        │  ⋮ │       │  ⋮ │       │  ⋮ │         │  ⋮ │
                └────┘       └────┘       └────┘         └────┘
                  =            =            =              =
                ┌────┐       ┌────┐       ┌────┐         ┌────┐
final input :   │E1+P1│      │E2+P2│      │E3+P3│  ...   │E6+P6│
                └────┘       └────┘       └────┘         └────┘
```

---

## The formulas

The original Transformer uses sine and cosine of varying frequencies. For position `pos` and dimension index `i` (with `d_model` = embedding dimension):

```
PE(pos, 2i)   = sin( pos / 10000^(2i / d_model) )
PE(pos, 2i+1) = cos( pos / 10000^(2i / d_model) )
```

- Even-indexed dimensions get **sin**.
- Odd-indexed dimensions get **cos**.
- The denominator `10000^(2i/d_model)` makes lower dimensions oscillate fast and higher dimensions oscillate slowly.

This gives every position a unique vector and lets the model easily learn relative positions (e.g. "two tokens apart").

---

## Why sin and cos?

- Bounded values (between -1 and 1) - won't blow up the embedding.
- Different frequencies for each dimension -> each position gets a unique fingerprint.
- For any fixed offset `k`, `PE(pos+k)` can be expressed as a linear function of `PE(pos)`. This makes it easy for the model to attend to **relative** positions (e.g. "the word two before me").

---

## Key takeaways

- Transformers are parallel; they need PE to know token order.
- Final input = embedding + positional encoding (element-wise sum).
- Same word at different positions -> different final input vectors.
- Sine and cosine of varying frequencies give every position a unique signature.

---

| &lt;- Previous | Section README | Next -&gt; |
|---|---|---|
| [Architecture Overview](01-architecture-overview.md) | [02-transformer](./) | [Self-Attention](03-self-attention.md) |

[Back to root README](../README.md)
