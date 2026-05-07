# 3. Word Embeddings

## Definition

A **word embedding** is a numerical vector representation of a word in a lower-dimensional space that captures **semantic and syntactic information**. Words with similar meaning end up with similar vectors.

In one line: *embeddings turn words into points in space, and similar words sit close together.*

---

## Intuition - the "KING" example

A machine cannot understand the word `KING` directly, so it converts the word into a vector. But how does it pick the numbers?

Imagine the model asks a set of questions about the word and stores the answers. Each question becomes one dimension of the vector.

| feature   | battle | horse | king | man | queen | woman |
|-----------|:------:|:-----:|:----:|:---:|:-----:|:-----:|
| authority |   0    | 0.01  |   1  | 0.2 |   1   |  0.2  |
| event     |   1    |   0   |   0  |  0  |   0   |   0   |
| has tail  |   0    |   1   |   0  |  0  |   0   |   0   |
| rich      |   0    |  0.1  |   1  | 0.3 |   1   |  0.2  |
| gender    |   0    |   1   |  -1  | -1  |   1   |   1   |

Reading off columns gives each word a numerical vector:

```
battle -> [0,    1, 0,  0,    0]
horse  -> [0.01, 0, 1,  0.1,  1]
king   -> [1,    0, 0,  1,   -1]
man    -> [0.2,  0, 0,  0.3, -1]
queen  -> [1,    0, 0,  1,    1]
woman  -> [0.2,  0, 0,  0.2,  1]
```

Each vector position is a **feature** of the word. These numbers capture meaning.

---

## How big are real embeddings?

In real models, we don't hand-design 5 features - the model learns them.

- **Google's Word2Vec** uses **300 dimensions** = 300 latent "questions" about each word.
- BERT's static input layer uses **768 dimensions**.
- GPT-3 uses **12,288 dimensions**.

Each dimension is some learned feature - we don't always know what it means in human terms, but together they capture meaning.

---

## Vector arithmetic - the magic property

Because semantically related words end up in similar regions of vector space, embeddings have a beautiful property:

```
vec(king) - vec(man) + vec(woman)  ≈  vec(queen)
```

Visually:

```
       king
        ^
        |  ( - man )
        |
        +-----> ( + woman )
                       |
                       v
                     queen
```

The model has learned the abstract concepts of **gender** and **royalty** as directions in space.

---

## Methods to learn embeddings

These are all **prediction-based** (unlike BoW / TF-IDF which are frequency-based):

- **Word2Vec** - predicts surrounding words from a target word (skip-gram) or vice versa (CBOW).
- **GloVe** - factorizes a global word-word co-occurrence matrix.
- **fastText** - like Word2Vec, but operates on character n-grams so it can handle unseen / misspelled words.

All three produce **fixed embeddings**: the word `bank` gets the same vector regardless of context. This is the limitation we will fix with ELMo and Transformers in the next chapters.

---

## Key takeaways

- A word embedding is a **vector** that represents a word.
- Each dimension corresponds to some learned feature (rich, gender, tense, animacy, ...).
- Similar words are **close** in vector space.
- Word2Vec, GloVe, fastText all produce **static** embeddings - one vector per word, regardless of context.

---

| &lt;- Previous | Section README | Next -&gt; |
|---|---|---|
| [Bag of Words and TF-IDF](02-bow-and-tfidf.md) | [01-fundamentals](./) | [ELMo and Contextual Embeddings](04-elmo-contextual.md) |

[Back to root README](../README.md)
