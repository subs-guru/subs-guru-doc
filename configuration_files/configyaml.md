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
