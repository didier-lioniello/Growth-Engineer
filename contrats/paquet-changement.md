# Paquet de changement

Version `2026.11.0`. Auteur : Didier Lioniello.

Le Growth Engineer écrit une demande quand un écart structurel apparaît, et chaque mardi pour la revue, même s’il n’y a aucune demande. Le GTM Engineer tranche. Le Growth Engineer n’applique pas lui-même le changement dans n8n ni dans les contrats.

Support : le même outil que le journal d’exploitation, hors git. Ne pas y coller un message de lead, une adresse réelle ni un secret. Citer `lead_id`, `execution_id`, `activite_id`.

## Champs

| Champ | Règle |
| --- | --- |
| `demande_id` | Identifiant unique dans le journal |
| `date` | Jour d’ouverture |
| `version_machine` | Version en vigueur au moment du fait |
| `type` | `canal`, `donnee`, `scoring`, `incident` ou `garde_fou` |
| `urgence` | `bloquant`, `genant` ou `amelioration` |
| `fait` | Ce qui a été observé, en une phrase, sans donnée personnelle |
| `preuve` | Identifiants d’exécution ou de lead. `aucune` seulement pour le paquet vide du mardi |
| `contournement` | `aucun`, ou la description du geste d’exploitation déjà autorisé par le runbook. Un geste hors runbook est lui-même un écart |
| `changement_demande` | La modification de contrat ou de flux souhaitée. Vide si le paquet du mardi ne porte aucune demande |
| `decision` | Rempli par le GTM Engineer : `accepte`, `refuse` ou `reporte` |
| `motif_decision` | Obligatoire si `refuse` ou `reporte` |
| `version_cible` | Obligatoire si `accepte` : le numéro de la version qui portera le changement. Pas un correctif sans numéro |

## Paquet du mardi sans demande

Une ligne suffit :

```text
type = revue
urgence = amelioration
fait = aucun changement demande
preuve = aucune
changement_demande = aucun
```

Le GTM Engineer écrit `decision = accepte` et `motif_decision = rien a publier`. La version ne bouge pas.

## Délais de la version

| Urgence | Comportement du Growth Engineer | Réponse du GTM Engineer |
| --- | --- | --- |
| `bloquant` | Paquet le jour même. Les files concernées sont gelées : on n’invente pas la donnée manquante et on n’envoie rien | Prise en charge le jour ouvré même, au moins pour confirmer le gel |
| `genant` | Contournement du runbook autorisé au plus 5 jours ouvrés, puis la demande devient obligatoire si elle est encore ouverte | Tranché au mardi, ou plus tôt |
| `amelioration` | Aucun contournement. La machine actuelle continue | Tranché au plus tard à la revue du premier mardi |

Un `accepte` sans `version_cible` est une décision incomplète : le Growth Engineer la laisse ouverte et la repose le mardi suivant.
