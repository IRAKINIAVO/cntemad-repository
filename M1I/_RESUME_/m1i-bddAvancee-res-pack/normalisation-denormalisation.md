### NORMALISATION

`Definition`

La normalisation est un processus de décomposition d'une table universelle en plusieurs tables en évitant le problème d'incohérence et de redondance sans perdre d'information tout en préservant les DFs et qui permet de vérifier la robustesse de leur conception pour améliorer la modélisation

---

`Roles de la normalisation`

La normalisation est mise en œuvre pour :
- Limiter la redondance des données
- Limiter la perte de données
- Limiter les incohérences de données
- Améliorer les performances de traitement

---

`Les 3 premieres formes normales`

1FN, 2FN, 3FN *(mais il y a aussi la forme normale BCNF de Boyce Codd)*

> Note: il existe 6 formes normales en total

---

`Relations entre les 3 premieres formes normales + BCNF`

- **1FN**
Une relation est 1FN si et seulement si tous ses attributs ont des valeurs atomiques (non multiples). 

=> Par definition d'une relation, toute relation est donc en 1FN

> attribut.s non atomique.s (separation possible)

<table>
<th>Produit</th>
<tr>
<td>
pc-0001
<br>
cell
<br>
samsung
</td>
</tr>
<td>
ord-03a
<br>
pc
<br>
dell
</td>
<tr>
<td>
mon-99
<br>
montre
<br>
casio
</td>
</tr>
</table>

> attribut.s atomique.s

<table>
<th>id produit</th>
<th>type produit</th>
<th>modele produit</th>
<tr>
<td>pc-0001</td>
<td>cell</td>
<td>samsung</td>
</tr>
<tr>
<td>ord-03a</td>
<td>pc</td>
<td>dell</td>
</tr>
<tr>
<td>mon-99</td>
<td>montre</td>
<td>casio</td>
</tr>
</table>

- **2FN**
Une relation est en 2FN si et seulement si:
```
1. La relation est en 1FN
2. Tout attribut non cle est pleinement dependant de l’intégralité de la cle ou bien toute DF entre la cle et un attribut non cle est elementaire. Autrement dit tout attribut non cle ne depend pas d une partie de la cle. Donc decomposition possible en plusieurs relation (R -> R1, R2, ... , Rn) La 2FN élimine donc les anomalies résultant des dépendances entre partie de clé et partie non clé.
```

=> Toute relation dont la seule cle est un attribut est une relation en 2FN

> **Exemple de non 2FN d'une relation**
Commande(codeClient, codeArticle, client, article) n'est pas en 2FN car la clE de cette relation est (codeClient, codeArticle) alors qu'on a aussi des DF `codeClient->client` et `codeArticle->article`

> **Exemple d'une relation en 2FN**
Etudiant(NumBacc, AnneeBacc, Nom, Prenom, Filiere) est en 2FN car: sa cle est `(NumBacc, AnneeBacc)` et aucune DF ne peut pas partir sur l'un de ces cles. Il faut l'integralitE de l'attribut cle pour determiner les attributs non cles 

- **3FN**
Une relation est en 3FN si et seulement si:
```
1. La relation est en 2FN 
2. Toutes les DF entre attribut clé et attribut non clé sont directes, c'est a dire pas de DF transitive. Autrement dit tout attribut non clé ne dépend pas d’un autre attribut non clé mais directement de la cle.
```

> **Exemple de non 3FN d'une relation**
Etudiant(Matricule, Nom, Classe, EffectifClasse) n'est pas en 3FN. En effet, `Matricule->Nom, Classe, EffectifClasse`. Or, on a aussi `Classe->EffectifClasse`. Donc on a une DF transitive: `Matricule->Classe->EffectifClasse` cause du non 3FN de cette relation

> **3FN pour la relation Etudiant**
On peut alors decomposer la relation comme suit pour avoir une relation en 3FN:
`Etudiant(Matricule, Nom, CodeClasse)`
`Classe(CodeClasse, EffectifClasse)`

- **BCNF**
Une relation est en BCNF (Forme normale de Boyce Codd) si et seulement si:
```
1. La relation est en 3FN 
2. Toutes les DF qui existent sont issues d'une seule cle primaire
```

>**Exemple d'une relation en BCNF**
`Etudiant(Matricule, Nom, Prenom, Adresse, Adresse_mail, Tel, Sexe, Niveau_id, Ville, Region)`
Tout les attributs non cles sont determinEs par la cle primaire 'matricule' Mais la cause du non BCNF c'est la DF: `ville->region` car ville n'est pas une cle
=> Par decomposition on obtient alors une autre table
`Ville(Ville_id, Region)` ce qui respecte la condition du BCNF

---

### DENORMALISATION

`Definition`

La denormalisation est un processus consistant à regrouper plusieurs tables liées par des références, en une seule table, en réalisant statiquement les opérations de jointure adéquates.

`Objectif`

L'objectif de la dénormalisation est d'améliorer les performances de la BD en implémentant les jointures plutôt qu'en les calculant.

`Interet (cas d'utilisation)`

Un schéma doit être dénormalisé lorsque les performances de certaines recherches sont insuffisantes et que cette insuffisance à pour cause des jointures

`Iconvenients`

La dénormalisation peut également avoir un effet néfaste sur les performances si on ne controle la redondance volontaire:

- **En mise à jour**
Les données redondantes devant être dupliquées plusieurs fois.

- **En contrôle supplémentaire**
Les moyens de contrôle ajoutés (triggers, niveaux applicatifs, etc.) peuvent être très coûteux.

- **En recherche ciblée**
Certaines recherches portant avant normalisation sur une "petite" table et portant après sur une "grande" table peuvent être moins performantes après qu'avant.

---

### DIFFERENCE ENTRE NORMALISATION & DENORMALISATION

<table>
<th>Normalisation</th>
<th>Denormalisation</th>
<tr>
<td>La normalisation consiste à diviser des tables plus volumineuses en tables plus petites, réduisant ainsi les données redondantes</td>
<td>La dénormalisation consiste à ajouter des données redondantes afin d'optimiser les performances</td>
</tr>
<tr>
<td>La normalisation est effectuée pour éviter les anomalies des bases de données</td>
<td>Une base de données dénormalisée peut offrir de meilleures performances en écriture qu'une base de données normalisée</td>
</tr>
</table>