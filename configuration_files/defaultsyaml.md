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

