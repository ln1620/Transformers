# 3. Evaluation Metrics

How do we measure whether a classifier is any good? With four standard metrics built on top of the **confusion matrix**.

---

## The confusion matrix

For a binary classifier, every prediction falls into one of four buckets:

|                       | Predicted: Positive | Predicted: Negative |
|-----------------------|:-------------------:|:-------------------:|
| **Actual: Positive**  | TP (True Positive)  | FN (False Negative) |
| **Actual: Negative**  | FP (False Positive) | TN (True Negative)  |

- **TP** - we said positive, it actually was positive.
- **TN** - we said negative, it actually was negative.
- **FP** - we said positive, it actually was negative (false alarm).
- **FN** - we said negative, it actually was positive (missed catch).

---

## The four metrics

### Accuracy

Out of *all* predictions, what fraction did we get right?

```
Accuracy = (TP + TN) / (TP + FP + TN + FN)
```

Good when classes are **balanced**. Misleading when one class dominates (e.g. 99% of patients are healthy).

### Precision

Out of all the items we **labeled positive**, how many actually were?

```
Precision = TP / (TP + FP)
```

Optimize for precision when **false positives are expensive** (e.g. spam filter wrongly flagging important mail).

### Recall

Out of all the items that **actually are positive**, how many did we catch?

```
Recall = TP / (TP + FN)
```

Optimize for recall when **false negatives are expensive** (e.g. cancer screening - missing a positive case is dangerous).

### F1-Score

Harmonic mean of precision and recall - one number that balances both.

```
F1 = 2 * (precision * recall) / (precision + recall)
```

Useful when you want a single metric and the classes are imbalanced.

---

## Worked example

A spam classifier evaluated on 100 emails gives:

| count | meaning                                            |
|-------|----------------------------------------------------|
| TP=40 | spam, correctly flagged                            |
| FP=10 | not spam, but wrongly flagged as spam              |
| FN=5  | spam, but classifier missed it                     |
| TN=45 | not spam, correctly left alone                     |

Compute the metrics:

```
Accuracy  = (40 + 45) / (40 + 10 + 45 + 5) = 85 / 100 = 0.85       (85 %)

Precision = 40 / (40 + 10) = 40 / 50 = 0.80                        (80 %)

Recall    = 40 / (40 + 5)  = 40 / 45 ≈ 0.889                       (88.9 %)

F1        = 2 * (0.80 * 0.889) / (0.80 + 0.889)
          ≈ 2 * 0.7112 / 1.689
          ≈ 0.842                                                  (84.2 %)
```

So this classifier is **good but not great** - it correctly handles 85% of emails, slightly favors recall over precision, and has a balanced F1 of ~0.84.

---

## When to use which

| metric    | use when...                                                |
|-----------|-------------------------------------------------------------|
| Accuracy  | classes are balanced and all errors cost the same           |
| Precision | false positives are expensive (spam, fraud-flagging)        |
| Recall    | false negatives are expensive (medical screening, security) |
| F1        | imbalanced data and you need a single number                |

---

## Beyond binary

For multi-class problems you can compute per-class precision/recall and then **macro-average** (treat all classes equally) or **micro-average** (weighted by support). For ranking and search problems you also use **Precision@k**, **Recall@k**, and **mAP**.

---

## Key takeaways

- The confusion matrix is the source of truth: TP, FP, FN, TN.
- Accuracy alone can lie when classes are imbalanced.
- Precision = "of what I flagged, how much was right?"
- Recall = "of what was truly positive, how much did I catch?"
- F1 balances the two.

---

| &lt;- Previous | Section README |
|---|---|
| [Gradient Descent](02-gradient-descent.md) | [03-supporting-concepts](./) |

[Back to root README](../README.md)
