# plans.yaml

Lists your subscription plans. Each plan have an ID that must be unique. If you want to make a new version of a plan, add a version in the ID.

## Properties

### description

Describes the plan

```yaml
description:
   iso1: description in first language
   iso2: description in second language
```

### availaible

This section defines the availaibility of the plan. `since` and `until` define an interval. `archived` hide the plan in the admin interface.

```yaml
availaible:
    since: start-date-iso #e.g. 2016-01-01
    until: end-date-iso #e.g. 2017-01-01
    archived: true
```

### color

Defines a color for the plan that be used in interface

```yaml
color: #hex
```

### icon

Defines an icon (from fontawesome) for the plan that will be used in the admin interface

```yaml
color: fontawesomeID
```

### trial

The `trial` section defines the trial period of the plan.

```yaml
trial:
    duration: 99(d|w|m|y)
    description:
        lang-iso: Description
        lang-iso2: Description...
```

#### change-policy

When a plan is remplace by a new plan, defines the policy followed : `free` (the new subscription can also have a free trial period), `continue` (can have an trial period, but will be billed at the last subscription price) or `forbidden` (the new can not have a trial).

This policy overrides the `trial` configuration.

```yaml
trial:
    change-policy : free|continue|forbidden
```

### minimum-duration

Must be a multiple of the period, defines the minimum duration of a customer subscription.

```yaml
minimum-duration: 99(d|m|y)
```

### commitment-policy

When a plan is replaced by a new one in a subscription, defines the policy to follow : `reset` the minimum-duration of the new plan if fully applied, `continue` the minimum-duration of the new plan is adapted to complete with the previous one and `continueif` do the same only if the commitment period defined by mimumduration is the same on the both plan, else do the reset policy

```yaml
commitment-policy: reset|continue|confinueif
```

### totalduration

Fixes the total duration of the subscription for non open-ended plan. Must be a multiple of the period.

```yaml
totalduration: 99(d|w|m|y)
```

### auto-renew

If true the plan is automaticly renew at the end of each period.

```yaml
auto-renew: true|false
```

### group

You can group different plans together

```yaml
group: name
```

### editable

Defines which value can be overwritten when the plan is created.

```yaml
editable: [description, period, minimum-duration, totalduration, auto-renew, trial] #key name
```

### terms-of-service

The `terms-of-service` section defines the TOS applied to the subscription.

```yaml
terms-of-service:
    description:
        lang-iso: description
    version: 99.99 # version number
    url: url # url to access the full version of the TOS
```

### note

Gives informations displayed in the backoffice. Nonpublic informations.

```yaml
note: internal information
```

### service-values

In this section you can add everything you want. It will not be used for the billing, but you will be able to access this data though the API. (same as at option level)

```yaml
service-values:
    "specific1":
        "iwant": true
        "isay": "why not"
    "specific2":
        etc: ...
```

## Options sections

This four sections describe options linked to this plan. Each option can be overloaded in each section, to set a specific price or quantity for example.

An option that is not linked to a plan through one of this four sections can not be applied to a customer.

### options-included

This section defines all the options include **in standard** in the plan. Each option have to be defined in `options.yaml`.

### options-usage

This section defines all the options that are **billed on metered usage** and included **in standard** in the plan. Each option have to be defined in `options.yaml`.

consumable resource.

### oneshot-initial

This section defines all the options included **in standard at the start** of the subscription plan. Each option have to be defined in `options.yaml`.

This options will be billed only one time.

### oneshot-ondemand

This section defines all the options that **can be added** in a subscription. Each option have to be defined in `options.yaml`. No other option can be added in a customer subscription.

## Examples

```yaml
plans:
    "young": # identifiant du plan
        group: SPREAD2013
        period: 12m # Durée d'une période standard en mois
        auto-renew: true # tacite reconduction
        minimum-duration: 12m #durée minimum d'un abonnement en mois
        #totalduration: 24m # durée totale fixe et maximum
        description: # description publique
            fr: Abonnement YOUNG
            en: YOUNG Subscription
        note: Ancien abonnement, utilisé de septembre 2014 à septembre 2016
        terms-of-service:
            description:
                fr: Conditions générales d'utilisation
                en: Term of service (french)
            version: 1.7 # version number
            url: http://www.spreadfamily.fr/cgu.pdf # url to access the full version of the TOS
        billing:
            installment: [1, 2, 6, free]
            term: upfront|endofperiod
            renew: anniversary|week|month|year
            renew-gap: 4 #only if new il month of year, the bill will be issued the 4th of the month
        trial:
            duration: 15d #ou 1m
            description:
                fr: Période d'essai
                en: Trial period
        oneshot-initial: #
            "service10":x
                description:
                    fr: Frais de mises en route
                    en: Setup fee
        options-included: # liste des options incluses dans le plan
            #####
            ##### comment on gere les prix ou pas pour ces options
            "young-subs":
                visible: true # sera visible sur la facture
                editable: [quantity, description, price]
            "profile50k":
                visible: true # sera visible sur la facture
                price-impact: false
            "service":
                quantity: 6 # 1 est la valeur par défaut qui pas de qté
            "sms100":
                quantity: 1
            "user":
                quantity: 2
                visible: false # ne sera pas affiché dans la facture. Ce paramètre passe automatique price-impact à false pour l'option
            "email1k":
                quantity: 50
        options-usage: # liste des options dont le prix est calculé en fonction de l'usage (en fin de période)
            "email1k":
                description:
                    fr: Pack de 1000 emails dépassement
                    en: 1000 emails pack (over included)
                price: # the price defined in the option is override
                    eur: 2
                    usd: 3
                visibility: full # affiche la quantité et le prix
        oneshot-ondemand: # liste des options que l'on peut ajouter au plan, ou facturer
            "young-sponsorship": #id de l'option (créé dans options.yaml)
                    visibility: optional # n'est pas ajouté mais doit être ajouté manuellement, valeur par defaut
            "service": # visibility: optional est par defaut, pas besoin
            "service10":
            "email1k":
            "sms100":
            "sms1000":
            "user":

```
