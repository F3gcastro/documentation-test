# Configurações Gerais
### Sumário de collections
- [AddressType](#addresstype)
- [BankAccountPurpose](#bankaccountpurpose)
- [BankAccountTypes](#bankaccounttypes)
- [Banks](#banks)
- [Cities](#cities)
- [ConfigurationStatus](#configurationstatus)
- [Countries](#countries)
- [DocumentTypes](#documenttypes)
- [EmailTypes](#emailtypes)
- [EmploymentContractTypes](#employmentcontracttype)
- [DiseaseClassification](#diseaseclassificationlayout)
- [Kinships](#kinships)
- [MailingTypes](#mailingtypes)
- [MonetaryUnitTypes](#monetaryunittypes)
- [PhoneTypes](#phonetypes)
- [ResignationTypes](#resignationtypes)
- [States](#states)
- [TaxRegimeTypes](#taxregimetypes)
- [RecoveryTemporalIndex](#recoverytemporalindex)

## AddressType
Diferentes tipos de endereço, como residencial, comercial, etc...

Nome do layout: Tipo_Endereco

### Schema no NavegaLibs[Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)
## BankAccountPurpose
Diferentes finalidades de conta bancária, como recebimento de benefício, pagamento de empréstimo, etc...

Nome do layout: Finalidade_Conta_Bancaria

### Schema no NavegaLibs[Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)
## BankAccountTypes
Diferentes tipos de conta bancária, como corrente, poupança, etc...

Nome do layout: Tipo_Conta_Bancaria

### Schema no NavegaLibs[Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)
## Banks
Dados de bancos.

Nome do layout: Bancos

### Schema no NavegaLibs
```
{
  name: { type: String, required: true, trim: true, maxLength: 250 },
  code: { type: String, required: true },
  isPartnerBank: { type: Boolean, default: false },
}
```

Origem dos campos: [Mock](../apoio/Mock.md)
## Cities
Dados de cidades.

Nome do layout: Cidades

### Schema no NavegaLibs
```
{
  name: { type: String, required: true, maxLength: 200, index: true, trim: true },
  state: { type: Schema.Types.ObjectId, ref: 'states', required: true }
}
```

### Modelagem base
Extração dos campos 

### Origem dos campos
- identificador: Campos "cidade" e "UF" da entidade de PessoaUnificada
- name: Campo "cidade" de PessoaUnificada
- state: Extenção feita com [States](#states) a partir do campo "UF" da entidade de PessoaUnificada.
## ConfigurationStatus
## Countries
## DocumentTypes
## EmailTypes
## EmploymentContractTypes
## DiseaseClassificationLayout
## Kinships
## MailingTypes
## MonetaryUnitTypes
## PhoneTypes
## ResignationTypes
## States
## TaxRegimeTypes
## RecoveryTemporalIndex