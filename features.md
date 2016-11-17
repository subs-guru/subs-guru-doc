# SUBS.GURU

Recurring billing subscriptions

**Features**:

- Multilingual BackOffice  (English and French)
- Multilingual Plans
- Multicurrency
- Integration via REST API and webhook for events
- Configuration in YAML

**Light** Technical requirements:

- PHP 5.6
- MySQL 5.5

**Organization**:

- Customer
- Plan: subscription template
- Subscription: a plan (possibly customised) for a client
- Contract: group subscriptions for a customer

## Subscription Management

**Recurrency**:

- Per year, month, week or day ...
- Calendar months or years, or renewed on anniversary
- Automatic renewal or not
- Minimum and maximum contract duration
- Trial period

**Pay per use**:

- Metered options
- 3 types: cumulative, limit or reset,
- Prorating quantities or not.

**Option management** :

- A plan can be changed (if allowed)
- You can add options to a defined plan
- You can add setup fees

**Permissions provisionning** on your software.

## Billing Management

**Automatic Invoice Generation** on a schedule defined automatically or manually:

- Payment in installments (2x, 3x, 4x, etc.)
- Upfront invoice or end of period
- Discount nanagement (in percentage or dollars for a limited period or permanent)
- Proratisation when changing an option or subscription

**Combining multiple subscriptions on one invoice.**

**Non recurrent invoice**

**Advanced taxes ** (per state / country and Euro VAT)

**Credit note **

## Payment Management

**Integrated Payment**

- SEPA direct debits (SEPA XML file)
- Manual payments (check, bank transfer...)
- Payline,
- Stripe * (soon)
- Braintree * (soon).

**Independent of invoices and payments**

- For each contract period  you can choose the number of invoice or payment.
- An invoice can be paid in several installments.
- A manual payment can also be assigned to multiple invoices.

**Refunds** 

## Customer Management

- Specific email address for invoices for accounting
- Custom fields