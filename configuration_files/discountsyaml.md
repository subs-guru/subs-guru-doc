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