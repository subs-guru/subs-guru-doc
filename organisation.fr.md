## Organisation des abonnements pour un client

Un client peut avoir plusieurs abonnements, mais ses abonnements peuvent être organisés de façon différente :

- soit le client ne veut avoir qu'une seule facture pour ses différents abonnements, alors il faut regrouper ses abonnements dans un même contrat. Quand plusieurs abonnements sont liés dans un contrat, ils ont automatiquement les mêmes périodes de facturation, ainsi que le même échéancier.
- soit le client veut avoir des factures séparées (même au même ordre), alors il suffit de faire des contrats différents.

La mise en place d'un abonnement pour un client necessite de choisir un **plan**. Un plan est un modèle d'abonnement, et définit l'intégralité du fonctionnement de l'abonnement.

## Définitions

**Client**

> Une adresse de facturation, une devise, une langue

**Contrat**

> Regroupement d'abonnements facturés en même temps (avec la même périodicité et les même périodes).

**Abonnement**

> Défini le service rendu, avec un début et une fin.

**Plan**

> Modèle d'abonnement qui défini son fonctionnement (à quoi à droit et comment il est facturé)

**Période**

> Lié au contrat, et s'applique à l'ensemble des abonnements d'un même contrat. Avec la même durée, la même date de début et la date de fin

Un client peut avoir plusieurs contrats :
- chaque contrat peut contenir plusieurs abonnements
- chaque abonnement peut avoir plusieurs plans qui se succèdent



## Les périodes

Les abonnements sont basés sur un système de période, ce système est très important à comprendre pour appréhender le fonctionnement global de la facturation des abonnements.

La périodicité définit la durée normale d'une période : par exemple, un abonnement sera annuel ou mensuel. Et de cette périodicité découlent des périodes : le premier mois, le deuxième mois, etc.

Chaque période donne lieu à une facture (ou plusieurs, mais au minimum une). Et la période définit la durée de référence concernant toutes les consommations (par exemple le nombre d'emails envoyés).

Un abonnement sur une période peut avoir plusieurs plans (un plan succédant à un autre, un seul plan est actif dans un abonnement à un instant donné)

## Evenements sur les contrats

Lors du début et la fin de chaque période des opérations spécifiques sont excécutès. Ainsi que lors de changement sur l'un des abonnements du plan.

### Début de la période

Les compteurs de type *cumulative*,*limit* et *reset* sont initialisés.

### Fin de la période

Les compteurs de type *consumption* sont utilisés pour facturer les clients, puis remis à zéro.

** FAUX, DOIT ETRE MIS A JOUR **