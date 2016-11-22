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
