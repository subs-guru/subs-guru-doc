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

