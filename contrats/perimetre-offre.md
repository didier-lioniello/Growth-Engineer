# Périmètre d’offre

Version `2026.11.0`. Auteur : Didier Lioniello.

Ce document fixe **qui peut entrer dans le scoring** et **qui est exclu avant le score**. Ce n’est pas une promesse commerciale et ce n’est pas un cas client. Tant que la case « périmètre réel confirmé » du [paquet de livraison](paquet-livraison.md) vaut `non`, ces critères servent au jeu d’essai et à la construction. Ils ne qualifient pas une personne réelle.

## Cible retenue pour la version

Une demande est dans la cible géographique et de taille seulement si les deux tests suivants passent :

1. Pays connu égal à `France` ou `FR` après normalisation.
2. Effectif connu, entier, compris entre 20 et 2000, bornes incluses.

Conséquences, appliquées dans l’ordre du [contrat de scoring](contrat-scoring.md) :

| Situation | Décision |
| --- | --- |
| Pays connu, autre que France / FR | Rejet `hors_cible_pays`, pas de score |
| Effectif connu hors de 20–2000 | Rejet `hors_cible_effectif`, pas de score |
| Pays vide ou effectif vide | Pas de rejet sur ce critère. La dimension « cible » vaut 0 et `donnees_partielles` passe à `oui` |
| Pays France / FR et effectif dans 20–2000 | Dimension « cible » = 2, soit 20 points |

Le secteur n’est pas un filtre dans cette version. Le champ peut être stocké. Il ne change ni le rejet ni les points.

## Exclusions avant le score

Ces exclusions sont mécaniques. Les listes de mots sont dans le contrat de scoring, afin qu’il n’y ait qu’une copie.

- Candidature, stage, recherche d’emploi.
- Message dont l’objet est de vendre une prestation à l’équipe.
- Doublon d’email : ce n’est pas un rejet, c’est un rattachement au lead déjà ouvert.

## Ce que la machine ne décide pas

- Elle ne choisit pas le prix, l’offre ni le discours.
- Elle ne réserve pas de créneau.
- Elle n’écrit pas au lead.
- Elle ne déclare pas qu’un lead « élevé » sera gagné.

## Changer le périmètre

Toute modification (pays, bornes d’effectif, exclusion) est une nouvelle version. Le GTM Engineer met à jour ce fichier, le contrat de scoring si les points changent, et le jeu d’essai dans le même changement. Le Growth Engineer n’ajuste pas ces bornes dans le CRM.
