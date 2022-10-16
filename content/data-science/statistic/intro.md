
### Théorème des probabilités composées
La définition même des probabilités conditionnelles permet d'écrire que :
$$
p(A \cap B)=p(A) \times p(B \mid A)
$$
et aussi que:
$$
p(A \cap B)=p(B) \times p(A \mid B)
$$
C'est le théorème des probabilités composées, que l'on peut énoncer ainsi : si un événement résulte du concours de deux événements, sa probabilité est égale à celle de l'un d'eux multipliée par la probabilité conditionnelle de l'autre sachant que le premier est réalisé.

Soit, par exemple, à calculer la probabilité pour que, tirant successivement deux cartes d'un jeu de 32 cartes, ces deux cartes soient des valets. Appelons $A$ et $B$ les deux événements suivants :
- $A$ : la première carte est un valet ( $A$ désigne tous les tirages possibles dont la lère carte est un valet)
- $B$ : la deuxième carte est un valet ( $B$ désigne tous les tirages possibles dont la $2 \mathrm{e}$ carte est un valet)
La probabilité cherchée est $p(A \cap B)$ qui est aussi égale à $p(A) \times p(B \mid A)$.
Lors du premier tirage, il y a 32 cartes et 4 valets dans le jeu, d'où $p(A)=\frac{4}{32}$.
Lors du second tirage, il reste 31 cartes et seulement 3 valets, puisque l'événement $A$ est réalisé, d'où $p(B \mid A)=\frac{3}{31}$.
Le résultat est donc: $: p(A \cap B)=\frac{4}{32} \times \frac{3}{31}=\frac{3}{248} \simeq 0.012$.

## Evénements équiprobables
Dans le cas discret (par exemple tirer au hasard des cartes parmi un jeu de 52 cartes), si il y a **équiprobabilité des issues**, la probabilité est égale au nombre de cas favorables divisé par le nombre de cas possibles :  
$$\frac{|A|}{|Ω|}$$

Par exemple, trouver la probabilité de tirer un carreau (on parle de poker):  
$$\frac{13}{52} = \frac{1}{4}$$
  

## Théorème des probabilités totales

![](_resources/Pasted%20image%2020220825075403.png)

$$|A\cup B| = |A|+|B|-|A\cap B|$$






