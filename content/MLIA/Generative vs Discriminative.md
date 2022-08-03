Generative v.s. Discriminative Models
[Apprentissage non-supervisé](Apprentissage%20non-supervisé.md)
## What's Generative or Discriminative model?

Let's say you have input data x and you want to classify the data into labels y. A **generative model** learns the joint probability distribution $$p(x,y)$$ and a **discriminative model** learns the conditional probability distribution $$p(y|x)$$ - which you should read as "the probability of $y$ given $x$

Generative algorithms model p(x,y), which can be tranformed into p(y|x) by applying Bayes rule and then used for classification.

The overall gist is that discriminative models generally outperform generative models in classification tasks.

A generative algorithm models how the data was generated in order to categorize a signal. It asks the question: based on my generation assumptions, which category is most likely to generate this signal? A discriminative algorithm does not care about how the data was generated, it simply categorizes a given signal.
![](Pasted%20image%2020220704120136.png)
## Example

Let's say you have input data `x` and you want to classify the data into labels `y`. A generative model learns the **joint** probability distribution `p(x,y)` and a discriminative model learns the **conditional** probability distribution `p(y|x)` - which you should read as _"the probability of `y` given `x`"_.

Here's a really simple example. Suppose you have the following data in the form `(x,y)`:

`(1,0), (1,0), (2,0), (2, 1)`

`p(x,y)` is

```
      y=0   y=1
     -----------
x=1 | 1/2   0
x=2 | 1/4   1/4
```

`p(y|x)` is

```
      y=0   y=1
     -----------
x=1 | 1     0
x=2 | 1/2   1/2
```

If you take a few minutes to stare at those two matrices, you will understand the difference between the two probability distributions.

The distribution `p(y|x)` is the natural distribution for classifying a given example `x` into a class `y`, which is why algorithms that model this directly are called discriminative algorithms. Generative algorithms model `p(x,y)`, which can be transformed into `p(y|x)` by applying Bayes rule and then used for classification. However, the distribution `p(x,y)` can also be used for other purposes. For example, you could use `p(x,y)` to _generate_ likely `(x,y)` pairs.

From the description above, you might be thinking that generative models are more generally useful and therefore better, but it's not as simple as that. [This paper](http://papers.nips.cc/paper/2020-on-discriminative-vs-generative-classifiers-a-comparison-of-logistic-regression-and-naive-bayes.pdf) is a very popular reference on the subject of discriminative vs. generative classifiers, but it's pretty heavy going. The overall gist is that discriminative models generally outperform generative models in classification tasks.

[1] [https://stackoverflow.com/questions/879432/what-is-the-difference-between-a-generative-and-discriminative-algorithm](https://stackoverflow.com/questions/879432/what-is-the-difference-between-a-generative-and-discriminative-algorithm)