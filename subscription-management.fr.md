## Modification d'un abonnement

### Changement de plan

Est-ce que c'est un truc où l'on change toutes les options ?

### Ajout d'un option

Lors de l'ajout d'une option :

1. Les factures de la période en cours sont annulés par des avoirs
2. Une nouvelle facture est émise, et tiens compte des prorata
3. L'échéancier de facturation ou de paiement est mis à jour



## Facturation

### Récurente

Pour facturer un client, on utilise les échanchiers de facture pour chercher via la période et le contrat : un client et des abonnements.

Tous les abonnements liés à une période de contrat sont facturés sur la même facture. Cela implique que l'on peut avoir plusieurs abonnements dans le même contrat, mais aussi des abonnements partiels (par exemple pour le changement d'un abonnement en cours de période). La facturation devra gérer la proratisation des prix pour les options et les abonnements qui ne couvent pas l'intégralité de la période.

### Ponctuelle

La facturation d'une prestation ou d'un rechargement ponctuelle se fait dans le cadre un abonnement existant. La facture est rataché à cette abonnement. Cela permet en particulier de bien créditer les bons compteurs.

### Calcul des compteurs

Les compteurs sont initialisés en début de période par abonnement et par période. Attention, si un abonnement a remplacé un autre, les compteurs doivent être cumulés ensemble.