# Loi normale
En théorie des probabilités et en statistique, les lois normales sont parmi les lois de probabilité les plus utilisées pour modéliser des phénomènes naturels issus de plusieurs événements aléatoires. Elles sont en lien avec de nombreux objets mathématiques dont le mouvement brownien, le bruit blan gaussien ou d'autres lois de probabilité. Elles sont également appelées lois gaussiennes, lois de Gauss ou lois de Laplace-Gauss des noms de Laplace (1749-1827) et Gauss (1777-1855), deux mathématiciens, astronomes et physiciens qui l'ont étudiée.

Plus formellement, une loi normale est une loi de probabilité absolument continue qui dépend de deux paramètres : son espérance, un nombre réel noté $\mu$, et son écart type, un nombre réel positif noté $\sigma$. La densité de probabilité de la loi normale d'espérance $\mu$, et d'écart type $\sigma$ est donnée par :
$$
f(x)=\frac{1}{\sigma \sqrt{2 \pi}} \mathrm{e}^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^{2}}
$$
La courbe de cette densité est appelée courbe de Gauss ou courbe en cloche, entre autres. C'est la représentation la plus connue de ces lois. Lorsqu'une variable aléatoire $X$ suit une loi normale, elle es dite gaussienne ou normale et il est habituel d'utiliser la notation avec la variance $\sigma^{2}$ :
$$
X \sim \mathcal{N}\left(\mu, \sigma^{2}\right) .
$$
La loi normale de moyenne nulle et d'écart type unitaire, $\mathcal{N}(0,1)$, est appelée loi normale centrée réduite ou loi normale standard.
Parmi les lois de probabilité, les lois normales prennent une place particulière grâce au théorème central limite. En effet, elles correspondent au comportement, sous certaines conditions, d'une suite d'expériences aléatoires similaires et indépendantes lorsque le nombre d'expériences est très élevé. Grâce à cette propriété, une loi normale permet d'approcher d'autres lois et ainsi de modéliser de nombreuses études scientifiques comme des mesures d'erreurs ou des tests statistiques, en utilisant par exemple les tables de la loi normale centrée réduite.

# Loi normale multivariée
$$
\begin{aligned}
&Z \sim \mathcal{N}\left(\underbrace{\mu}\_\mathbb{R^n}, \underbrace{\Sigma}\_\mathbb{R^{n*n}}\right),  z\in \mathbb{R^n}\\
&E[z]=\mu \\
&\operatorname{Cov}(z)=E\left[(z-\mu)(z-\mu)^{\top}\right]\\
&E[z]=E_{z}=E_{z} z^{\top}-\left(E_{z}\right)\left(E_{z}\right)^{\top}
\end{aligned}
$$

![](Pasted%20image%2020220704151359.png)

![](Pasted%20image%2020220704151154.png)