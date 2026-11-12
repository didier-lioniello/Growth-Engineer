# Contrat de scoring

Version `2026.11.0`. Auteur : Didier Lioniello.

Grille unique. Le Growth Engineer recalcule avec cette page. Le nœud de code n8n devra appliquer la même page. Un modèle de langage ne note pas dans cette version : il n’y a pas d’étape d’extraction libre.

Normalisation commune, avant toute règle :

1. Trim des textes.
2. Email en minuscules, espaces internes retirés.
3. Pour comparer des mots : ramener les apostrophes `’` `‘` `ʼ` à l’apostrophe droite `'`, passer en minuscules, retirer les accents (décomposition Unicode, marques supprimées).
4. Une frontière de mot est tout ce qui n’est pas une lettre. Le trait d’union et l’apostrophe sont des frontières. « co-fondateur » contient donc le mot `fondateur`.
5. Effectif : premier entier de la valeur brute, sinon vide. « environ 120 » donne 120. « NC » donne vide.
6. Pays : même repli que les mots, puis comparaison exacte. Seuls `france` et `fr` sont dans la cible.

## Ordre de décision

On s’arrête à la première décision.

1. **Doublon.** Email normalisé non vide, déjà présent sur un lead. Un email vide ne rapproche aucune fiche : il n’y a rien à comparer. Nouvelle activité sur le lead trouvé. Statut de l’entrée : `doublon`. Pas de nouveau score.
2. **Rejet candidature.** Le message ou le titre contient un mot entier de la liste candidature, ou l’expression `curriculum vitae`.
3. **Rejet fournisseur.** Le message contient une expression de la liste fournisseur.
4. **Rejet pays.** Pays non vide et différent de `france` / `fr`.
5. **Rejet effectif.** Effectif non vide et hors de 20–2000, bornes incluses.
6. **Incomplet.** Il manque un champ obligatoire du [canal](contrat-canaux.md). On liste les champs. Pas de score.
7. **Score.** Sinon, calcul ci-dessous et route.

Le rejet gagne donc sur l’incomplet : une candidature sans email est `rejete`, motif `candidature`, pas `incomplet`.

### Liste candidature (mots entiers, forme déjà repliée)

`candidature`, `candidat`, `candidate`, `stage`, `alternance`, `alternant`, `alternante`, `stagiaire`, `etudiant`, `etudiante`, `cv`

Expression, sous-chaîne : `curriculum vitae`

### Liste fournisseur (sous-chaînes, forme déjà repliée)

`nous vendons`, `on vend`, `je vous propose nos services`, `notre offre de services`

## Points

Chaque dimension vaut 0, 1 ou 2. Les points sont fixés ici.

| Dimension | Niveau | Points | Règle |
| --- | --- | --- | --- |
| Cible | 2 | 20 | Pays `france` ou `fr`, et effectif dans 20–2000 |
| Cible | 0 | 0 | Pays vide ou effectif vide. `donnees_partielles = oui`. Les rejets ont déjà écarté les hors-cibles connus |
| Rôle | 2 | 20 | Le titre contient un mot entier décideur |
| Rôle | 1 | 10 | Sinon, le titre contient un mot entier influenceur |
| Rôle | 0 | 0 | Titre vide, ou aucun mot des deux listes. Titre vide ⇒ données partielles |
| Besoin | 2 | 30 | Le message contient une sous-chaîne « besoin fort » |
| Besoin | 1 | 15 | Sinon, une sous-chaîne « besoin faible » |
| Besoin | 0 | 0 | Sinon, y compris message vide |
| Timing | 2 | 30 | Le message contient une sous-chaîne « délai court » |
| Timing | 1 | 15 | Sinon, une sous-chaîne « délai long » |
| Timing | 0 | 0 | Sinon |

`score_valeur` = somme des quatre points. Entier, de 0 à 100. Pas d’arrondi : chaque ligne de points est déjà entière.

### Mots entiers du rôle (forme repliée)

Décideur : `directeur`, `directrice`, `dirigeant`, `dirigeante`, `responsable`, `head`, `fondateur`, `fondatrice`, `gerant`, `gerante`, `ceo`, `coo`, `cmo`, `cro`, `president`, `presidente`

Influenceur : `charge`, `chargee`, `specialiste`, `consultant`, `consultante`

On teste le décideur avant l’influenceur.

### Besoin fort (sous-chaînes repliées)

`demandes entrantes`, `qualif`, `leads`, `pipeline`, `prise de rendez-vous`, `no-show`, `relance`, `crm`

`qualif` couvre qualification, qualifier, qualifiées.

### Besoin faible (sous-chaînes repliées)

`documentation`, `informations`, `tarif`, `un prix`, `demo`, `appel`

### Délai court (sous-chaînes repliées)

`cette semaine`, `ce mois`, `ce trimestre`, `sous 30 jours`, `sous 60 jours`, `sous 90 jours`, `avant la fin du trimestre`, `avant la fin du mois`

### Délai long (sous-chaînes repliées)

`l'annee prochaine`, `l'an prochain`, `pas urgent`, `dans six mois`, `dans 6 mois`

Si un délai court et un délai long sont tous les deux présents, le niveau timing est 2.

## Routes

| Condition | `route_machine` | `statut` juste après le score |
| --- | --- | --- |
| score ≥ 70 | `route_eleve` | `route_eleve` |
| 40 ≤ score ≤ 69 | `route_moyen` | `route_moyen` |
| score ≤ 39 | `route_faible` | `route_faible` |

La file humaine est la même mécanique pour les trois routes et pour `incomplet` : passage à `en_attente`, traitement par une personne, puis `traite`. L’ordre de traitement des files est fixé dans le [workflow du Growth Engineer](../workflows/growth-engineer.md).

`donnees_partielles = oui` ne change pas la route. Le flag reste visible dans la file. Il n’autorise pas un message automatique.

## Ce qui est interdit

- Changer un poids, un mot, un seuil ou l’ordre sans publier une nouvelle `score_version`.
- Laisser un modèle de langage choisir la route.
- Recalculer les leads déjà scorés lorsqu’une version nouvelle paraît. La nouvelle grille vaut pour les entrées futures. Une reprise de l’historique serait un blueprint séparé, absent de cette version.
- Interpréter la route comme une probabilité de gain. Aucun taux n’est associé à ces seuils.

## Contrôle

Le [jeu d’essai](jeu-essai.md) donne le détail de calcul attendu. Un écart d’un point entre le recalcul et ce contrat est un écart structurel, même si la route tombe dans la même bande.

## Limites de la grille

La grille est volontairement mécanique. Elle se recalcule à la main. Elle est aussi brutale :

- les listes « besoin » et « délai » sont des sous-chaînes : `ce mois` reconnaît « ce mois-ci », `appel` reconnaît « appeler », `qualif` reconnaît « qualification » ;
- le mot `charge` reconnaît « chargé » et aussi « en charge » ;
- `crm` et `demo` reconnaissent ces lettres à l’intérieur d’un mot plus long.

Un faux positif connu se corrige en publiant une nouvelle version, pas en ajustant un lead dans le CRM. Aucune de ces règles n’est un taux de conversion.
