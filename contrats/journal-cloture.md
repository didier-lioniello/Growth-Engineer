# Journal de clôture

Version `2026.11.0`. Auteur : Didier Lioniello.

Le Growth Engineer écrit les lignes dans le support de journal nommé par le [paquet de livraison](paquet-livraison.md). Tant que ce support vaut `non choisi`, la phase documentaire se tient dans un tableur local qui ne va pas dans git. Les colonnes restent celles-ci.

Pas de message, pas d’email, pas de nom de personne dans ces tables. Les preuves sont des identifiants.

## Clôture quotidienne

Une ligne par jour ouvré. L’absence de ligne est un écart d’exploitation. On ne rattrape pas un oubli en inventant les compteurs : la ligne de rattrapage porte `rattrapage = oui` et les compteurs réellement relus, ou `compteurs = inconnus` si la relecture est impossible.

| Colonne | Valeurs | Comment la remplir |
| --- | --- | --- |
| `date` | `AAAA-MM-JJ` | Le jour ouvré concerné, pas le jour où l’on écrit en retard |
| `version_machine` | texte | Version lue dans le paquet ce matin-là |
| `phase` | `documentaire` ou `en_service` | `documentaire` tant que `mise_en_service = non` |
| `canaux_branches` | entier | Nombre de lignes `branche = oui` dans le contrat de canaux |
| `n8n` | `absent`, `ok`, `erreurs` | `absent` si aucune instance n’existe. En phase documentaire, cette valeur n’est pas un incident. `ok` si l’instance a été lue et qu’aucune exécution n’est en erreur sur la fenêtre. `erreurs` si au moins une exécution est en erreur |
| `executions_en_erreur` | entier ou `sans_objet` | `sans_objet` seulement si `n8n = absent` |
| `relances` | entier ou `sans_objet` | Nombre de relances transitoires effectuées |
| `structurels` | entier | Nombre d’écarts structurels ouverts dans la journée, y compris les relances qui ont échoué |
| `exploitation` | entier | Nombre d’écarts d’exploitation |
| `recalculs` | entier | Fiches rescorées à la main. `0` en phase documentaire sans lead réel |
| `ecarts_calcul` | entier | Fiches dont le score diffère du contrat |
| `preuves` | identifiants ou `aucune` | `execution_id` / `lead_id` des écarts. `aucune` si les compteurs d’écarts sont à 0 |
| `rattrapage` | `oui` ou `non` | |

En phase documentaire, avec le paquet `2026.11.0` et sans instance : `phase = documentaire`, `canaux_branches = 0`, `n8n = absent`, `executions_en_erreur = sans_objet`, `relances = sans_objet`, `structurels = 0`, `exploitation = 0`, `recalculs = 0`, `ecarts_calcul = 0`, `preuves = aucune`. Cette ligne se répète chaque jour ouvré. Elle ne devient pas un volume commercial.

## Revue hebdomadaire

Une ligne par mardi, en plus des clôtures quotidiennes.

| Colonne | Comment la remplir |
| --- | --- |
| `semaine` | Date du mardi |
| `version_machine` | |
| `clotures_attendues` | Nombre de jours ouvrés de la fenêtre |
| `clotures_presentes` | Nombre de lignes quotidiennes trouvées |
| `canaux_compares` | Pour chaque canal branché : nom, compte export, compte machine, `ecart` ou `egal`. `aucun canal branche` si la liste est vide |
| `cout` | Montant copié depuis l’export, avec la devise écrite dans l’export, ou `cout_non_fourni` |
| `conflits_enrichissement` | Nombre de fiches `conflit_enrichissement = oui` sur 7 jours, ou `0` |
| `contournements_anciens` | Demandes `genant` ouvertes depuis plus de 5 jours ouvrés |
| `demande_id` | Identifiant du paquet de changement du jour, y compris le paquet vide |

On ne calcule pas de retour sur dépense, de coût d’acquisition ni de valeur vie client dans cette revue. Si un coût est présent, il est recopié. Il n’est pas interprété ici.

## Revue mensuelle

Une ligne le premier mardi, en plus de la revue hebdomadaire.

| Colonne | Comment la remplir |
| --- | --- |
| `mois` | `AAAA-MM` |
| `versions_publiees` | Liste des versions livrées pendant le mois, ou `aucune` |
| `demandes_ouvertes` | Identifiants encore sans décision complète |
| `droits_n8n` | `conforme` si seul le GTM Engineer édite les flux, `ecart` sinon, `instance_absente` en phase documentaire sans instance |
| `git_personnel` | `conforme` si aucun commit du mois ne contient d’email réelle, de message de lead ou de secret. Sinon `ecart` |
| `ordre_mois_suivant` | Identifiants des demandes, classés `bloquant` puis `genant` puis `amelioration` |
