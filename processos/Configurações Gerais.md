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
- [DiseaseClassification](#diseaseclassification)
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

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)


## BankAccountPurpose
Diferentes finalidades de conta bancária, como recebimento de benefício, pagamento de empréstimo, etc...

Nome do layout: Finalidade_Conta_Bancaria

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)


## BankAccountTypes
Diferentes tipos de conta bancária, como corrente, poupança, etc...

Nome do layout: Tipo_Conta_Bancaria

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

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
Extração dos campos "cidade" e "UF" da entidade de PessoaUnificada.

### Origem dos campos
- **identificador**: Campos "cidade" e "UF" da entidade de PessoaUnificada
- name: Campo "cidade" de PessoaUnificada
- state: Extenção feita com [States](#states) a partir do campo "UF" da entidade de PessoaUnificada.


## ConfigurationStatus
Configurador de situações válidas nas configurações do Navega, dentre eles: Ativo, Inativo e Não Informado.

Nome do layout: Situacao_Configuracao

### Schema no NavegaLibs
```
{
  name: { type: String, required: true, maxLength: 50, trim: true },
  isDefault: { type: Boolean, required: true, default: false },
}
```

Origem dos campos: [Mock](../apoio/Mock.md)



## Countries
Dados de países.

Nome do layout: Paises

### Schema no NavegaLibs
```
{
  name: {
    type: String,
    required: true,
    maxLength: 250,
    unique: true,
    index: true,
    trim: true,
  },
  code: {
    type: String,
    required: true,
    maxLength: 2,
    unique: true,
    index: true,
    trim: true,
  },
  isoCode: {
    type: String,
    required: true,
    maxLength: 3,
    unique: true,
    index: true,
    trim: true,
  },
  taxRate: { type: Number, required: false, min: 0, max: 100 },
}
```

### Modelagem base
Extraído 100% da [API de países do IBGE](https://servicodados.ibge.gov.br/api/v1/localidades/paises), cujos resultados sofrem um leve ETL.

### Origem dos campos
Conforme API mencionada acima.

## DocumentTypes
Tipos de documentos que uma pessoa pode, como CPF, RG, CNH...

Nome do layout: Tipo_Documento

### Schema no NavegaLibs
```
{
  name: { type: String, required: true, maxLength: 50, unique: true, index: true, trim: true },
  isDefault: { type: Boolean, required: true, default: false },
  status: { type: Schema.Types.ObjectId, ref: 'configuration_status', required: true },
  registryType: { type: String, required: true, enum: Object.values(DocumentRegistryType) },
  expiration: {
    unit: { type: String, required: false, enum: ExpirationUnit },
    duration: { type: Number, required: false },
  }
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

Obs: O campo de "expiration" não está sendo carregado.

## EmailTypes
Tipos de email, como pessoal, comercial...

Nome do layout: Email

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)


## EmploymentContractTypes
Tipos de contratos trabalhistas. 

Nome do layout: Tipo_Contrato_Trabalho

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)



## DiseaseClassification
DESCRIÇÃO

Nome do layout: Classificacao_Doencas

### Schema no NavegaLibs
```
{
  name: { type: String, required: true, maxLength: 100, trim: true },
  code: { type: String, required: true, maxLength: 10, trim: true },
}
```

Origem dos campos: [Mock](../apoio/Mock.md)


## Kinships
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr



## MailingTypes
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr



## MonetaryUnitTypes
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr



## PhoneTypes
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr



## ResignationTypes
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr



## States
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr



## TaxRegimeTypes
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr



## RecoveryTemporalIndex
DESCRIÇÃO

Nome do layout: Nome

### Schema no NavegaLibs
```
{

}
```

### Modelagem base
SE APLICAVEL

### Origem dos campos
- **identificador**: Descr
- campo_1: Descr
- campo_2: Descr