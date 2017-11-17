# COMPTE RENDU TP SPARK 

### Probleme
Le but de se TP est de réorganiser de façon cohérente et intelligente des fichiers (.csv) sur un système de fichier distribué Hadoop. Les fichiers contiennent des informations astronomiques. Une des informations principales contenue dans ces fichiers sont deux valeurs, ra et decl, qui permettent le localiser l'observation celeste. Cependant il est coûteux d'effectuer des opérations sur ces quantités. Ainsi il est nécessaire de réorganiser ces fichiers selon une logique de proximité des objets célestes.

### Langage utilisé
Nous avons décidé de travailler sous python, en utilisant le package pyspark. 

### Première approche
La première approche, naive, que nous avons eu, fût de répartir les observations sur un quadrillage en prenant en compte les valeurs extrèmales de ra et decl, sans tenir compte de leuir signification physique.

Nous avons fixé le nombre de fichiers à 7x7 = 49 <SUP>1 </SUP>. Puis nous sommes allé chercher les valeurs extrèmales de ra et decl, afin d'affecter les informations à un certain fichier. La figure ci-dessous explique clairement l'affectation :
<figure>
<img src="http://www.cjoint.com/doc/17_11/GKrlyERxeXC_affectation1.png" 
width="300">
<figcaption>Figure 1: Logique d'affectation</figcaption>
</figure>

<SUP>1 </SUP> _Explications :_ Ce nombre est obtenu à partir du calcul simpe : 5Go/128Mo ~ 40 
Ainsi on choisit une grille de 7x7 = 49 cases pour avoir, dans le cas idéal d'une répartition homogène, des fichiers de moins de 128Mo (avec une petite sécurité évidemment). 

#### Résulats première approche 
Après avoir réussi à faire tourner notre premier code, nous nous sommes intéressés à la distribution effective de nos fichiers :
<figure>
<img src="http://www.cjoint.com/doc/17_11/GKrmphqYbMC_histoooooooo.png"
width="300"
alt="Bloc-Notes">
<figcaption>Figure 2: Répartition des lignes dans nos fichiers</figcaption>
</figure>


### Deuxième approche
Lors de noter première segmentation, nous avions ces bornes :
ra_min =
ra_max = 
Cependant, en s'intéressant à la répartition dans nos fichiers, nous nous sommes aperçus qu'une grande plage d'angles n'était pas présente dans nos données. En utilisant les propriétés des angles d'obseravtion, nous avons recentré les angles sur le lieu d'observation et nous avons obtenu 
ra_min = 
ra_max =
La fonction utlisée est simple :
```
def t_ra():
```


Ainsi les subdivisions sont bien plus précises, cf l'histogramme ci dessous : 

<figure>
<img src="http://www.cjoint.com/doc/17_11/GKrn1lGtdXn_better-histo.png"
width="300"
alt="Bloc-Notes">
<figcaption>Figure 3: Répartition des lignes dans nos fichiers</figcaption>
</figure>

Sur la Figure 3, on s'aperçoit que tous les plages de ra_min et de ra_decl sont utilisées.


