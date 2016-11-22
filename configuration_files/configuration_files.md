# Configuration

## Files

- **[config.yaml](config.yaml)**
  Your service, supported languages and currencies, invoice parameters and custom fields
- **[taxes.yaml](configuration_files/taxesyaml.md)**
  Taxes that need to be applied on your invoice
- **[counters.yaml](#counters.yaml)**
  Defines the counters you need to use with options (e.g. users included, storage space, etc.)
- **[options.yaml](#options.yaml)**
  Defines the options : description, price, counter, etc.
- **[plans.yaml](#plans.yaml)**
  Defines each plan that can be used as a subscription. Mostly composed by options.
- **[contracts.yaml](#contracts.yaml)**
  Defines how a plan must be subscribed and billed
- **[defaults.yaml](#defaults.yaml)**
  Defines counters, options and plans default values that will be applied on all.




## Convention

All IDs (for plans, options, taxes, counters) must be enclose by double-quote like `"my-counter"`

Name of the key are mandatory.