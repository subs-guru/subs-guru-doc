## Structure logique des données

Un **customer** (peut avoir des champs personnalisés)  :
- peut avoir *plusieurs contrats*
 
---
Un **contrat** défini une relation commerciale avec une client, sur une période donnée
- contient *plusieurs abonnements* (plusieurs abonnements actifs, des abonnements finis ou brouilons) 
- à *plusieurs périodes* (les itérations de période au fur et à mesure)

---
Un **abonnement** défini le détail de ce qui sera facturé au client. (peut avoir des champs personnalisés)
- lié à *plusieurs plans* (un plan est un modèle d'abonnement defini dans les fichiers de configuration .yaml). Il n'y a qu'un seul plan actif à un moment donnée; quand il y a plusieurs plans leur date de début et de fin sont gérés pour faire une continuité (et gere ainsi le changement de plan pour un même abonnement
- peut être lié à *plusieurs compteurs* différents.

---
Une **période** est une itération d'un contrat (durée et début défini par le contrat). La période est gérée automatiquement.
- liée à *une ou plusieurs écheances* de facture. (Une période peut avoir plusieurs facture)

---
Une **échéance de facture** défini quand une facture doit être généré pour une période de contrat.
- liée à *une ou plusieurs* écheances de paiement. (Une facture peut avoir plusieurs paiement)

---
Une **échéance de paiement** défini quand une facture devra être payé.

---
Une **facture** peut avoir des custom fields et est  :
- liée à *une période* si c'est une facture récurente (les ponctuelles ne le sont pas)
- liée à *un contrat* 
- peut être liée à une *échance de facture* (ne l'est pas pour les factures ponctuelles)

---
Un **paiement** est :
- lié à *une ou plusieurs factures* (un paiement peut concerner plusieurs factures et une facture peut avoir plusieurs paiements)
- peut être liée à une *échance de paiement* (ne l'est pas pour les factures ponctuelles)