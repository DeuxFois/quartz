## Guassian Discrimant Analysis model (GDA)
When we have a classification problem in which the input features $x$ are **continuous-valued random variables**, we can then use the Ganssian Discriminant Analysis (GDA) model, which models $p(x \mid y)$ using a multivariate normal distribution. The model is:
$$
\begin{aligned}
y & \sim \text { Bernoulli }(\phi) \\
x \mid y=0 & \sim \mathcal{N}\left(\mu_{0}, \Sigma\right) \\
x \mid y=1 & \sim \mathcal{N}\left(\mu_{1}, \Sigma\right)
\end{aligned}
$$
Writing out the distributions, this is:
$$
\begin{aligned}
p(y) &=\phi^{y}(1-\phi)^{1-y} \\
p(x \mid y=0) &=\frac{1}{(2 \pi)^{d / 2}|\Sigma|^{1 / 2}} \exp \left(-\frac{1}{2}\left(x-\mu_{0}\right)^{T} \Sigma^{-1}\left(x-\mu_{0}\right)\right) \\
p(x \mid y=1) &=\frac{1}{(2 \pi)^{d / 2}|\Sigma|^{1 / 2}} \exp \left(-\frac{1}{2}\left(x-\mu_{1}\right)^{T} \Sigma^{-1}\left(x-\mu_{1}\right)\right)
\end{aligned}
$$
<details>
<summary>Maximizing likelihood</summary>

Here, the parameters of our model are $\phi, \Sigma, \mu_{0}$ and $\mu_{1}$. (Note that while there're two different mean vectors $\mu_{0}$ and $\mu_{1}$, this model is usually applied using only one covariance matrix $\Sigma$.) The log-likelihood of the data is given by
$$
\begin{aligned}
\ell\left(\phi, \mu_{0}, \mu_{1}, \Sigma\right) &=\log \prod_{i=1}^{n} p\left(x^{(i)}, y^{(i)} ; \phi, \mu_{0}, \mu_{1}, \Sigma\right) \\
&=\log \prod_{i=1}^{n} p\left(x^{(i)} \mid y^{(i)} ; \mu_{0}, \mu_{1}, \Sigma\right) p\left(y^{(i)} ; \phi\right)
\end{aligned}
$$
By maximizing $\ell$ with respect to the parameters, we find the maximum likelihood estimate of the parameters to be:  

$$
\begin{aligned}
\phi &=\frac{1}{n} \sum\_{i=1}^{n} 1\lbrace\{y^{(i)}=1\rbrace\} \\
\mu\_{0} &=\frac{\sum\_{i-1}^{n} 1\lbrace\{y^{(i)}=0\rbrace\} x^{(i)}}{\sum\_{i=1}^{n} 1\lbrace\{y^{(i)}=0\rbrace\}} \\
\mu\_{1} &=\frac{\sum\_{i=1}^{n} 1\lbrace\{y^{(i)}=1\rbrace\} x^{(i)}}{\sum\_{i=1}^{n} 1\lbrace\{y^{(i)}=1\rbrace\}} \\
\Sigma &=\frac{1}{n} \sum\_{i=1}^{n}(x^{(i)}-\mu\_{y(i)})(x^{(i)}-\mu\_{y}(i))^{T} .
\end{aligned}
$$
  
  </details>


