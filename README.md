# PHP Wrapper for eBoekhouden SOAP server

This package makes it easy to connect to your eBoekhouden Accounting system.

## API Documentation
The implementation is based on version 4.9 of the SOAP documentation found here: 
https://secure.e-boekhouden.nl/handleiding/Documentatie_soap.pdf

## Setup
Login to your account on eBoekhouden and go to Beheer -> Instellingen -> API/SOAP.
 
Call the eBoekhoudenConnect class as follow:

```$eBoekhouden = new eBoekhoudenConnect("Username", "SecurityCode1", "SecurityCode2");```

## Usage
After that you can use the class as described below:

### Relations
Here are some usage examples.

#### Find relation
```
$eBoekhouden->getRelationByCode('BAR');
```   

#### Add new relation
```
$relation = new Relation();
$relation->setRelationCode("BAR");
$relation->setCompanyName("Foo Company");

$eBoekhouden->addRelation($relation);
```   

### Mutations
Here are some usage examples.

#### Find mutation
```
$mutation = $api->getMutationsByMutationsByInvoiceNumber($invoice_number);
if (!isset($mutation->Mutaties->cMutatieList)) {
    // Mutation with $invoice_number exists
}
```
#### Add new mutation
```
$mutation = new Mutation();
$mutation->setKind("FactuurVerstuurd"); // FactuurOntvangen, FactuurVerstuurd, FactuurbetalingOntvangen, FactuurbetalingVerstuurd, GeldOntvangen, GeldUitgegeven
$mutation->setDate(date('Y-m-d'));
$mutation->setAccount(1000); // Ledger account code
$mutation->setRelationCode("BAR"); // Must match existing Relation
$mutation->setInvoiceNumber("INV-500"); // Must be unique
$mutation->setTermOfPayment(30); // In days

$mutation->setMutationLines([
    [
        'BedragInvoer' => 100,
        'BedragExclBTW' => 100,
        'BedragBTW' => 21,
        'BedragInclBTW' => 121,
        'BTWCode' => $this->btwCode,
        'BTWPercentage' => 21,
        'TegenrekeningCode' => 1000,
    ]
]);

$eBoekhouden->addMutation($mutation);
```

### Invoices
Please not that you have to use the Mutation functions when synchronising orders/invoices from your own system to e-Boekhouden.nl, not Invoices.

```$eBoekhouden->getInvoices($dateFrom, $toDate, $invoiceNumber, $relationCode)```

| Field | Format | Mandatory |
| --- | --- | :---: |
| $dateFormat | yyyy-mm-dd | Y |
| $toDate | yyyy-mm-dd | Y |
| $invoiceNumber | STRING 50 | N |
| $relationCode | STRING 15 | N |

