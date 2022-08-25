## Distributions jointes et indépendance
Si on a plusieurs variables, on a une loi de probabilité à plusieurs variables (joint probability mass function) qu'on note p(xi, yi) pour le cas discret, et f(xi,yi) pour le cas continu.

Voir [ce pdf pour avoir des exemples](https://ocw.mit.edu/courses/mathematics/18-05-introduction-to-probability-and-statistics-spring-2014/readings/MIT18_05S14_Reading7a.pdf).    

La fonction de répartition à plusieurs variables est:  

$$F(x,y) = p(X \leq x, Y \leq y) = \iint\limits_{[a,y][b,x]} f(u,v)dudv$$

Pour retrouver la loi de densité de probabilité, il faut dériver selon les deux variables.  

$$f(x,y) = \frac{\partial^2F(x,y)}{\partial x\partial y}$$

### Loi de probabilité marginale
Une loi de probabilité marginale permet d'avoir le "comportement" d'une seule variable.

Si y prend ses valeurs dans [c, d], on a :  

$$ fX(x) =  \int_{c}^{d} f(x,y)dy$$

(on somme les valeurs de y).


### Fonction de répartition
Pour avoir la fonction de répartition marginale, si X et Y prennent leur valeur dans [a,b]x[c,d], on a:  
$$FX(x) = F(x,d)$$  

$$FY(y) = F(b,y)$$

### Indépendance
X et Y sont indépendantes ssi:  
$$F(X,Y) = FX(x)FY(y)$$  
ou encore:   
$$f(x,y) = fX(x)fY(y)$$  

