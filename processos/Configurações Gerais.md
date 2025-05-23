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

É diferente de [MailingTypes](#mailingtypes), esta outra tendo relação com tipos ou finalidades de mensagens enviadas via email.

Nome do layout: Email

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)


## EmploymentContractTypes
Tipos de contratos trabalhistas. 

Nome do layout: Tipo_Contrato_Trabalho

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)



## DiseaseClassification
Classificação internacional de doenças (em português, CID).

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
Tipos de relacionamentos e relações de vínculos entre pessoas.

Nome do layout: Parentesco

### Schema no NavegaLibs
[Schema Base](./Schema%20Base.md) com ID customizado baseado no campo "name".

### Modelagem base
Uso da coluna "Parentesco" da entidade PessoaUnificada, após passar por um [de-para](../apoio/De-Para.md).

Além disso, os valores "Avô" e "Avó" são deparados para "oAvo" e "aAvo", para evitar duplicidades na geração do identificador, que não considera acentuação.

### Origem dos campos
- **identificador**: Campo "Parentesco" da pessoa unificada, **considerando** o ajuste avô/avó mencionado.
- name: Campo "Parentesco" da pessoa unificada, **sem considerar** o ajuste avô/avó mencionado.
- isDefault: False (fixo no código)
- status: Referência estrangeira do status ativo - 1 (fixo no código)



## MailingTypes
Tipos de correspondência (no sentido de correspondência virtual, ou seja, mensagens de e-mail).

É diferente de [EmailTypes](#emailtypes), esta outra tendo relação com tipos de endereço de email.

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)



## MonetaryUnitTypes
Tipos de unidades monetárias (cota, real, cruzeiro, índice...)

Nome do layout: Tipo_Unidade_Monetaria

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)



## PhoneTypes
Tipos de telefone (celular, comercial, residencial...)

Nome do layout: Tipo_Telefone

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)



## ResignationTypes
Tipos de encerramento de um vínculo empregatício.

Nome do layout: Tipo_Desligamento

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)



## States
Dados de estados, províncias e outras denominações socioculturais de divisões dentro de nações.

Nome do layout: Estados

### Schema no NavegaLibs
```
{
  name: { type: String, required: true, maxLength: 100, trim: true },
  code: { type: String, required: true, maxLength: 2, trim: true },
  capital: { type: Schema.Types.ObjectId, ref: 'cities' },
  country: { type: Schema.Types.ObjectId, ref: 'countries', required: true },
}
```

### Modelagem base
Usa o campo de "UF" da PessoaUnificada para - por meio de um de-para - enriquecer o dataframe com os campos "NomeUF" e "CodUF".

Além disso, esse depara também transforma estados inválidos no estado "NI - Não Informado".

### Origem dos campos
- **identificador**: Descr
- name: Campo "NomeUF", vindo do de-para de estados aplicado sobre o campo "UF" da PessoaUnificada.
- code: Campo "UF" da PessoaUnificada.
- capital: Referência estendida a [Cities](#cities). Como esse campo causa uma dependência cíclica entre Cities e States - e a chave de States em Cities é mais forte - o atributo de capital não é preenchido na migração.
- country: Referência estrangeira a [Countries](#countries), fixa na migração para ser sempre Brasil ("BRA").


## TaxRegimeTypes
Tipos de regimes tributários (progressivo / regressivo).

Nome do layout: Tipo_Regime_Tributario

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)



## RecoveryTemporalIndex
Armazenamento de índices históricos (INPC, IPCA, Salário Mínimo...) para serem usados como base de cálculos e recálculos do sistema.

Nome do layout: Recuperacao_Indice_Temporal

### Schema no NavegaLibs
```
{
  url: { type: String, required: false },
  code: { type: String, required: false },
  name: { type: String, required: true },
  effectiveDate: new EffectiveDate(),
}
```

Origem dos campos: [Mock](../apoio/Mock.md)