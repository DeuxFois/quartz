Consider training data $(\mathbf{x},y)\sim P(X,Y)$ with
- $\mathbf{x}\in\mathbb{R}^{p},$
- $y \in \{0,1\}.$
- $P({\bf x}|y)=\frac{1}{\sqrt{(2\pi)^{p}|\Sigma|}}\exp\left(-\frac{1}{2}({\bf x}-\mu_{y})^{T}\Sigma^{-1}({\bf x}-\mu_{y})\right)$

Using the Baye's rule, we have
$$
\begin{aligned}
P(Y=1 \mid \mathbf{x}) &=\frac{P(\mathbf{x} \mid Y=1) P(Y=1)}{P(\mathbf{x})} \\
&=\frac{P(\mathbf{x} \mid Y=1) P(Y=1)}{P(\mathbf{x} \mid Y=0) P(Y=0)+P(\mathbf{x} \mid Y=1) P(Y=1)} \\
&=\frac{1}{1+\frac{P(\mathbf{x} \mid Y=0) P(Y=0)}{P(\mathbf{x} \mid Y=1) P(Y=1)}} .
\end{aligned}
$$
It follows that with
$$
\sigma(x)=\frac{1}{1+\exp (-x)},
$$
we get
$$
P(Y=1 \mid \mathbf{x})=\sigma\left(\log \frac{P(\mathbf{x} \mid Y=1)}{P(\mathbf{x} \mid Y=0)}+\log \frac{P(Y=1)}{P(Y=0)}\right)
$$
Therefore,
$$\begin{aligned}
&P(Y=1 \mid \mathbf{x}) \\
&=\sigma(\log \frac{P(\mathbf{x} \mid Y=1)}{P(\mathbf{x} \mid Y=0)}+\underbrace{\log \frac{P(Y=1)}{P(Y=0)}}\_a) \\
&=\sigma(\log P(\mathbf{x} \mid Y=1)-\log P(\mathbf{x} \mid Y=0)+a) \\
&=\sigma\left(-\frac{1}{2}\left(\mathbf{x}-\mu\_1\right)^T \Sigma^{-1}\left(\mathbf{x}-\mu\_1\right)+\frac{1}{2}\left(\mathbf{x}-\mu\_0\right)^T \Sigma^{-1}\left(\mathbf{x}-\mu\_0\right)+a\right) \\
&=\sigma(\underbrace{\left(\mu\_1-\mu\_0\right)^T \Sigma^{-1}}\_{\mathbf{w}^T} \mathbf{x}+\underbrace{\frac{1}{2}\left(\mu\_0^T \Sigma^{-1} \mu\_0-\mu\_1^T \Sigma^{-1} \mu\_1\right)+a}\_b) \\
&=\sigma\left(\mathbf{w}^T \mathbf{x}+b\right)
\end{aligned}$$