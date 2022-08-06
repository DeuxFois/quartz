# Kmeans
- centroïde 
	$\boldsymbol{\mu}=\frac{1}{m} \sum_{i}^{m} \mathbf{x}_{i}$
- inertie $\mathcal{I}_{T}=\sum_{\mathbf{x}_{i}}\left\|\mathbf{x}_{i}-\boldsymbol{\mu}\right\|^{2}$
![](https://miro.medium.com/max/700/1*5oS5b3j47zvCe43HuyUiUg.gif)

![[Pasted image 20220406110758.png]]
## Inertie
L'inertie $\mathcal{I}_{T}$ d'un nuage des points est représentée par la distance au carré des points à leur centroïde
$$\sum_{\mathbf{x}_{i}}\left\|\mathbf{x}_{i}-\boldsymbol{\mu}\right\|^{2}=\sum_{c=1} m_{c}\left\|\boldsymbol{\mu}_{c}-\boldsymbol{\mu}\right\|^{2} \quad+\sum_{c=1} \sum_{\mathbf{x}_{i} \mid \hat{y}_{i}=c}\left\|\mathbf{x}_{i}-\boldsymbol{\mu}_{c}\right\|^{2}
$$

**théorème de Huygens** 
Inertie totale = Inertie inter-classe + Inertie intra-classe
$$\mathcal{I}_{T}=\mathcal{I}_{B}+\mathcal{I}_{W}
$$

## Eviter un minimum local
## choix du nombre de cluster