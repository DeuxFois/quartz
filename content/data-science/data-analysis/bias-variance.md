## Introduction

consider a function $\mathcal{f} : \mathcal{X} \rightarrow \mathcal{Y}$ produced by some learning algorithm. The prediction of this function can be evaluated through a loss $$ \ell:  \mathcal{Y} \times \mathcal{Y} \rightarrow \mathbb{R}$$ such that $\mathcal{l}(y, f(x)) \geq 0$ measures how close the prediction $\mathcal{f}(x)$ from $y$ is

<br />
<br />

Let $\mathcal{F}$ the hypothesis space, i.e. the set of all functions $f$ than can be produced by the chosen learning algorithm.
We are looking for a function $f\in{\mathcal{F}}$ with a small _exprected risk_ (or generalization error)
$$R(f)=\mathbb{E}_{(\mathbf{x},y)\sim{\mathcal{P}}(X,Y)}\left[\ell(y,f(\mathbf{x})\right)].$$
So, for a given data generating distribution $P(X,Y)$ and a given hypthesis space $\mathcal{F}$, the optimal model is $$f_{*}=\arg\operatorname*{min}_{f\in{\mathcal{F}}}R(f).$$

<br />

Unfortunaly, since $P(X,Y)$ is inknown, the exprected risk cannot be evaluated and the optimal model cannot be determined

However, if we have i.i.d training data $\mathbf{d}=\{(\mathbf{x}_{i},y_{i})|i=1,\cdot\cdot,N\}$, we can compute an estimate, the _empirical risk_ (or training error)
$$\hat{R}(f,{\bf d})=\frac{1}{N}\sum_{({\bf x}_{i},y_{i})\in\bf d}\ell(y_{i},f({\bf x}_{i})).$$
<br />
<br />
Most machine learning algorithms, including _neural networks_, implement empirical risk minimization. Under regularity assumptions, empirical risk minimizers converge:
$$\operatorname*{lim}_{N\rightarrow\infty}f_{*}^{\bf d}=f_{*}$$

![[_resources/Pasted image 20221008124227.png]]


Let ${\mathcal{Y}}^{\chi}$ be the set of all functions $f:X\to{\mathcal{V}}$. 
We define the _Bayes risk_ as the minimal expected risk over all possible functions, $$R_{B}=\operatorname*{min}_{f\in{\mathcal{Y}}^{X}}R(f)$$and call Bayes model the model that achieves this minimum.
No model $f$ can perform better than $f_B$.


## Biais Variance

- If the capacity of $\mathcal{F}$ is too low, then $f_{B}\not\in \mathcal{F}$ and $R(f)-R_{B}$ is large for any $f \in \mathcal{F}$, including $f_*$ and $f_{*}^{\bf d}$. Such models are said to **underfit** the data. 
- If the capacity of $\mathcal{F}$ is too high, then  $f_{B}\in F$ or $R(f_{\ast})-R_{B}$ is small. However, because of the high capacity of the hypothesis space, the empirical risk minimizer could fit the training data arbitrarily well such  that$$R(f_{*}^{\mathbf{d}})\ge R_{B}\ge\hat{R}(f_{*}^{\mathbf{d}},{\mathbf{d}})\ge0.$$In this situation, $f_{*}^{\mathbf{d}}$ becomes too specialized with respect to the true data generating process and a large reduction of the empirical risk (often) comes at the price of an increase of the expected risk of the empirical risk minimizer $R(f_{\star}^{\mathrm{d}})$. In this situation, $f_{*}^{\mathbf{d}}$ is said to **overfit** the data.
 ![[_resources/Pasted image 20221008142246.png]]
Nevertheless, an unbiased estimate of the expected risk can be obtained by $f_{*}^{d}$ evaluating on data $d_{test}$ independent from the training samples $d$ : $$\hat{R}(f_{\ast}^{\mathrm{d}},{\bf d}_{\mathrm{test}})=\frac{1}{N}\sum_{({\bf x}_{i},y_{i})\in\mathbf{d_{test}}}\ell(y_{i},f_{\ast}^{\mathrm{d}}({\bf x}_{i}))$$This **test error** estimate can be used to evaluate the actual performance of the model. However, it should not be used, at the same time, for model selection.

### Best evaluation protocol
![[_resources/Pasted image 20221008150101.png]]
![[_resources/Pasted image 20221008150108.png]]
![[_resources/Pasted image 20221008150115.png]]


## Biais-variance decomposition

The local expected risk of $f_*^{\mathrm{d}}$ is
$$
\begin{aligned}
R\left(f_*^{\mathbf{d}} \mid x\right) &=\mathbb{E}_{y \sim P(Y \mid x)}\left[\left(y-f_*^{\mathbf{d}}(x)\right)^2\right] \\
&=\mathbb{E}_{y \sim P(Y \mid x)}\left[\left(y-f_B(x)+f_B(x)-f_*^{\mathbf{d}}(x)\right)^2\right] \\
&=\mathbb{E}_{y \sim P(Y \mid x)}\left[\left(y-f_B(x)\right)^2\right]+\mathbb{E}_{y \sim P(Y \mid x)}\left[\left(f_B(x)-f_*^{\mathbf{d}}(x)\right)^2\right] \\
&=R\left(f_B \mid x\right)+\left(f_B(x)-f_*^{\mathbf{d}}(x)\right)^2
\end{aligned}
$$
where
- $R\left(f_B \mid x\right)$ is the local expected risk of the Bayes model. This term cannot be reduced.
- $\left(f_B(x)-f_*^{\mathrm{d}}(x)\right)^2$ represents the discrepancy between $f_B$ and $f_*^{\mathrm{d}}$.

Formally, the expected local expected risk yields to:
$$
\begin{aligned}
&\mathbb{E}_{\mathbf{d}}\left[R\left(f_*^{\mathrm{d}} \mid x\right)\right] \\
&=\mathbb{E}_{\mathbf{d}}\left[R\left(f_B \mid x\right)+\left(f_B(x)-f_*^{\mathrm{d}}(x)\right)^2\right] \\
&=R\left(f_B \mid x\right)+\mathbb{E}_{\mathbf{d}}\left[\left(f_B(x)-f_*^{\mathrm{d}}(x)\right)^2\right] \\
&=\underbrace{R\left(f_B \mid x\right)}_{\text {noise }(x)}+\underbrace{\left(f_B(x)-\mathbb{E}_{\mathbf{d}}\left[f_*^{\mathrm{d}}(x)\right]\right)^2}_{\text {bias }^2(x)}+\underbrace{\mathbb{E}_{\mathbf{d}}\left[\left(\mathbb{E}_{\mathbf{d}}\left[f_*^{\mathrm{d}}(x)\right]-f_*^{\mathrm{d}}(x)\right)^2\right]}_{\operatorname{var}(x)}
\end{aligned}
$$
This decomposition is known as the bias-variance decomposition.
- The noise term quantities the irreducible part of the expected risk.
- The bias term measures the discrepancy between the average model and the Bayes model.
- The variance term quantities the variability of the predictions.
![[_resources/Pasted image 20221008160744.png]]

