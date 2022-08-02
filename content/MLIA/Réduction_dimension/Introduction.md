# Comment décrire les données ?

## Approche 1
effectuer une analyse descriptive multidimensionelle
⊖ trop longue et souvent trop complexe

## Approche 2 : 
utiliser des méthodes d’analyse des données
ex: les méthodes factorielles comme l’Analyse en Composantes Principales (ACP) 
• **Synthèse :** réduire la dimension du problème tout en restituant le maximum d’information 
• **Descriptif** et exploratoire : visualisation des données (production de graphiques simples)

### Etude des observations
mise en évidence de ressemblances entre les observations 
	• quand peut-on dire de deux observations qu’elles sont similaires ? 
	• s’il y a beaucoup d’observations, est-il possible de faire un bilan des ressemblances ?

### Etude des variables
recherche de liaisons entre les variables 
• on s’intéresse généralement aux liaisons linéaires qui sont simples, très fréquentes et résument de nombreuses liaisons ⇒ étude des corrélations

### Lien entre l'étude des observations et celle des variables
ressemblances entre les observations caractérisées par les variables

# Conclusion 
Projeter les données dans un espace de faible dimension.
$\left\{\mathbf{x}_{i} \in \mathbb{R}^{d}\right\}_{i=1}^{n} \Rightarrow\left\{\tilde{\mathbf{x}}_{i} \in \mathbb{R}^{d^{\prime}}\right\}_{i=1}^{n}$ 
	$d^{\prime} \ll d$ (souvent 2 ).
- Paramètres :
	- Type de projection.
	- Mesure de similarité.
![[Pasted image 20220406112402.png]]

Méthodes
- Sélection de variables.
- Analyse en composantes principales (ACP, PCA).
- Réduction non-linéaire.