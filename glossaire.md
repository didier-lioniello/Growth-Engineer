# Glossaire

Version `2026.11.0`. Auteur : Didier Lioniello.

Les noms de rôles restent en anglais, comme les intitulés de poste. Le reste de la procédure est en français.

| Terme | Sens dans ce dépôt |
| --- | --- |
| Machine GTM | L’ensemble versionné : contrats, blueprints n8n, files, journal. Pas une équipe. |
| GTM Engineer | La personne qui **construit** la machine. Procédure : [workflows/gtm-engineer.md](workflows/gtm-engineer.md). |
| Growth Engineer | La personne qui **fait tourner** la machine : données, automatisation, scoring, acquisition. Procédure : [workflows/growth-engineer.md](workflows/growth-engineer.md). |
| Version | Identifiant unique des contrats publiés ensemble. La version courante est `2026.11.0`. |
| Mise en service | Case du paquet de livraison. `non` : données fictives et cadence documentaire seulement. `oui` : les canaux marqués branchés peuvent recevoir des données réelles. |
| Canal | Une entrée autorisée (formulaire, email, import, acquisition payante). Liste fermée : [contrats/contrat-canaux.md](contrats/contrat-canaux.md). |
| Lead | Une demande entrante normalisée. Une adresse email déjà connue ne crée pas un second lead. |
| Route | Issue du scoring : `route_eleve`, `route_moyen` ou `route_faible`. Le rejet et l’incomplet ne sont pas des routes de score. |
| File humaine | Liste des leads qu’une personne doit traiter. La machine y dépose. Elle n’envoie pas de message. |
| Contrat de scoring | Règles de rejet, points et seuils. Seul le GTM Engineer le modifie, en changeant de version. |
| Données partielles | Il manque la société, le pays, l’effectif ou le titre. Le score peut quand même être calculé si le lead n’est ni rejeté ni incomplet. |
| Écart d’exploitation | Écart que le Growth Engineer corrige sans toucher aux contrats : export oublié, champ recopié depuis le message, file non vue, clôture oubliée. |
| Écart structurel | Écart qui vient de la machine : règle, champ, correspondance de champs, secret d’accès, panne répétée, jeu d’essai faux. Retour au GTM Engineer. |
| Paquet de livraison | Dossier qu’une version doit contenir pour être recevable. Schéma : [contrats/paquet-livraison.md](contrats/paquet-livraison.md). |
| Paquet de changement | Demande écrite du Growth Engineer. Schéma : [contrats/paquet-changement.md](contrats/paquet-changement.md). |
| Jeu d’essai | Fiches fictives et décision attendue. Référence : [contrats/jeu-essai.md](contrats/jeu-essai.md). |
| Blueprint | Plan de nœuds n8n. Ce n’est pas un JSON importable et ce n’est pas un flux déjà exécuté. |
| Clôture | Ligne quotidienne écrite par le Growth Engineer. Schéma : [contrats/journal-cloture.md](contrats/journal-cloture.md). |
| n8n | Outil d’automatisation retenu comme colonne vertébrale. Le reste de l’outillage est interchangeable. |
