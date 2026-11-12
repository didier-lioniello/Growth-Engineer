# Paquet de livraison

Version `2026.11.0`. Auteur : Didier Lioniello.

Le GTM Engineer remet ce paquet au Growth Engineer à chaque version. Le Growth Engineer ne démarre pas une cadence « en service » si les cases de la section Garde ne sont pas toutes à `oui`.

Aucune URL de webhook, aucun jeton, aucun mot de passe. Les noms d’outils, le jour où ils sont choisis, suffisent. Les secrets restent dans le coffre n8n.

## Contenu obligatoire

| Pièce | Emplacement dans cette version |
| --- | --- |
| Périmètre | [perimetre-offre.md](perimetre-offre.md) |
| Dictionnaire | [dictionnaire-donnees.md](dictionnaire-donnees.md) |
| Canaux | [contrat-canaux.md](contrat-canaux.md) |
| Scoring | [contrat-scoring.md](contrat-scoring.md) |
| Jeu d’essai | [jeu-essai.md](jeu-essai.md) |
| Blueprints | [capture](../blueprints/capture-et-normalisation.md), [scoring](../blueprints/scoring-et-routage.md), [journal](../blueprints/journal-et-files.md) |
| Schéma de clôture | [journal-cloture.md](journal-cloture.md) |
| Schéma de demande | [paquet-changement.md](paquet-changement.md) |

Les sept fichiers de contrats et les trois blueprints portent la même version. Une divergence est un refus de réception.

## État des cases

| Case | État `2026.11.0` | `oui` seulement si |
| --- | --- | --- |
| `calcul_papier` | oui | Le jeu d’essai du dépôt contient l’arithmétique. À refaire à la main avant de publier une grille modifiée |
| `replay_n8n` | non | Rejeu sur base sans `@exemple.test`, enrichissement forcé en échec, toutes les issues du jeu atteintes, compte-rendu sans donnée personnelle |
| `perimetre_reel` | non | Le GTM Engineer confirme par écrit que les bornes du périmètre sont celles de l’activité, ou il publie d’autres bornes dans une nouvelle version |
| `conservation` | non | La durée du dictionnaire (24 mois pour les fiches, 90 jours pour le journal technique) est confirmée, ou une autre durée est publiée |
| `mise_en_service` | non | `replay_n8n`, `perimetre_reel` et `conservation` sont `oui`, et le contrat de canaux passe au moins un canal à `branche = oui` dans la même version |
| `messagerie_equipe` | non | Le nom de l’outil de messagerie et le nom du canal interne sont écrits dans le paquet. Pas d’URL secrète |
| `support_journal` | non choisi | Tableur, base ou table n8n, nommé ici |
| `support_crm` | non choisi | Connecteur nommé ici, secret hors git |
| `boite_email` | non | Adresse de la boîte dédiée au canal `email_entrant`, mot de passe hors git |

`mise_en_service = non` est l’état normal de cette publication. La cadence du Growth Engineer existe quand même : c’est la phase documentaire, décrite dans son workflow.

## Interdiction

Cocher `mise_en_service = oui` dans une conversation, sans mettre à jour ce fichier et le contrat de canaux dans la même version, ne compte pas. Le Growth Engineer suit les fichiers, pas un message oral.
