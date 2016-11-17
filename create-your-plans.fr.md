

Pour configurer votre système, vous devez créer la configuration complete qui définira vos plans. Le détail de la configuration est disponible dans la documentation *configuration.md*. Vous trouverez ci dessous l'explication globale du fonctionnement des abonnements et des plans.

## Contenu d'un plan

Un plan contient 4 catégories d'options qui décrivent son contenu, et les différentes possibilités d'évolution :

- des options récurrentes :
  - `options-included` : les options fixes (activés au début de période)
  - `options-usage` : les options facturées à la consommation (en fin de période)
- des offres ponctuelles :
  - `oneshot-initial` : des offres facturées avec le plan au début de la première période
  - `oneshot-ondemand` : des offres facturables hors abonnement (non lié à une période)

Les catégories listent des options qui peuvent être intégrées automatiquement ou à la demande (un catalogue). Dans la configuration `plans.yaml` chaque option peut être mise à :

-  `enabled : "true"` Elle est ajouté dans le plan, actif par défaut.
-  `enabled : "false"` Elle est disponible dans le plan, désactivé par défaut.

Si pour l'option, on a la propriété `editable : [enabled] ` on peut modifier le comportement par défaut de `enabled` (c'est à dire supprimer une option qui était prévue en standard dans le plan)

###Options incluses (option-included)

Listes des options incluses automatiquement dans le forfait, ou que l'on peut ajouter (peut être ajouté ou supprimé pendant la composition du plan si autorisé par `editable : [enabled]`)

Chaque option est :

- Est liée à l'abonnement.
- Est explicitement incluse dans le plan.
- Offre récurrente uniquement.


###Options sur consommation (options-usage)

Listes des options qui définissent une part variable dans le forfait, ou que l'on peut ajouter (peut être ajouté ou supprimé pendant la composition du plan si autorisé par `editable : [enabled]`)

Chaque option est :

- Est liée à l'abonnement.
- Est explicitement incluse dans le plan.
- Offre récurrente uniquement.


###Offres initiales (oneshot-initial)

Liste d'offres qui seront facturées une fois lors de l'abonnement (ajouté automatiquement ou manuellement). Cela permet de définir les frais de mise en route ou des options du type formation initiale par exemple.

Chaque option est :

- Est liée à l'abonnement.
- Est explicitement incluse dans le plan.
- Offre ponctuelle uniquement.


###Offres à la demande (oneshot-ondemand):

Définis un catalogue d'offres, offres qui peuvent être facturées pendant la souscription, et librement achetées par le client.

Chaque option est :

- Est liée à l'abonnement (le plan définit juste les offres disponibles par rapport à ce plan)
- N'est pas explicitement incluse dans le plan (l'achat se fait ponctuellement, à la demande du client)
- Aucune récurrence.

Ces options ne sont jamais incluses (toujours `enabled : false`) et doivent toujours être à  `editable : [enabled] ` 



## Système de surcharge

Les options sont automatiquement surchargées par les paramètres définis dans l'option par défaut. La fusion de fait en se base sur la clé.

### Parametrage global

Si vous avez une configuration qui doit d'appliquer globalement, il suffit de la configurer dans le "default". Par exemple, si l'ensemble de vos options utilisent le même système de taxes, il suffit de le configurer dans `defaults:option`.

### Options

Les options définissent un plan, et par défaut correspondent à une ligne sur la facture. Une option ne peut pas être utilisée en dehors d'un plan. Si une option doit pouvoir être ajoutée manuellement à un plan, il faut qu'elle possède la propriété `editable : [enabled]`

Si vous souhaitez rentre disponible une option sur l'ensemble des plans, il suffit de l'ajouter dans sa catégorie dans`default.plans`dans le fichier defaults.yaml



## Tarif du plan

### Calcul

Le prix du plan est la somme des options qui sont comprises dans le plan et qui n'ont pas une `price-impact: false`.

Si le prix du plan est "forfaitaire", c'est-à-dire qu'il ne correspond pas exactement à la somme de ses options, il suffit de créer une option qui correspond à la base du prix et mettre les options qui ne doivent pas impacter le prix, mais sont importantes (les compteurs par exemple) avec la propriété `price-impact: false`.

## Contrat

Un plan est disponible peut être disponible sous plusieurs contrats (par exemple sur des contrats annuels ou mensuels)

Un contrat définit :

- la périodicité
- en combien de fois une période doit être facturé
- si l'abonnement est facturé en début ou en fin de période
- s'il est renouvelé à date anniversaire, en début de mois, en début d'année

### Période

#### Durée

La période d'un contrat définit l'unité minimum du contrat :

- les quantités incluses dans les abonnements de ce contrat se réfèrent à cette période (par exemple, 100 000 emails par an)
- la durée d'engagement est automatiquement supérieure à la période
- le renouvellement se fait toujours pour une période entière.

#### Lien avec la facturation et le paiement

La facturation n'est pas forcément liée à la période : on doit avoir au minimum une facture par période, mais un abonnement annuel peut être facturé mensuellement.

#### Lien avec la durée de l'option

La période de facturation du plan peut être différente de cette de l'option. Dans le cas, le prix de l'option est calculé en tenant compte de cette différence. Par exemple, on peut avoir un plan avec une période de 12 mois, et une option définie sur 1 mois.

Cela permet d'avoir un plan annuel et un plan mensuel, mais qui utilisent les mêmes options.

### Facturation et paiement

Un abonnement peut être facturé en une ou plusieurs factures, en fonction de ce qui est défini dans le contrat.

Plusieurs choix sont possibles, sur une période on peut :

- faire 1 unique facture, mais la payer en plusieurs fois
- faire plusieurs factures, qui seront payées individuellement en une fois
- évidemment, faire une facture, qui sera payée en une fois.

## Counters

Les compteurs sont définis de façon globale sur le plan : si vous avez deux options avec le même compteur, les quantités seront cumulées et appliquées de façon globale au renouvellement du plan.

Pour les facturations des consomations en fin de périodes, l'ensemble des évolutions des compteurs sur la périodes est pris en compte.

## Taxes

Chaque option peut avoir plusieurs taxes (qui s'appliquerons ou pas). A la fin de la facture, le prix de chaque option est cumulé par type de taxes. Le pourcentage de la taxe est appliqué sur ce montant, et ajouté en fin de facture.

S'il y a plusieurs taxes, plusieurs lignes sont affichées en bas de facture.

Les taxes ne s'appliquent pas sur l’option qui n'impacte pas le prix (`price-impact: false`)

## Mise à jour et évolution des plans

Les plans appliqués à un client sont dynamiques : si vous changez un plan dans un fichier de configuration, il sera automatiquement appliqué aux clients (l'effet sera en général lors du renouvellement de la période)

Attention, si vous modifiez le contenu à la baisse, l'abonnement pourrait ne plus correspondre au contrat signé par le client.

Si votre modification ne doit s'applique qu'aux nouveaux abonnements souscrits par les clients, il faut faire une nouvelle version du plan, avec un nouvel identifiant. 

