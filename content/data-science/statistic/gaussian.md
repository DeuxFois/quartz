## Normal law
In Statistics and Probability theory, normal laws are among the most used laws to model phenomena. She is related to many mathematical objects including Brownian motion, Gaussian white noise or other probability laws.  
They are also called Gaussian laws, Gauss laws or Laplace-Gauss laws.

<br/>

More formally, a normal distribution is an absolutely **continuous probability distribution** which depends on **two parameters**:  

- $\mu$ the mean or expectation of the distribution (and also its median and mode)  
- $\sigma$ the standard deviation ($\sigma^{2}$ is the variance of the distribution )

$$
f(x)=\frac{1}{\sigma \sqrt{2 \pi}} \mathrm{e}^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^{2}}
$$
it is usual to use the notation with the variance $\sigma^{2}$:
$$
X \sim \mathcal{N}\left(\mu, \sigma^{2}\right) .
$$
![|600](_resources/Pasted%20image%2020220812145448.png)

## Théorème central limite et loi des grands nombres.

### Loi des grands nombres
Si on a un ensemble de n variables indépendantes et identiquement distribuées (i.i.d), plus n augmente, plus la moyenne des X s'approche de E(X).

### Théorème central limite
On a un ensemble de n variables i.i.d. Soit Sn la somme de ces éléments, et Xn la moyenne de ces évènements. Ces deux variables suivent approximativement des lois normales (si n est suffisamment grand):  

$$Zn = \frac{Sn - n\mu}{\sigma \sqrt{n}} = \frac{\bar Xn - \mu}{\frac{\sigma}{\sqrt{n}}}$$

Zn est la loi normale centrée réduite N(0,1)

Si on a 100 lancés de pièces succesifs, on peut estimer la probabiblité d'avoir plus de 55 faces, par exemple.

## Multivariate normal law
$$
\begin{aligned}
&Z \sim \mathcal{N}\left(\underbrace{\mu}\_\mathbb{R^n}, \underbrace{\Sigma}\_\mathbb{R^{n*n}}\right),  z\in \mathbb{R^n}\\
&E[z]=\mu \\
&\operatorname{Cov}(z)=E\left[(z-\mu)(z-\mu)^{\top}\right]\\
&E[z]=E_{z}=E_{z} z^{\top}-\left(E_{z}\right)\left(E_{z}\right)^{\top}
\end{aligned}
$$

![](_resources/Pasted%20image%2020220704151359.png)

![](_resources/Pasted%20image%2020220704151154.png)