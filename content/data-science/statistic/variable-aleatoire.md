## Variable aléatoire

### Discrète
La **fonction de masse** (probability mass function = pmf) décrit la probabilité d'obtenir chacune des issues. On la note p(x).
La **fonction de répartition** (cumulative mass function = cmf) décrit la probabilité d'avoir p(X < x). On la note F(x).
Elle est définie comme suit:  

$$F(x) = \sum \limits_{i=1}^x p(x)$$

L'**espérance** est la moyenne des issues. Elle est notée E(x).
On a:  

$$ E(x) = \sum \limits_{i=1}^n p(xi) * xi $$

**Propriétés de E(x)**:  
$$E(aX+Y+b) = aE(X) + E(Y) + b$$

La **variance** est l'espérance des carrés des écarts à la moyenne (le carré est là pour ne pas avoir de nombres négatifs). Notée Var(X):  
$$Var(X) = E((X - \mu)^2)$$

**Propriétés de Var(X)**:
Si X et Y sont *indépendantes*   

$$Var(aX + Y + b) = a^2Var(X) + Var(Y)$$  
$$Var(X) = E(X^2) - E(X)^2$$  

L'**écart-type** est un indicateur de la dipersion des mesures. C'est la racine carrée de la variance:
$$\sigma = \sqrt{Var(X)}$$  

### Continue
La **densité de probabilité** (probability density function = pdf) la loi de probabilité des issues. La "probabilité unitaire" est f(x)dx. On la note f(x).
La **fonction de répartition** (cumulative density function = cdf) décrit la probabilité d'avoir p(X < x). On la note F(x).
Elle est définie comme suit:  
$$F(b) = \int_{-\infty}^{b} p(x)dx$$

**Propriétés de F(x)**:
$$p(a \le X \le b) = F(b) - F(a) $$  
$$F'(x) = f(x)$$  
$$p(a \le X \le b) = \int_{a}^{b} f(x)dx$$  

L'**espérance** est la moyenne des issues. Elle est notée E(x).
On a:  
$$E(x) = \int_{a}^{b} xf(x)dx$$

**Propriétés de E(x)**:  
$$E(aX+Y+b) = aE(X) + E(Y) + b$$

La **variance** est l'écart à la moyenne (au carré pour ne pas avoir de nombres négatifs). Notée Var(X):  
$$Var(X) = E((X - \mu)^2)$$

**Propriétés de Var(X)**:
Si X et Y sont *indépendantes*  

$$Var(aX + Y + b) = a^2Var(X) + Var(Y)$$  
$$Var(X) = E(X^2) - E(X)^2$$  

L'**écart-type** est un indicateur de la dipersion des mesures. C'est la racine de la variance:  
$$\sigma = \sqrt{Var(X)}$$
