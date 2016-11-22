

# config.yaml

## Sections & properties

### service

The **service** section defines your service to name it in the back office :

```yaml
service:
   name: SERVICE NAME
   logo: URL
```

-  service.name : the name of your service
-  service.logo : absolute or relative URL to your logo in a web image format (jpg, png, svg, etc.)

### locale

The **locale** section defines the default value but also what is the country you are billing from (for EU VAT application for example) :

```yaml
locale:
   country: ISO
   lang: iso
```

- locale.country : [ISO 3166-1 code](https://fr.wikipedia.org/wiki/ISO_3166-1) (alpha-2) of your country
- locale.lang : [ISO 639-1 code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) of your language

### lang

The **lang** section lists all languages that need to be supported for your billing. List of key/value pair, the key is the ISO code of the language, the value is the name of the language.

```yaml
lang:
   iso: Description in the language
   iso2: idem
```

### currency

The **currency** section lists the currencies you want to use to bill your customer. First currency is default. The object key is the [ISO 4217 of the currency](https://en.wikipedia.org/wiki/ISO_4217). `money_format`and `lc_monetary` are used to format the price (via [money_format](http://php.net/manual/en/function.money-format.php)).

```yaml
currency:
   iso-code: #key is ISO code
      name: Currency name
      symbol: Currency symbol ($, €, £, etc.)
      money_format: %.2n€
      lc_monetary: full ISO code (en_US, fr_FR, etc.)
   iso-code2:
      ...
```

### invoice


The **invoice** section defines the data used to personalize you customer invoices

```yaml
invoice:
   header: |
      Your full address
      Multiple lines
   logo: URL
   mention: Text displayed just under invoice lines
   footer: Text displayed on invoice footer
```

### customfields

The **customfields** section allows you to add custom fields on customers, subscriptions and invoices entities. This field can be used through the admin interface and the API. To add fields, create a key under the entity section. Five types of custom fields are availaibles : *numeric*, *text*, *textarea*, *boolean* (displayed as a checkbox) and *select* (displayed as a select box).

```yaml
customfields:
   field-type: # choose type : subscriptions|invoices|customers
      "source":
         type: text|textarea|numeric|select|boolean
         label: Title of the field
         help: Description displayed in backoffice
         default: ValueA
         options: #only for select
            valueA: valueA
            valueB: valueB
            etc.: etc.
```

**Note about custom fields**

Pas besoin de champs custom sur les plans/options/counter, il suffit d'ajouter des valeurs dans le YAML et elles seront transmises au service Saas alors des appels API et Webhook. Au service d'en faire ce qu'il veut a sa convenance.

### payment

The payment section lists the available payment mode for your customers. Corresponding payment plugins must be installed. Each payment is identified by its plugin name.

```yaml
payment:
    plugin-payment-code: # see plugin description for the code needed here
        description:
            lang-iso: Public description in this language
            lang-iso2: Public description in this language
        active: true|false
        visibility: user|admin
        config:
            # see plugin description for the keys needed here
```

## Example

```yaml
service:
   name: My Extraordinary service
   logo: https://www.extraordinary-service.tld/logo.png

local:
   country: FR
   lang: fr

lang:
   fr: Français
   en: English

currency:
   eur:
      name: Euro
      symbol: €
      money_format: %.2n€
      lc_monetary: fr_FR
   usd:
      name: Dollar
      symbol: $
      format: $%.2n
      lc_monetary: en_US

invoice:
   header: |
      Extraordinary service, inc.
      Main street
      CA-1234 Silicon beach
      USA
      Tel : +1 555 123 456
   logo: https://www.extraordinary-service.tld/logo.svg
   mention: Our team realy like to serve you !
   footer: Legal mention... bla bla... bla bla bla...

# Custom Fields definition
customfields:
   customers:
      "source":
         type: text
         label: Title of the field
         description: Description displayed in backoffice
      "specific-id":
         type: numeric
         label: Title of the field
         description: Description displayed in backoffice
      "salesrep":
         type: select
         label: Title of the field
         description: Description displayed in backoffice
         options:
            - John
            - Kate
            - Mark
       "partner":
          type: boolean
          label: Title of the field
          description: Description displayed in backoffice
   subscriptions:
      "etc.":
   invoices:
      "...":
payment:
    payline:
        description:
            fr: Carte bancaire
            en: Credit Card
        active: true
        visibility: user
        config:
            publickey: 123456789
            privatekey: AZERTYUIOP
    sepa:
        description:
            fr: Prélévement SEPA
            en: SEPA direct debit
        active: true
        visibility: user
    check:
         description:
             fr: Chèque
             en: Check
         active: true
         visibility: admin
    transfert:
         description:
             fr: Virement bancaire
             en: Bank transfert
         active: true
         visibility: admin
```

# taxes.yaml

## Sections & properties

Lists taxes that can be used in plan configuration. The key is the ID of your tax.

### description

The **description** property gives a name to this tax (used on invoice)

```yaml
    description:
      fr: TVA
      en: VAT tax
```

### excludeeuvat

The **excludeeuvat** property describes if it's a european tax.

```yaml
   excludeeuvat: true|false
```

If set true, European VAT tax is not applied if :

- the customer is a company that have provided his EU VAT Number
- and is not in the invoice emitter country.

### rate

The **rate** property defines the tax rate used

```yaml
rate: 0.20
```

### apply

The **apply** section defines on which customer this tax will be applied depending of its country.

```yaml
apply:
```

The **behavior** property defines how the tax will be applied :

```yaml
      behavior: everywhere|apply|exclude
```

- `everywhere` : this tax will be applied on all customer
- `apply` : applied only on the countries listed in the country and state sections
- `exclude` : applied on all contries except those listed in the country and state sections

The **countries** and **states** sections describe when the tax will be applied.

```yaml
      countries: # Apply/or exclude on this country
         - iso-code # e.g. FR, US...
         - iso-code2
      states: #  Apply/or exclude on this country+state
         - iso-code # e.g. US-MA
```

## Example

European VAT on goods, applied on all european countries, 20% tax.

```yaml
"tva20":
    description:
      en: VAT tax
      fr: TVA
   apply:
      behavior: apply
      country: [BE, BG, CZ, DK, DE, EE, IE, EL, ES, FR, HR, IT, CY, LV, LT, LU, HU, MT, NL, AT, PL, PT, RO, SI, SK, FI, SE, UK]
   rate: 0.2
   excludeeuvat: true
```

# counters.yaml

**counters.yaml** defines all the counters that can be used on options.

## Sections & properties

### description

```yaml
"counter-id" :    # Unique ID for a counter
    description:
        - lang-iso: description # iso code of the language : description
        - lang-iso2: description
```

### icon

The **icon** property defines a pictogram displayed of this counter. The value is the name of the [font awesome](http://fontawesome.io/icons/) chartername

```yaml
   icon: email #Font awesome icon ID
```

### unit

The **unit** section describes the name of the unit, in each language, in its singular and plural forms :

```yaml
  unit:
      lang-iso: # iso code of the language
         s: unit # singular form
         p: units # plural form
      lang-iso2:
         s: unit
         p: units
```

### type

The **type** property sets how the counter works :

```yaml
     type: cumulative
```

- "*cumulative*" : at each period beginning (and renewal), the value is added to the counter
- "*reset*": the counter is set at the value on each beginning (and renewal)

### quantity-prorated

The **quantity-prorated** property defines the behavior when an option is added during a period. If *quantity-prorated* is *true*, the option quantity is time prorated.

```yaml
    quantity-prorated: true  # quantité proratisé (sauf si dans le cadre d'un oneshot)
```

For example, a counter that defines the maximum number of users will not be prorated : as soon as the customer add the option, he has the new count of users.

Another example : a counter that defines how many emails can be sent during the period needs to be prorated. The customer will have a full counter at the period renewal.

### counter-update

Defines how the counter is updated : `consumption` (each consumption is sent for invoicing), `fullcounter` (electricity meter type : the counter is never reseted) or `period-consumption` (the counter is reseted one each period begin)

```yaml
    counter-update: consumption|fullcounter|period-consumption
```

## Example

3 counters :

```yaml
 counters:
    "user":
       description:
          fr: Utilisateurs
          en: Users
       icon: user
       unit:
          fr:
             s: utilisateur
             p: utilisateurs
          en:
             s: user
             p: users
       type: limit
       quantity-prorated: false
    "email":
       description:
          fr: E-mails
          en: Emails
       icon: envelope #Font awesome icon ID
       unit:
          fr:
             s: e-mail
             p: e-mails
          en:
             s: email
             p: emails
       type: limit
       quantity-prorated: true
    "sms":
       description:
          fr: SMS
          en: SMS
       icon: mobile
       unit:
          fr:
             s: SMS
             p: SMS
          en:
             s: SMS
             p: SMS
       type: cumulative
       quantity-prorated: true
```

- user : maximum account that can be created on you service
- email : email consumption included for the period
- sms : sms counter, refilled at the beginning of the period

# options.yaml

Options are parts of a subscription plan. Each option has a unique text id used as key.

Each option is merged with `defaults.option` configured in defaults.yaml. The properties of the option override the defaults.

## Sections & properties

### quantity

Quantity of the option

```yaml
quantity: 1
```

### description

Public description for the option

```yaml
description:
    fr: titre
    en: title
```

### note

Private comment for the option

```yaml
note: |
   this text will be displayed in the backoffice.
   and can be a full description...
```

### price-period

Defines the duration of the price, in month (m), in days (d) or even year (y)

```yaml
price-period: 1m
```

### price

Price in each currency you have defined in config.yaml

```yaml
price:
    eur: 999999
    usd: 999999
```

### prorated

Set *false* if the option price should not be prorated if added in the plan during the period.

```yaml
prorated: false
```

### taxes

Lists of the taxes that will be applied on this option. Each tax can be overload locally in the option.

```yaml
taxes:
    "tax-id":
```

### permissions

Lists of the permissions unlocked by this option in your SAAS service.

```yaml
permissions: [perm-A, perm-B, etc]
```

### service-values

In this section you can add everything you want. It'll not be used for the billing, but you'll be able to access this data though the API.

```yaml
service-values:
    "specific1":
        "iwant": true
        "isay": "why not"
    "specific2":
        etc: ...
```

### counters

Counter that will be impacted by this option. You can add properties that override, or complete the options defined in the counter.

````yaml
counter:
    "counter1":
        value: 999
````

### editable

Defines which value can be overwritten when the plan is created or an option is added.

```yaml
editable: [quantity, description, price] #key name
```

## Example

```yaml
options:
    "young-subs":
        description:
            fr: Abonnement YOUNG
            en: YOUNG Subscription
        note: Comprend dans son prix un ensemble d'options qui doivent être incluses dans le plan
        price:
            eur: 250
            usd: 300
        price-period: 1m
        permissions: [basic]
```

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

# contracts.yaml

Lists all the contract template availaible. A contrat defines how a plan can be subscribe (duration, trial, payment...)

Each contrat has an unique ID. This ID is use in plan configuration to allow the plan to be subscribe in this type of contract.

### billing

The `billing` section defines how the subcription can be charged.

```yaml
billing:
    installment: [1, 2, 6, free]
    term: upfront|endofperiod
    renew: anniversary|week|month|year
    renew-gap: 4 #only if new il month of year, the bill will be issued the 4th of the month
```

#### billing.installment

Lists how the customer can pay his invoice :

- `1` : he have to do 1 payement following the invoice,
- `2`, `3`, `4`, `5`, etc. : the period of the subscription is divided by the number,
- `free` : when the subscription is started, the sales defined a payment schedule freely.

#### billing.term

Defines when the invoice is issued :

- `upfront` : at the start of the period,
- `enofperiod` : at the end.

### renew

The `renew` section defines how the contract will be renewed.

#### renew.start

Defines if all the customer are billed on the same day, matching a `week`, a `month`, or a `year`.

With `anniversary`, the customer is billed each period on the same day he starts.

#### renew.partial-first

Defines if the first period is prorated in case `renew.start` in set to `week`, `month` or `year`.

#### renew.gap

If you decide to bill your customer on a specific day, defines witch day of month of year is used.

### period

Defines the duration of a standard period, in month (m), in days (d), in week(w) or even year (y). The period of the subscription the minimum duration of the plan, and impact the manner of the counters are  managed.

```yaml
period: 99(d|w|m|y)
```

### editable

Defines witch value can be overwriten when the plan is created.

```yaml
editable: [trial.duration, auto-renew, price] #key name
```

## Example

```yaml
contracts:
    "1y":
        description:
            fr: Contrat annuel
            en: Annual contract
        period: 12m # Durée d'une période standard en mois
        auto-renew: true # tacite reconduction
        minimum-duration: 12m #durée minimum d'un abonnement en mois
        billing:
            installment: [1, 2, 3, 6, free]
            term: upfront
            renew: anniversary
        trial:
            duration: 15d #ou 1m
            description:
                fr: Période d'essai
                en: Trial period
        editable: [trial.duration, auto-renew]
    "12m":
        description:
              fr: Contrat annuel avec facturation mensuelle
              en: Annual contract avec facturation mensuelle
        period: 1m # Durée d'une période standard en mois
        auto-renew: true # tacite reconduction
        minimum-duration: 12m #durée minimum d'un abonnement en mois
        #totalduration: 24m # durée totale fixe et maximum
        billing:
              installment: [1]
              term: upfront
              renew: anniversary
        trial:
            duration: 15d #ou 1m
            description:
                fr: Période d'essai
                en: Trial period
        editable: [trial.duration]
      "1m":
          description:
                fr: Contrat mensuel sans engagement
                en: Contrat mensuel sans engagement
          period: 1m # Durée d'une période standard en mois
          auto-renew: true # tacite reconduction
          minimum-duration: 1m #durée minimum d'un abonnement en mois
          #totalduration: 24m # durée totale fixe et maximum
          billing:
                installment: [1]
                term: upfront
                renew: anniversary
```



# discounts.yaml

Lists all the discount code that can be applied on a customer subscription.

### percent

Discount amount in percent of the value of options included in the `apply` list.

### apply

Lists the option categories on which the discount will be applied.

### fixed

Fixed amount value of the discount, in each currency.

### duration

Total duration of the discount, must be a multiple of the subscription period.

### Example

```yaml
discounts:
    "new50":
        description:
            fr: 50% de remise pendant 1 mois
            en: 50% discount during 1 month
        percent: 50
        apply: [options-included, options-usage, oneshot-initial, oneshot-ondemand]
        fixed:
            eur: 10
            usd: 8
        duration: 1m
```

# defaults.yaml

Defines default sections and properties for options, plans and counters.

```yaml
defaults:
    option:
        # all sections and properties availaibles for options
    contract:
        # all sections and properties availaibles for contracts
    plan:
        # all sections and properties availaibles for plans
    counter:
        # all sections and properties availaibles for counters
```

All options, contracts, plans and counter you will define will be merged with the configured defaults (kind of like an heritage).

It's very useful to apply the same taxes on all option for example, and allow to define global configuration.

### Examples

```yaml
defaults:
    # All options will be merge with this default options
    option:
        description: # it's a good practice to define default for each your language, to avoid error
            fr: Manque description sur l'option
            en: Missing description on the option
        price-period: 1m
        price: # it's a good practice to define default price for each your currency, to avoid error
            eur: 999999
            usd: 999999
        taxes: #this taxes will apply on each option
            "tva20":
        editable: [quantity, description, price]

    # All plans will be merger with this default plan
    # todo à voir comment en gere les CGU. Parce qu'elles peuvent avoir à évoluer...
    plan:
        terms-of-service:
            version: 1.0
            url: http://ws.com/tos-1.0.pdf
        description: # it's a good practice to define default for each your language, to avoid error
            fr: Manque description sur l'abonnement
            en: Missing description one the subscription
        options-included: # liste des options incluses dans le plan
        options-usage: # liste des options dont le prix est calculé en fonction de l'usage (en fin de période)
        oneshot-ondemand: # liste des options que l'on peut ajouter au plan, ou facturer

    # All counter will be merger with this default counter (moins utile que sur les autres, mais autant généraliser la logique...
    counter:
        description: # it's a good practice to define default for each your language, to avoid error
            fr: Manque description sur le compteur
            en: Missing description one the counter

```

