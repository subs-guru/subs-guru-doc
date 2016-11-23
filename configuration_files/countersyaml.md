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
- - "*limit*": the counter is set at the value but this counter didn't allow consumption

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
