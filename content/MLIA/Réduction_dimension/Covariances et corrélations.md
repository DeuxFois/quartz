# Covariance et Corrélations
- Matrice des variances-covariances
$$
\begin{aligned}
S=\operatorname{Var}(\mathbf{X}) &=\left[\begin{array}{ccccc}
\operatorname{Var}\left(X_{1}\right) & \operatorname{Cov}\left(X_{1}, X_{2}\right) & \cdots & \operatorname{Cov}\left(X_{1}, X_{d}\right) \\
\operatorname{Cov}\left(X_{2}, X_{1}\right) & \ddots & \cdots & \operatorname{Cov}\left(X_{2}, X_{d}\right) \\
\vdots & & \vdots & \ddots & \vdots \\
\operatorname{Cov}\left(X_{d}, X_{1}\right) & \operatorname{Cov}\left(X_{d}, X_{2}\right) & \cdots & \operatorname{Var}\left(X_{d}\right)
\end{array}\right] \\
&=\left[\begin{array}{cccc}
s_{X_{1}}^{2} & s_{X_{1}, X_{2}}^{2} & \cdots & s_{X_{1}, X_{d}}^{2} \\
s_{X_{2}, X_{1}}^{2} & \ddots & \cdots & s_{X_{2}, X_{d}}^{2} \\
\vdots & \vdots & \ddots & \vdots \\
s_{X_{d}, X_{1}}^{2} & s_{X_{d}, X_{2}}^{2} & \cdots & s_{X_{d}}^{2}
\end{array}\right]
\end{aligned}
$$
avec $s_{j, j^{\prime}}^{2}$ la covariance entre les variables $X_{j}$ et $X_{j^{\prime}}$, tel que
$$
s_{X_{j}, X_{j^{\prime}}}^{2}=\sum_{i=1}^{m} p_{i}\left(x_{i j}-\mu_{j}\right)\left(x_{i j^{\prime}}-\mu_{j^{\prime}}\right)
$$
La covariance mesure la liaison linéaire qui peut exister entre un couple de variables quantitatives.

$$
\operatorname{Cor}(\mathbf{X})=\left[\begin{array}{cccc}1 & \operatorname{Cor}\left(X_{1}, X_{2}\right) & \cdots & \operatorname{Cor}\left(X_{1}, X_{d}\right) \ \operatorname{Cor}\left(X_{2}, X_{1}\right) & \ddots & \cdots & \vdots \ \vdots & \vdots & \ddots & \vdots \ \operatorname{Cor}\left(X_{d}, X_{1}\right) & \cdots & \cdots & 1\end{array}\right]=\left[\begin{array}{cccc}1 & \frac{s_{X_{1}, X_{2}}^{2}}{s_{X_{1}}{ }{X} X{2}} & \cdots & \frac{s_{X_{1}, X_{d}}^{2}}{s_{X_{1}}{ }^{s} X_{d}} \ \frac{s_{X_{2}}^{2}, X_{1}}{s_{X_{2}}{ }^{s} X_{1}} & \ddots & \cdots & \frac{s_{X_{2}}^{2}, X_{d}}{s_{X_{2}}{ }^{s} X_{d}} \ \vdots & \vdots & \ddots & \vdots \ \frac{s_{X_{d}, X_{1}}^{2}}{s_{X_{d}}{ }^{s} X_{1}} & \frac{s_{X_{d}, X_{2}}^{2}}{ X_{d}{ }^{s} X_{2}} & \cdots & 1\end{array}\right]
$$