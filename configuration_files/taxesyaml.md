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
