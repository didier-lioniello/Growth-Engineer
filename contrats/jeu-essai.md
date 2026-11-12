# Jeu d’essai

Version `2026.11.0`. Auteur : Didier Lioniello.

Fiches fictives. Domaine `exemple.test` uniquement. Aucune de ces personnes n’existe. Les décisions ci-dessous sont l’application du [contrat de scoring](contrat-scoring.md) et du [contrat de canaux](contrat-canaux.md), dans l’ordre des identifiants.

Rejeu :

1. Partir d’une base sans aucune adresse `@exemple.test`.
2. Appliquer les fiches dans l’ordre `T01` → `T25`. `T11` n’a de sens qu’après `T01`.
3. Considérer l’enrichissement comme un échec : un champ vide le reste. Le blueprint de capture impose le même échec forcé lors d’un rejeu n8n.
4. Comparer `statut`, `motif_rejet` ou `champs_manquants`, `score_valeur`, `detail_score`, `route_machine`, `donnees_partielles`.
5. Un écart d’un point, ou une route différente, renvoie la version au GTM Engineer.

Le jeu teste la grille, y compris des canaux encore `branche = non`. En exploitation, une fiche réelle arrivée sur un canal non branché est un écart structurel. Ici, ce n’est pas le cas : ce sont des fiches de papier.

Identifiant logique `L-Txx` : le lead que la fiche doit créer. Ce n’est pas un `lead_id` de CRM.

## Décisions attendues

| Id | Canal | Email | Issue attendue |
| --- | --- | --- | --- |
| T01 | `formulaire_site` | `a.leade@exemple.test` | Crée `L-T01`. Score 100. `route_eleve`. Partiel `non` |
| T02 | `formulaire_site` | vide | `incomplet`. Champ `email`. Pas de score |
| T03 | `formulaire_site` | `c.leade@exemple.test` | `rejete` / `candidature` |
| T04 | `formulaire_site` | `f.leade@exemple.test` | `rejete` / `fournisseur` |
| T05 | `formulaire_site` | `p.leade@exemple.test` | `rejete` / `hors_cible_pays` |
| T06 | `formulaire_site` | `s.leade@exemple.test` | `rejete` / `hors_cible_effectif` |
| T07 | `formulaire_site` | `m.leade@exemple.test` | Crée `L-T07`. Score 50. `route_moyen`. Partiel `oui` |
| T08 | `formulaire_site` | `b.leade@exemple.test` | Crée `L-T08`. Score 30. `route_faible`. Partiel `non` |
| T09 | `formulaire_site` | `n.leade@exemple.test` | Crée `L-T09`. Score 70. `route_eleve`. Partiel `non` |
| T10 | `formulaire_site` | `q.leade@exemple.test` | Crée `L-T10`. Score 40. `route_moyen`. Partiel `non` |
| T11 | `formulaire_site` | `a.leade@exemple.test` | Doublon. Activité sur `L-T01`. Pas de nouveau lead, pas de nouveau score |
| T12 | `acquisition_payante` | `u.leade@exemple.test` | `incomplet`. Champ `utm_campaign`. Pas de score |
| T13 | `import_manuel` | `i.leade@exemple.test` | Crée `L-T13`. Score 40. `route_moyen`. Partiel `non` |
| T14 | `formulaire_site` | `v.leade@exemple.test` | Crée `L-T14`. Score 80. `route_eleve`. Partiel `oui` |
| T15 | `formulaire_site` | `o.leade@exemple.test` | Crée `L-T15`. Score 75. `route_eleve`. Partiel `non` |
| T16 | `formulaire_site` | `g.leade@exemple.test` | Crée `L-T16`. Score 40. `route_moyen`. Effectif 20 accepté |
| T17 | `formulaire_site` | `h.leade@exemple.test` | Crée `L-T17`. Score 40. `route_moyen`. Effectif 2000 accepté |
| T18 | `formulaire_site` | `j.leade@exemple.test` | `rejete` / `hors_cible_effectif`. Effectif 19 |
| T19 | `formulaire_site` | `k.leade@exemple.test` | `rejete` / `hors_cible_effectif`. Effectif 2001 |
| T20 | `formulaire_site` | `stade.leade@exemple.test` | Crée `L-T20`. Score 100. `route_eleve`. Le mot « stade » n’est pas `stage` |
| T21 | `email_entrant` | `w.leade@exemple.test` | `rejete` / `candidature`. Mot entier `cv` |
| T22 | `formulaire_site` | vide | `rejete` / `candidature`, et non `incomplet` |
| T23 | `formulaire_site` | `y.leade@exemple.test` | Crée `L-T23`. Score 60. `route_moyen`. Partiel `non` |
| T24 | `formulaire_site` | `z.leade@exemple.test` | Crée `L-T24`. Score 40. `route_moyen`. Partiel `oui` (société vide, cible quand même complète) |
| T25 | `email_entrant` | `t.leade@exemple.test` | `rejete` / `candidature`. Le mot est dans le titre, message « Bonjour » |

Sauf mention, société = `Atelier Exemple`, pays = `France`, et les UTM sont vides.

## Textes et arithmétique

Niveaux notés `cible / rôle / besoin / timing`, puis points.

### T01 — 2/2/2/2 = 20+20+30+30 = 100

- Effectif `120`. Titre `Directrice des opérations`.
- Message : `Nous voulons une qualification des demandes entrantes. Décision avant la fin du trimestre.`
- `detail_score` = `cible:2;role:2;besoin:2;timing:2`

### T02 — incomplet

- Mêmes titre, effectif, pays et message que T01. Email vide.
- Aucun mot de rejet. `champs_manquants` = `email`

### T03 — candidature

- Message : `Je vous envoie ma candidature pour un stage.`

### T04 — fournisseur

- Message : `Nous vendons une prestation et je vous propose nos services.`

### T05 — pays

- Pays `Allemagne`. Effectif `100`. Titre `Directeur commercial`.
- Message : `Qualification des leads avant la fin du trimestre.`
- Le message aurait donné un score élevé. Le rejet pays intervient avant.

### T06 — effectif

- Effectif `5`. Titre `Directeur`. Message : `Il nous faut qualifier les demandes entrantes ce trimestre.`

### T07 — 0/2/2/0 = 0+20+30+0 = 50

- Société vide. Pays `France`. Effectif vide. Titre `Responsable commercial`.
- Message : `Les demandes entrantes ne sont pas qualifiées.`
- `detail_score` = `cible:0;role:2;besoin:2;timing:0`
- `donnees_partielles` = `oui`

### T08 — 2/1/0/0 = 20+10+0+0 = 30

- Effectif `80`. Titre `Chargé de marketing`.
- Message : `Bonjour, merci de me recontacter.`
- `detail_score` = `cible:2;role:1;besoin:0;timing:0`

### T09 — 2/2/2/0 = 20+20+30+0 = 70

- Effectif `80`. Titre `Directeur commercial`.
- Message : `Nous devons traiter les leads. Pas de date fixée.`
- `detail_score` = `cible:2;role:2;besoin:2;timing:0`
- Seuil haut inclus.

### T10 — 2/2/0/0 = 20+20+0+0 = 40

- Effectif `100`. Titre `Directrice`. Message : `Bonjour`
- `detail_score` = `cible:2;role:2;besoin:0;timing:0`
- Seuil bas du niveau moyen inclus.

### T11 — doublon

- Même email que T01. Message : `Autre question sur les leads ce mois-ci.`
- Le message contient des mots de score. Il ne doit pas rescorer `L-T01`.

### T12 — UTM

- Société, pays `France`, effectif `100`, titre `Directeur`.
- Message : `Besoin de qualifier les leads ce mois-ci.`
- `utm_source` = `lettre`, `utm_medium` = `cpc`, `utm_campaign` vide.
- `champs_manquants` = `utm_campaign`

### T13 — import, message vide, 2/2/0/0 = 40

- Le message n’est pas obligatoire sur `import_manuel`.
- Effectif `100`. Titre `Responsable`. Message vide.
- `detail_score` = `cible:2;role:2;besoin:0;timing:0`

### T14 — 0/2/2/2 = 0+20+30+30 = 80

- Pays vide. Effectif `100`. Titre `Fondatrice`.
- Message : `Prise de rendez-vous des leads à industrialiser sous 30 jours.`
- `detail_score` = `cible:0;role:2;besoin:2;timing:2`
- `donnees_partielles` = `oui`

### T15 — 2/1/1/2 = 20+10+15+30 = 75

- Effectif `50`. Titre `Consultant`.
- Message : `Merci d'envoyer la documentation. Ce n'est pas urgent, mais ce mois-ci si possible.`
- Après repli, `ce mois` est présent dans « ce mois-ci » et l’emporte sur `pas urgent`.
- `detail_score` = `cible:2;role:1;besoin:1;timing:2`

### T16 et T17 — bornes incluses, 2/2/0/0 = 40

- T16 : effectif `20`, titre `Directrice`, message `Bonjour`.
- T17 : effectif `2000`, titre `Directeur`, message `Bonjour`.
- `detail_score` = `cible:2;role:2;besoin:0;timing:0`

### T18 et T19 — bornes exclues

- T18 : effectif `19`, titre `Directrice`, message `Bonjour`.
- T19 : effectif `2001`, titre `Directeur`, message `Bonjour`.

### T20 — 2/2/2/2 = 100, contrôle de frontière

- Effectif `80`. Titre `Responsable acquisition`.
- Message : `Nous en sommes au stade de la qualification des demandes entrantes ce trimestre.`
- `detail_score` = `cible:2;role:2;besoin:2;timing:2`

### T21 — mot `cv`

- Message : `Voici mon cv pour un échange.`
- Titre `Consultant`. Le rejet candidature intervient avant le score.

### T22 — rejet avant incomplet

- Email vide. Message : `Ceci est une candidature spontanée.`

### T23 — 2/1/1/1 = 20+10+15+15 = 60

- Effectif `80`. Titre `Spécialiste marketing`.
- Message : `Merci pour les informations. Projet pour l'année prochaine.`
- `detail_score` = `cible:2;role:1;besoin:1;timing:1`

### T24 — cible complète, société vide, 2/2/0/0 = 40

- Société vide. Pays `France`. Effectif `80`. Titre `Directeur`.
- Message : `Bonjour`
- `detail_score` = `cible:2;role:2;besoin:0;timing:0`
- `donnees_partielles` = `oui`

### T25 — titre

- Titre `Stagiaire`. Message : `Bonjour`. Pays `France`. Effectif `80`.

## Ce que ce jeu ne mesure pas

Il ne mesure pas un volume, un coût, un délai de traitement réel ni une qualité de modèle. Il vérifie qu’une version applique encore la grille publiée.
