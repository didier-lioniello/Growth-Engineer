# Contrat de canaux

Version `2026.11.0`. Auteur : Didier Lioniello.

Liste fermée. Un canal absent de ce tableau n’est pas une entrée de la machine. En ajouter un est une nouvelle version, livrée par le GTM Engineer. Le Growth Engineer ne branche pas un outil d’acquisition de son côté.

État de cette version : **aucun canal n’est branché**. `branche = non` partout. C’est l’état normal tant que le [paquet de livraison](paquet-livraison.md) n’a pas `mise_en_service = oui` pour un canal précis.

| `canal` | `branche` | Déclencheur prévu dans n8n | Champs obligatoires en plus du tronc commun | Volume de référence, une fois branché |
| --- | --- | --- | --- | --- |
| `formulaire_site` | non | Webhook ou formulaire n8n | `message` | Nombre de soumissions exportées par l’outil de formulaire, même fenêtre que la clôture |
| `email_entrant` | non | Déclencheur email de la boîte dédiée | `message` | Nombre de messages reçus dans cette boîte, hors indésirables déjà classés par la boîte |
| `import_manuel` | non | Dépôt d’un fichier, une colonne par champ du dictionnaire | aucun champ de message | Nombre de lignes du fichier, moins l’en-tête |
| `acquisition_payante` | non | Webhook ou import dont les UTM sont fournis par l’outil d’acquisition | `utm_source`, `utm_medium`, `utm_campaign` | Nombre de leads comptés par l’outil d’acquisition pour les mêmes UTM et la même fenêtre |

Tronc commun, tous canaux : `canal`, et `email` dès que la fiche n’a pas été arrêtée par un rejet ou un doublon. Voir l’ordre dans le [contrat de scoring](contrat-scoring.md).

## Règles d’exploitation

- `branche = non` : le Growth Engineer n’attend aucun volume. La clôture dit `aucun canal branché`. Il n’invente pas un chiffre.
- `branche = oui` : toute entrée de la fenêtre doit apparaître comme lead ou comme activité de doublon. Un écart de comptage avec l’export du canal est noté le jour même.
- L’export du canal est la source du volume. S’il ne contient pas de coût, la clôture indique `cout_non_fourni`. Personne n’estime un coût.
- Un UTM manquant sur `acquisition_payante` ne se reconstitue pas de mémoire. La fiche passe `incomplet`, champs manquants nommés.
- L’import manuel utilise les noms de colonnes du [dictionnaire](dictionnaire-donnees.md). Une colonne inconnue est un écart structurel : on n’importe pas le fichier « quand même ».

## Modèle de fichier d’import

Colonnes, dans cet ordre, en-tête obligatoire :

```text
email,prenom,nom,societe,pays,effectif,titre,message,utm_source,utm_medium,utm_campaign
```

`canal` ne figure pas dans le fichier : la machine écrit `import_manuel`. Les cellules vides restent vides.
