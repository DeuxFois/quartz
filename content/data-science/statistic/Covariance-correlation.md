## Covariance et corrélation
### Covariance
La covariance est une mesure de la façon dont deux variables varient ensemble. Par exemple la taille et le poids des girafes ont des covariances positives car quand l'une est grande, l'autre a tendance à l'être aussi. Inversement, quand la covariance est négative, quand une des variables est grande, l'autre a tendance a être petite.

$$Cov(X,Y) = E((X- \mu X)(Y- \mu Y))$$  

**Propriétés de la covariance**  

$$Cov(aX+b, cY+ d) = acCov(X,Y)$$  

$$Cov(X1+X2, Y) = Cov(X1,Y)+Cov(X2,Y)$$  

$$Cov(X,X) = Var(X)$$  

$$Cov(X,Y) = E(X,Y) - \mu X \mu Y$$  

$$Var(X+Y) = Var(X) + Var(Y) + 2Cov(X, Y)$$  


Si X et Y sont indépendants, alors Cov(X, Y) = 0.  
**Attention la réciproque n'est pas vraie!**

### Corrélation
Le coefficient de corrélation permet de créer une mesure sans unité, adaptée pour comparer entre deux paires de variables.  

$$Cor(X,Y)=\frac{Cov(X,Y)}{\sigma X\sigma Y}$$