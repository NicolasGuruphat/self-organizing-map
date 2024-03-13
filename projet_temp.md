# Self Organizing Map

par Gregory Moutote et Nicolas Guruphat 

## 3 Étude “theorique” de cas simples

### 3.1 Influence de η

#### *Dans le cas où η = 0 quelle sera la prochaine valeur des poids du neurone gagnant ?*

Dans ce cas là, le delta sera de 0 car le taux d'apprentissage est facteur de toute la formule. Le poids ne changera donc pas.

#### *Même question dans le cas où η = 1*

Si le taux d'apprentissage est égal à 1, $(x_i - w_{ij})$ tendra vers 0 (car nous travaillons sur le neuronne gagnant), donc le delta tendra également vers 0 au fur et à mesure que le neurone gagnat se rapprochera de l'entrée.

#### *Dans le cas où $η ∈]0, 1[$ (paramétrisation “normale”) où se situera le nouveau poids par rapport à W∗ et X en fonction de η (formule mathématique simple ou explication géométrique) ?*

$$\Delta w_{ij} = \eta e^{-\frac{||j-j^*||^2_c}{2\sigma^2}} (x_i - w_{ij})
$$
En considérant que $j = j^*$ devient alors :

$$
\Delta w_{ij} =  \eta \times 1 \times (x_i - w_{ij})
\Leftrightarrow \Delta w_{ij} = \eta (x_i - w_{ij})
$$

Sachant que $\eta \in ]0;1[$ et que $(x_i - w_{ij}) {\rightarrow} 0$, on peut dire que $\Delta w_{ij}\rightarrow 0$ avec une évolution semblable de $e^{-x}$. Donc, l'évolution du poids $w$ sera logarithmique.

#### *Que va-t-il se passer si η > 1 ?*

Si $\eta > 1$, les poids risquent de permettre un déplacement du neurone qui dépassera l'entrée. On peut donc dire qu'il "sur-réagira" à l'entrée en lui donnant un effet sur son poids trop grand par rapport à ce qu'il devrait être. 

### 3.2 Influence de $\sigma$

#### *Si $\sigma$ augmente, les neurones proches du neurone gagnant (dans la carte), vont-ils plus ou moins apprendre l'entrée courante ?*



| Équation  | Variation |
|-----------|-----------|
| $\sigma$    | $\nearrow$  |
| $2\sigma^2$ | $\nearrow$  |
| $\frac{\|\|j-j^*\|\|_c^2}{2\sigma^2}$ |$\searrow$ |
| $e^{\frac{\|\|j-j^*\|\|_c^2}{2\sigma^2}}$         | $\searrow$  |
| $\frac{1}{e ^{\frac{\|\|j-j^*\|\|_c^2}{2\sigma^2}}} \Leftrightarrow e ^{-\frac{\|\|j-j^*\|\|_c^2}{2\sigma^2}}$          | $\nearrow$  |
|   $\Delta w$         | $\nearrow$  |

Comme le démontre ce tableau, les neurones proches du neurone gagnant seront plus modifiés si $\sigma$ augmente.

#### *Si $\sigma$ est plus grand, à convergence, l’auto-organisation obtenue sera-t-elle donc plus “resserrée” (i.e. une distance plus faible entre les poids des neurones proches) ou plus “lâche” ?*

Si $\sigma$ est plus grand, on aura, à convergence, une auto-organisation plus resserrée. En effet, l'augementation de $\sigma$ implique une augmentation des poids et donc une attraction plus forte du neurone gagnant, ce qui concentrera les neurones en un réseau plus serré. 

#### *Quelle mesure (formule mathématique qui sera à implémenter dans la section 4) pourrait quantifier ce phénomène et donc mesurer l'influence de $\sigma$ sur le comportement de l'algorithme ?*

La mesure nous permettant de quantifier l'impact de $\sigma$ sur le comportement de l'algorithme est la dérivée de la fonction calculant $\Delta w$ en fonction de $\sigma$. En effet, cette dérivée nous indiquera les variations de l'évolution.

$$
\Delta w(\sigma)= \eta e^{-\frac{||j-j^*||^2_c}{2\sigma^2}} (x_i - w_{ij})\newline
\Rightarrow \Delta w'(\sigma)=-\eta(x_i - w_{ij})\times||j-j^*||^2_c\sigma^{-3}e^{\frac{-||j-j^*||^2_c}{2\sigma^2}}
$$

### 3.3 Influence de la distribution d’entrée  

#### *Prenons le cas très simple d’une carte à 1 neurone qui reçoit deux entrées X1 et X2*

##### *Si X1 et X2 sont présentés autant de fois, vers quelle valeur convergera le vecteur du poids du neurone (en supposant un $\eta$ faible - pour ne pas avoir à tenir compte de l’ordre de présentation des entrées - et suffisamment de présentations - pour négliger l’influence de l’initialisation des poids) ?*

La valeur du neurone se trouvera entre celles des deux entrées (à équi-distance)

##### *Même question si X1 est présenté n fois plus que X2.*


```
x1|---------N----------------|x2  
   <-------> <-------------->
    1/(n+1)      n/(n+1)
```

Avec N le neurone. La distance entre N et les entrées sera inversement proportionelle à la représentation de ces mêmes entrées. Plus leur représentation sera grande, plus le neurone en sera proche.

 On voit sur ce schéma que la valeur du poid de N est la moyenne pondérée des poids de x1 et x2 (la pondération représentant le nombre d'occurrence)


##### *Même question dans le cas d’entrées provenant d’une base de données quelconque.*

Le poid du neurone sera le barycentre des entrées. En effet, le barycentre représente le résultat pondéré de la position de vecteurs.

$w=\frac{k_1\times x_1 + k_2 \times x_2 + k_3 \times x_3}{k_1+k_2+k_3}$

Avec :
- $k_i$ le nombre d'occurence de l'entrée $i$ 
- $x_i$ le poids de l'entrée $i$   

#### *Reconsidérons maintenant le cas d’une carte avec plusieurs neurones en se focalisant sur un neurone quelconque. Quels exemples apprend ce neurone et avec quelle “force” ?*

Si ce neurone quelconque n'est pas le neurone gagnant, il apprendra ce vers quoi le neuronne gagnant tend, avec une force inversement proportionnelle à sa distance avec le neurone gagnant.

#### *En déduire comment vont se répartir les neurones d’une carte à plusieurs neurones recevant des données d’une base d’apprentissage en fonction de la densité des données. Pour rappel, la mesure de quantification vectorielle permet de mesurer ce phénomène.*

Si on prend un espace avec une proportion non uniforme d'entrée, il y aura plus de neurones gagnants dans la zone plus dense. Comme il y aura plus de neurones gagnants, plus de neuronnes seront attirés dans la zone plus dense. Toutefois, il restera un certain nombre de tirages dans la zone moins dense, mais qui sera moindre par rapport à la zone plus dense. Il y aura donc une répartion en proportion de la densité. Cela implique que l'espace entre les neurones sera optimal une fois que le réseau aura convergé.

Dans un espace avec un proportion uniforme d'entrée, la logique sera la même. En effet, la proportion de neurones gagnant par "zone" sera dorénavant uniforme et l'espace sera encore une fois réparti optimalement entre les neurones.