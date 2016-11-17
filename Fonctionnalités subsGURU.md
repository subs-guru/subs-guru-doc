# Fonctionnalités N.S.A

Facturation récurrente d'abonnements

**Fonctionnalités** :

- Backoffice multilangue (anglais et français)
- Configuration des plans multilangues
- Multidevise
- Intégration via API REST et webhook pour les événements
- Configuration par fichier YAML

**Prérequis techniques légers** :

- PHP 5.6
- MySQL 5.5

**Organisation** :

- client
- plan : modèle d'abonnement
- abonnement : un plan (éventuellement personnalisé) pour un client
- contrat : regroupement de plusieurs abonnements pour un même client

## Gestion des abonnements

**Récurences riches** :

- Par an, par mois, par semaine ou en jours...
- Callés sur les mois ou années calendaires, ou renouvellé à date anniversaire
- Renouvellement automatique ou pas
- Durée minimum et maximum d'un contrat
- Gestion de période d'essai

**Gestion des consommables** :

- Facturation à l'usage,
- 3 types : cumulatif, limite ou reset,
- Proratisation des quantités ou pas.

**Gestion des options** :

- Un plan peut être modifié (si autorisé)
- On peut ajouter des options à un plan défini
- On peut ajouter des frais de mise en route facture

**Gestion des permissions ouvertes par l'abonnement** coté application.

## Gestion de la facturation

**Génération automatique des factures** sur un échéancier défini automatiquement ou manuellement :

- Paiement en plusieurs échéances (2x, 3x, 4x, etc.)
- Facture terme échu ou à échoir
- Gestion de remise (en pourcentage ou en euro, pour une durée limité ou permanente)
- Proratisable lors du changement d'une option ou d'un abonnement

**Regroupement de plusieurs abonnements** sur une même facture.

Gestion de la **facturation hors récurence** pour les options ponctuelles

**Gestion perfectionnée des taxes** (par états/pays et  TVA intracommunautaire)

**Gestion des avoirs**

## Gestion des paiements

**Modes de paiement intégrés** :

- Payline,
- Prélévements SEPA (SEPA XML),
- Paiements manuels (chèque, virement...)
- Stripe*,
- Braintree*.

**Gestion indépendantes des factures et des paiements** :

- Pour chaque période de contrat, vous pouvez choisir le nombre de facture ou de paiement.
- Une facture peut être payé en plusieurs échéances. 
- Un paiement manuel peut aussi etre affectés à plusieurs factures.

**Gestion des remboursements**

## Gestion des clients

Adresse email de reception des factures pour la comptabilité

Gestion de champs personnalisés