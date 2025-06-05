# Configurações Previdenciárias

### Sumário de collections

- [BeneficiaryTypes](#beneficiarytypes)
- [BenefitGroups](#benefitgroup)
- [BenefitPaymentReceiving](#benefitpaymentreceiving)
- [BenefitTypes](#benefittype)
- [CalculationSeries](#calculationseries)
- [Companies](#companies)
- [CompanyTypes](#companytypes)
- [Entities](#entities)
- [EntityTypes](#entitytypes)
- [InstituteTypes](#institutetypes)
- [InvestmentProfiles](#investmentprofiles)
- [MonetaryUnit](#monetaryunit)
- [MonetaryUnitValue](#monetaryunitvalue)
- [ParticipationClasses](#participationclasses)
- [ParticipationGroup](#participationgroup)
- [ParticipationStatus](#participationstatus)
- [ParticipationTypes](#participationtypes)
- [PaymentMethod](#paymentmethod)
- [PaymentReceiving](#paymentreceiving)
- [PensionAccount](#pensionaccount)
- [PensionAccountTypes](#pensionaccounttypes)
- [PensionTypes](#pensiontypes)
- [Plans](#plans)
- [PlanFormats](#planformats)
- [PlanStatus](#planstatus)
- [PlanTypes](#plantypes)
- [RepresentationTypes](#representationtypes)
- [Rubrics](#rubrics)
- [RubricSupport](#rubricsupport)
- [WaitingPeriods](#waitingperiods)
- [WaitingPeriodTypes](#waitingperiodtypes)

## BeneficiaryTypes

Tipos de beneficiário (dependente, beneficiário legal, indicado...).

Nome do layout: Tipo_Beneficiario

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## BenefitGroup

Grupos de Benefício (renda em reais, prazo certo, pagamento único...). Similaridade não justificada com [PaymentMethods](#paymentmethod) e [PaymentReceiving](#paymentreceiving).

Nome do layout: Grupo_Beneficio

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## BenefitPaymentReceiving

Determinação dos tipos de benefícios possíveis em cada plano, além de parâmetros para estes (ex: se pode pagar renda em reais, qual os prazos mínimo e máximo de anos?). Funciona como uma **representação dos regulamentos de cada plano** dentro do Navega.

A possibilidade de preencher alguns de seus atributos, como as informações de parcelamento de um benefício (installments), é configurada de acordo com o [PaymentReceiving](#paymentreceiving) selecionado.

Nome do layout: Recebimento_Pagamento_Beneficio

Schema no NavegaLibs:

```typescript
{
  plan: {
    type: createExtendedReferenceWithName('plans'),
    required: true,
  },
  benefitType: {
    type: createExtendedReferenceWithName('benefit_types'),
  },
  instituteType: {
    type: createExtendedReferenceWithName('institute_types'),
  },
  paymentReceiving: {
    type: createExtendedReferenceWithName('payment_receiving', {
      paymentReceivingType: { type: String, enum: BenefitPaymentReceivingType },
    }),
  },
  cashRedemption: {
    min: { type: Number, min: 0 },
    max: { type: Number, min: 0 },
    step: { type: Number }, // Intervalo mínimo de variáção
    tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
  },
  installments: {
    installmentType: { type: String, enum: [BenefitPaymentReceivingInstallmentType] },
    min: { type: Number, min: 0 },
    max: { type: Number, min: 0 },
    step: { type: Number }, // Intervalo mínimo de variáção
    tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
  },
  reservePercentage: {
    min: { type: Number, min: 0 },
    max: { type: Number, min: 0 },
    step: { type: Number }, // Intervalo mínimo de variáção
    tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
  },
  fixedAmount: {
    installmentType: { type: String, enum: [BenefitPaymentReceivingFixedAmountType] },
    periods: { type: Number }, // Quantidade de períodos
    tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
  },
  actuarialFactor: {
    type: { type: String, enum: Object.values(ActuarialFactorType) },
    tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
    calculation: {
      rule: {
        ref: { type: Schema.Types.ObjectId, refPath: 'calculation.rule.type' },
        type: { type: String, enum: Object.values(RuleType) },
        name: { type: String, trim: true },
      },
    },
  },
  lifetimeBenefit: {
    tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
  },
  formType: { type: createExtendedReferenceWithName('form_types') },
  calculationReport: {
    rule: {
      ref: { type: Schema.Types.ObjectId, refPath: 'calculationReport.rule.type' },
      type: { type: String, enum: Object.values(RuleType) },
      name: { type: String, trim: true },
    },
  },
  singlePaymentConversion: {
    rule: {
      ref: { type: Schema.Types.ObjectId, refPath: 'singlePaymentConversion.rule.type' },
      type: { type: String, enum: Object.values(RuleType) },
      name: { type: String, trim: true },
    },
  },
  minBenefitAmount: {
    rule: {
      ref: { type: Schema.Types.ObjectId, refPath: 'minBenefitAmount.rule.type' },
      type: { type: String, enum: Object.values(RuleType) },
      name: { type: String, trim: true },
    },
  },
  ruleExtractOption: {
    rule: {
      ref: { type: Schema.Types.ObjectId, refPath: 'extractOption.rule.type' },
      type: { type: String, enum: Object.values(RuleType) },
      name: { type: String, trim: true },
    },
  },
  instituteFullRedemption: {
    installments: {
      deferralTime: { type: Number },
      deferralType: { type: String, enum: Object.values(BenefitPaymentReceivingInstallmentInstituteType) },
      installmentsMaxQuantity: { type: Number },
      description: { type: String },
      tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
    },
  },
  institutePartialRedemption: {
    cashRedemption: {
      min: { type: Number, min: 0 },
      max: { type: Number, min: 0 },
      step: { type: Number },
      tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
    },
    installments: {
      deferralTime: { type: Number },
      deferralType: { type: String, enum: Object.values(BenefitPaymentReceivingInstallmentInstituteType) },
      installmentsMaxQuantity: { type: Number },
      description: { type: String },
      tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
    },
  },
  institutePortability: {
    cashRedemption: {
      min: { type: Number, min: 0 },
      max: { type: Number, min: 0 },
      step: { type: Number },
      tags: [{ type: createExtendedReference<Pick<ITag, '_id' | 'name'>>('tags', pick(['name'], tagDefinition)) }],
    },
  },
}
```

### Origem dos campos

Conforme [Mock](../apoio/Mock.md), exceto os seguintes campos (em suas diversas aparições):

```
tags, calculation, formType e rule
```

Esses campos se relacionam com collections e conceitos que não fazem parte da migração de dados. Com isso, não tem como serem preenchidos.

## BenefitType

Tipos de benefício (aposentadorias, pensões, portabilidades, benefícios de BD...). Por mais que a collection seja voltada para benefícios, foram cadastrados institutos, já que **hoje** ela é considerada como um **ponto de partida para pagamentos na folha**.

Nome do layout: Tipo_Beneficio

### Schema no NavegaLibs

```typescript
{
  name: { type: String, required: true, maxlength: 200, trim: true },
  isRisky: { type: Boolean, required: true },
  pensionDeath: { type: Boolean, required: true },
  groups: [{ type: Types.ObjectId, ref: 'benefit_groups', required: true }],
  singlePayment: { type: Boolean, required: true },
  effectiveDate: new EffectiveDate(),
  temporaryDisablementBenefitValue: {type: { amountOfAverageParticipationSalaries: { type: Number }, }, required: false,}
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

## CalculationSeries

Configurações de bases de cálculo (rubricas utilizadas, séries múltiplas envolvidas...)

Nome do layout: Series_Calculo

### Schema no NavegaLibs

```
{
  name: { type: String, required: true },
  status: { type: createExtendedReference<IConfigurationStatus>('configuration_status', pick(['name'], configurationStatusDefinition)), required: true },
  referenceRubric: { type: createExtendedReference<IRubric>('rubrics', pick(['name'], rubricDefinition)), required: true },
  simpleCalculationSerie: {
    type: [{
      rubric: { type: createExtendedReference<IRubric>('rubrics', pick(['name'], rubricDefinition)), required: true },
      order: { type: Number, required: true },
    }],
    required: false,
  },
  multipleCalculationSerie: {
    type: [{
      name: { type: String, required: true },
      description: { type: String, required: true },
      monthlyRubrics: {
        type: [{
          rubric: { type: createExtendedReference<IRubric>('rubrics', pick(['name'], rubricDefinition)), required: true },
          order: { type: Number, required: true },
        }],
        required: true,
      },
      discountRubrics: {
        type: [{
          rubric: { type: createExtendedReference<IRubric>('rubrics', pick(['name'], rubricDefinition)), required: true },
          order: { type: Number, required: true },
        }],
        required: true,
      },
    }],
    required: false,
  },
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

## Companies

Dados de patrocinadoras/companhias.

Nome do layout: Patrocinadora

### Schema no NavegaLibs

```
{
  companyName: { type: String, required: true, maxLength: 250, trim: true },
  tradeName: { type: String, required: true, maxLength: 250, trim: true },
  document: { type: String, required: true, maxLength: 50, trim: true },
  documentType: { type: Schema.Types.ObjectId, ref: 'document_types', required: true },
  companyType: { type: Schema.Types.ObjectId, ref: 'company_types', required: true, index: true },
  plans: [{ type: Schema.Types.ObjectId, ref: 'plans' }],
  effectiveDate: new EffectiveDate(),
  parent: { type: Schema.Types.ObjectId, ref: 'companies', index: true },
  manager: { type: Schema.Types.ObjectId, ref: 'people' },
  code: { type: String, required: true, maxLength: 10, trim: true },
}
```

### Modelagem base

Uso direto do arquivo "Empregadores" enviado pelo cliente.
A única tratativa é a de completar zeros da esquerda no campo de CNPJ, que são suprimidos no formato enviado (CSV).

### Origem dos campos

- **identificador**: Campo "Empregador" do arquivo utilizado, que contém um código numérico único de cada empregador.
- companyName: Campo "NomeEmpregador" da base.
- tradeName: Idem do companyName (não existe mais de um campo para nome)
- document: Campo "Cnpj" da base.
- documentType: Referência fixa ao [DocumentType](./Configurações%20Gerais.md#documenttypes) "CNPJ", já que esse foi o único documento de empregador enviado pelo cliente.
- companyType: Referência fixa ao [CompanyType](#companytypes) "Patrocinadora", já que não existem na carga bruta atual companhias institutidoras.
- plans: Enviado vazio na carga, devido a referência cíclica com [Plans](#plans)
- effectiveDate: Preenchimentos padrão por não existirem campos correspondentes na carga.
  - Início: Data da carga
  - Fim: Vazio
- parent: Vazio. Não existe preenchimento na carga nem mapeamento nosso.
- manager: idem de parent.
- code: Campo "Empregador", conforme já mencionado.

## CompanyTypes

Tipos de companhias (patrocinadora ou instituidora).

Nome do layout: Tipo_Companhia

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## Entities

Cadastro de EAPCs e EFPCs.

Nome do layout: Entidades

```typescript
{
  /** Razão Social */
  companyName: { type: String, required: true, maxLength: 250, unique: true, index: true, trim: true },
  /** Nome Fantasia */
  tradeName: { type: String, required: true, maxLength: 250, trim: true },
  document: { type: String, required: true, maxlength: 20, unique: true, index: true, trim: true },
  documentType: { type: Schema.Types.ObjectId, ref: 'document_types', required: true },
  type: { type: Schema.Types.ObjectId, ref: 'entity_types', required: true },
  inComplianceWith: { type: Number, required: true, min: 108, max: 109 },
  plans: [{ type: Schema.Types.ObjectId, ref: PLANS_COLLECTION_NAME, required: true }],
  effectiveDate: {
    type: new EffectiveDate(),
    required: true,
  },
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

## EntityTypes

Tipos de entidade de previdência complementar (aberta, fechada)

Nome do layout: Tipo_Entidade

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## InstituteTypes

Tipos de instituto.

Nome do layout: Tipo_Instituto

### Schema no NavegaLibs

```typescript
{
  name: { type: String, required: true, trim: true },
  type: { type: String, required: true, enum: INSTITUTE_TYPE },
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

## InvestmentProfiles

Perfis de investimento.

Nome do layout: Perfil_Investimento

### Schema do NavegaLibs

```typescript
{
  name: { type: String, required: true, maxLength: 150, trim: true },
  description: { type: String, required: true, maxLength: 500, trim: true },
  isPredefined: { type: Boolean, required: true },
  participationClasses: [{ type: createParticipationClassExtendedReference(), required: true }],
  monetaryUnits: {
    type: [{
      monetaryUnit: monetaryUnitDefinition,
      percentage: { type: Number, required: true },
      min: { type: Number, required: true },
      max: { type: Number, required: true },
      effectiveDate: { type: new EffectiveDate(), required: true },
    }],
    _id: false,
    required: true,
  },
}
```

### Modelagem base

Uso do arquivo tratado de PessoaUnificada, com enfoque nos dados obtidos por meio do arquivo bruto do cliente "Migração_Perfil Visão_Multi", combinado com o campo "PerfilInvestimento" do arquivo bruto de "Ativos".

Além disso, identificação de classes de participação através da junção do [mock](../apoio/Mock.md) de Classes de Participação com os campos de "Plano" e "Situação" da PessoaUnificada.

### Origem dos campos

- **identificador**: Teste
- name: Nome do perfil, identificado na coluna "PerfilInvestimento" da Pessoa Unificada.
- description: Idem de name. Não existe um campo próprio na carga.
- isPredefined: Booleano false fixo, já que não existe na carga.
- participationClasses: A agregação que cria a lista de refs para [ParticipationClasses](#participationclasses) usa como base as colunas "Plano" e - a lista de - "Situação" de cada Perfil.
- monetaryUnits: A referência para [MonetaryUnit](#monetaryunit) é feita usando também o campo "PerfilInvestimento", já que o ID desta outra collection é construído da mesma maneira que desta. Os outros campos não tem correspondência na carga, então são preenchidos com dados padrão (percentual: 100, min e max: 0, effectiveDate: data da carga e null)

## MonetaryUnit

Cadastro de unidades monetárias (cotas, unidades previdenciárias...).

Nome do layout: Unidade_Monetaria

### Schema do NavegaLibs

```typescript
{
  name: { type: String, required: true, maxLength: 50, trim: true },
  type: createExtendedReferenceWithName('monetary_unit_types'),
  plan: createExtendedReference<IMonetaryUnit['plan']>('plans', pick(['_id', 'name'], planDefinition)),
}
```

### Modelagem base

Campos de "NomePLano", "Perfil Investimento" e "Plano" do arquivo bruto de "Ativos", através da PessoaUnificada

Além disso, a cota PGA é incluída de maneira mockada no código.

### Origem dos campos

- **identificador**: Combinação de "NomePlano" e "PerfilInvestimento", separando cotas de mesmos tipos de perfis de planos diferentes.
- name: Mesma combinação do identificador.
- type: Referência fixa à [MonetaryUnitType](./Configurações%20Gerais.md#monetaryunittype) "cota", já que todas as MonetaryUnits da carga se encaixam nesse tipo.
- plan: Referência a [Plans](#plans), conforme o campo "Plano" da base.

## MonetaryUnitValue

Séries históricos das [MonetaryUnits](#monetaryunit).

Nome do layout: Valor_Unidade_Monetaria

### Schema do NavegaLibs

```typescript
{
  monetaryUnit: { type: createExtendedReference<IMonetaryUnit>('monetary_units', pick(['_id', 'name', 'plan'], monetaryUnitDefinition)) },
  entranceOrigin: { type: String, required: true, index: true, enum: Object.values(MONETARY_UNIT_VALUE_ENTRANCE_ORIGIN) },
  referenceDate: { type: Date, required: true },
  inclusionDate: { type: Date, required: true, default: Date.now },
  value: { type: Number, required: true },
  variation: { type: Number },
}
```

### Modelagem base

Uso do arquivo bruto de "Cotacao", complementado com um depara que identifica o Plano de cada cota (inclui as colunas de "Plano": código do plano e "NomePlano": nome do plano).

### Origem dos campos

- **identificador**: ID da cota (NomePlano, PerfilInvestimento), em combinação com os valores informados nos atributos "entranceOrigin" e "referenceDate".
- monetaryUnit: Referência a [MonetaryUnit](#monetaryunit), baseada nas colunas "NomePlano" e "PerfilInvestimento".
- entranceOrigin: String fixa "manual", indicando que a série não foi incluída por nenhuma API ou serviço.
- referenceDate: Data a partir da coluna "DataCotacao"
- inclusionDate: Idem de referenceDate
- value: Valor da cota, da coluna "Cotacao"
- variation: Não enviada na migração

## ParticipationClasses

Situações para os diferentes tipos de poarticipação em cada plano.

Nome do layout: Classes_Participacao

### Schema do NavegaLibs

```typescript
{
  plan: { type: createExtendedReferenceWithName('plans'), required: true },
  type: { type: createExtendedReferenceWithName('participation_types'), required: true },
  status: { type: createExtendedReferenceWithName('participation_status'), required: true },
  needs: { type: [String], enum: Object.values(ParticipationClassNeedType) },
}

```

Origem dos campos: [Mock](../apoio/Mock.md)

## ParticipationGroup

Submassas (grupos) de participantes, usadas pelas entidades para controles internos e relatórios específicos.

Nome do layout: Grupo_Participacao

### Schema do NavegaLibs

```typescript
{
  name: { type: String, required: true, maxlength: 100, trim: true },
  plan: { type: createExtendedReference<IParticipationGroup['plan']>('plans', pick(['name'], planDefinition)), required: true },
  participationDeadline: { type: Date },
  effectiveDate: new EffectiveDate(),
}
```

### Modelagem base

Uso do arquivo tratado "GrupoParticipacao", que combina o arquivo bruto do cliente "ListaGrupoParticipantes" com o arquivo tratado de "PessoaUnificada".

Origem dos campos: Conforme arquivo de "GrupoParticipacao"

## ParticipationStatus

Situações em que uma participação pode estar.

Nome do layout: Situacao_Participacao

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## ParticipationTypes

Tipos de participações.

Nome do layout: Tipo_Participacao

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## PaymentMethod

Métodos de pagamento de um benefício. Contém as representações específicas de um [PaymentReceiving](#paymentreceiving), conforme a tabela de exemplo abaixo (PaymentRecieving - 1:n - PaymentMethod):

Nome do layout: Metodo_Pagamento

| metodo_pagamento                            | metodo_recebimento |
| :------------------------------------------ | :----------------- |
| Parcelas Constantes                         | Prazo certo        |
| Parcelas Constantes c/ Fator Atuarial Anual | Prazo certo        |
| Prazo Certo em Reais                        | Prazo certo        |
| Prazo Certo em Reais Decrescente            | Prazo certo        |
| Prazo Certo Recalculo Mensal                | Prazo certo        |
| Renda em Reais                              | Valor fixo         |
| Renda em Reais com Alteração              | Valor fixo         |

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## PaymentReceiving

Formas de receber um benefício. Seus registros são genéricos, ou seja, contém atributos especificando possíveis implementações em [PaymentMethod](#paymentmethod) (acesse o link para visualizar a relação PaymentRecieving - 1:n - PaymentMethod)

Sua característica genérica também é utilizada para confirar o preenchimento dos atributos de [BenefitPaymentReceiving](#benefitpaymentreceiving). Por exemplo: para cadastrar um registro em BenefitPaymentReceiving que se paga em parcelas mensais, em um prazo de 5 a 30 anos, primeiro deve ser selecionado um paymentReceiving que suporte o pagamento parcelado (allowInstallmentPayment true).

Nome do layout: Recebimento_Pagamento

### Schema no NavegaLibs

```
{
  type: { type: String, enum: PaymentReceivingType, required: true },
  paymentReceivingType: { type: String, enum: BenefitPaymentReceivingType },
  name: { type: String, required: true, trim: true },
  status: { type: createExtendedReferenceWithName('configuration_status'), required: true },
  allowCashRedemption: { type: Boolean, required: true }, // Resgate à vista
  allowInstallmentPayment: { type: Boolean, required: true }, // Pagamento parcelado
  allowReservePercentagePayment: { type: Boolean, required: true }, // Pagamento por % de reserva
  allowFixedAmountPayment: { type: Boolean }, // Pagamento por valor fixo
  allowPresetAmountPerPeriod: { type: Boolean }, // Predefinir valor por período
  allowActuarialFactorPayment: { type: Boolean }, // Pagamento com valor atuarial
  isActuariallyCalculated: { type: Boolean }, // Atuarialmente calculado
  planPaymentReceivingType: { type: String, enum: PlanPaymentReceivingType }, // Pagamento CD/BD/Único
  allowRedemptionDeferral: { type: Boolean }, // Diferimento do Resgate
  paymentMethod: createExtendedReference<IPaymentMethod>(PAYMENT_METHODS_COLLECTION_NAME, pick(['name'], BaseConfigurationDefinition)),
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

## PensionAccount

Cadastro de contas previdenciárias, onde é distribuído o dinheiro de participantes ou fundos coletivos, separado por finalidades diversas.

Nome do layout: Conta_Previdenciaria

### Schema no NavegaLibs

```
{
  name: { type: String, required: true, maxLength: 150, trim: true },
  monetaryUnit: createExtendedReferenceWithName('monetary_units'),
  type: { type: createExtendedReferenceWithName('pension_account_types') },
  isRegressivePortable: { type: Boolean, required: true },
  isCollectiveAccount: { type: Boolean, required: true },
  effectiveDate: new EffectiveDate(),
  plan: createExtendedReferenceWithName('plans'),
}
```

### Modelagem base

Uso do arquivo tratado de "Movimentos". Esse arquivo vem da combinação do arquivo bruto do cliente "Movimentos", combinado com a "PessoaUnificada" e dois outros arquivos mapeados com o auxílio do time de negócios:

- Um [de-para](../apoio/De-Para.md) que determina o [tipo de cada conta previdenciária](#pensionaccounttypes)
- Um arquivo de apoio que aponta quais contas são coletivas e quais as cotas que devem ser usadas para cada uma

Além disso, com a collection está modelada em uma relação 1:1 com Plans, as contas são replicadas para todos os planos onde elas aparecem. Com isso, se uma conta existe nos planos A, B e C; serão criados três registros para essa conta, um em cada plano.

### Origem dos campos

- **identificador**: Campos "ContaSeguridade" (nome da conta) e "Plano" (código do plano)
- name: Campo "ContaSeguridade"
- monetaryUnit: Enviado vazio, já podem ser extraídas várias cotas para cada conta na carga
- type: Campo "TipoContaSeguridade", advindo do de-para já mencionado
- isRegressivePortable: Booleando "false" fixo. Esse campo não existe na carga
- isCollectiveAccount: Booleando da coluna "ContaColetiva", do de-para aplicado na base
- effectiveDate: Campo não existente na carga, preenchido com data da carga e null
- plan: Referência a [Plans](#plans) através do campo "Plano"

## PensionAccountTypes

Diferentes tipos de contas previdenciárias (contas de contribuições de participante, contrib. de patrocinadora, fundos coletivos...)

Nome do layout: Tipo_Conta_Previdenciaria

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## PensionTypes

Tipos de pensão.

Nome do layout: Tipo_Pensao

### Schema no NavegaLibs

```typescript
{
  name: { type: String, required: true },
  hasCalculationBasis: { type: Boolean, required: true, default: false },
  hasQuantity: { type: Boolean, required: true, default: false },
  hasPercentage: { type: Boolean, required: true, default: false },
  hasAmount: { type: Boolean, required: true, default: false },
  hasAdjustmentMonth: { type: Boolean, required: true, default: false },
  hasCorrectionIndex: { type: Boolean, required: true, default: false },
  status: { type: createExtendedReferenceWithName('configuration_status'), required: true },
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

## Plans

Planos previdenciários ofertados pela entidade.

Nome do layout: Planos

### Schema do NavegaLibs

```typescript
{
  name: {
    type: String,
    required: true,
    maxlength: 250,
    trim: true,
  },
  documents: [{
    issuedAt: { type: Date },
    document: { type: String, required: true },
    documentType: createExtendedReferenceWithName('document_types', {
      isDefault: { type: Boolean, required: true },
      registryType: { type: String, required: true },
    }),
  }],
  type: { type: createExtendedReferenceWithName('plan_types') }, // plan_types
  format: createExtendedReferenceWithName('plan_formats,'), // plan_formats,
  status: createExtendedReferenceWithName('plan_status'), // plan_status
  entity: createExtendedReference('private_pension_entities', {
    companyName: { type: String, required: true },
    tradeName: { type: String, required: true },
  }),
  parentPlans: [{
    type: Types.ObjectId,
    ref: 'plans',
  }],
  benefitTypes: [new PlanBenefitType()],
  instituteTypes: {
    type: [
      createExtendedReference<IPlanInstituteType>(INSTITUTE_TYPES_COLLECTION_NAME, {
        ...pick(['name', 'type'], instituteTypeDefinition),
        instituteStartDateRule: {
          rule: createRuleRefDefinition(),
        },
        participationClasses: {
          type: [
            createExtendedReference<IPlanInstituteTypeParticipationClass>('participation_classes',
              { ...pick(['status', 'type'], participationClassDefinition), effectiveDate: new EffectiveDate() }),
          ],
        },
      })],
  },
  adjustmentPeriod: { type: String, required: true, maxLength: 10, enum: Object.values(MONTHS) },
  isPrevSonho: { type: Boolean, required: true },
  companies: [{
    company: {
      _id: { type: Types.ObjectId, required: true },
      companyName: { type: String, required: true },
      tradeName: { type: String, required: true },
    },
    wasteAccount: createExtendedReferenceWithName('pension_accounts'),
    effectiveDate: new EffectiveDate(),
  }],
  investmentProfiles: [
    createExtendedReferenceWithNameAndEffectiveDate('investment_profiles'),
  ],
  insuranceCompanies: [
    createExtendedReferenceWithNameAndEffectiveDate('insurance_companies'),
  ],
  operationalCollection: { type: operationalCollectionsDefinition, required: false },
  operationalBenefit: OperationalBenefitDefinitionSchema,
  effectiveDate: new EffectiveDate(),
  isEnrollmentAvailable: { type: Boolean, default: false },
  participationClassesMigration: {
    type: [createExtendedReference<IParticipationClass>(PARTICIPATION_CLASSES_COLLECTION_NAME, pick(['plan', 'type', 'status'], participationClassDefinition))],
    default: [],
  },
}
```

Objeto ``PlanBenefitType``:

```typescript
{
  benefitType: createExtendedReference<IBenefitType>('benefit_types', pick([
      'name', 'pensionDeath', 'isRisky', 'singlePayment', 'effectiveDate'
    ],
    benefitTypeDefinition)),
    hasAnnualAllowance: { type: Boolean, default: false },
    effectiveDate: new EffectiveDate(),
    benefitStartDateRule: {
      rule: createRuleRefDefinition({ refPath: 'benefitStartDateRule.rule.type' }),
    },
    participationClasses: [{
      type: createExtendedReference('participation_classes', {
        type: createExtendedReferenceWithName('participation_types'),
        status: createExtendedReferenceWithName('participation_status'),
        effectiveDate: new EffectiveDate(),
      }),
    }],
}
```

### Modelagem base

Combinação dos arquivos brutos "PlanosVisão" e "Empregadores", para delimitar as patrocinadoras de cada plano. Além disso, o arquivo tratado de "PessoaUnificada" e o [mock](../apoio/Mock.md) "Classes_Participacao" são adicionados à mistura para identificar os perfis, benefícios, institutos e classes de participação de cada plano.

### Origem dos campos

- **identificador**: Coluna "Plano" da base, que indica o código único desse plano
- name: Coluna "NM_PLANO" da base
- documents: Lista com 1 dict (existe apenas um documento), usando os campos "DOCUMENTO" e "TIPO_DOCUMENTO" da base. Além disso, a effectiveDate consome diretamente do campo effectiveDate da raíz da collection
- type: Ref. estrangeira para [PlanTypes](#plantypes) a partir da coluna "DESCRICAO_TIPO" da base
- format: Referência fixa ao [PlansFormat](#planformats) "Patrocinado", excedo para o plano 399 que é "Instituído".
- status: Referência à [PlanStatus](#planstatus) pela coluna "SITUACAO_PRINCIPAL"
- entity: CPNJ "7205215000198" fixo, que faz referência ao registro da VisãoPrev (cliente atual) em [Entities](#entities)
- parentPlans: Mapeado conforme regulamentos dos planos com auxílio da equipe de negócios, inserido na coluna "PlanosOrigem" do arquivo bruto "PlanosVisão"
- benefitTypes: O objeto "PlanBenefitType" é composto por uma agregação do campo "DescricaoBeneficio", obtido a partir da "PessoaUnificada". Cada um dos outros campos tem uma origem distinta:
  - A ref. à [BenefitType](#benefittype) é composta diretamente por esse campo
  - Os campos "hasAnnualAllowance" e "benefitStartDateRule" são enviados vazios, porque não estão previstos nos dados brutos.
  - A effectiveDate consome diretamente da effectiveDate da raíz da collection
  - As classes de participação são preparadas na modelagem base, através de uma junção entre a situação dos participantes com cada tipo de benefício e as classes de participação de seu mock.
- instituteTypes: Agregação com os institutos e classes de participação dos participantes com cada tipo de benefício, similar ao preenchimento de benefitTypes
- adjustmentPeriod: String fixa "January". Não existe na carga um campo correspondente a este. Além disso, esse atributo deve corresponder aos valores do Enum "MONTHS" do NavegaLibs.
- isPrevSonho: Booleano fixo "false". Não existe na carga indicador de se o plano se encaixa ou não na metodologia "prevSonho"
- companies: Lista de companhias agregada a partir da junção com Empregadores e PessoaUnifica. A referência a [companies](#companies) é feita a partir do campo "Empregador", o "wasteAccount" é enviado null e a effectiveDate consome diretamente a effectiveDate da raíz da collection.
- investmentProfiles: Lista de perfis agregada a partir da junção com PessoaUnificada
- insuranceCompanies: Envio de lista vazia, sem correspond.
- operationalCollection: Não enviado na carga, já que não existe correspondência
- operationalBenefit: idem operationCollection
- effectiveDate: A data de fim de vigência só é preenchida para planos com status diferente de "ATIVO". Além disso, existem duas metodologias para a determinação da vigência:
  - Ao preencher as colunas opcionais "INICIO_VIGENCIA" e "FIM_VIGENCIA" no arquivo de "Planos", essas datas serão consideradas.
  - Se um registro não tiver essas datas, se colhe a data da adesão mais antiga como início e a data do encerramento mais recente como fim.
- isEnrollmentAvailable: idem operationCollection
- participationClassesMigration: idem operationCollection

## PlanFormats

Cadastro de formatos de planos previdenciários (patrocinado ou instituído)

Nome do layout: Formato_Plano

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## PlanStatus

Situações em que um plano pode estar (ativo, encerrado, aprovado...)

Nome do layout: Situacao_Plano

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## PlanTypes

Cadastro de tipos de planos previdenciários (CD, BD, CV...)

Nome do layout: Tipo_Plano

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## RepresentationTypes

Tipos de representação (tutoria, curatela...)

Nome do layout: Tipo_Representacao

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)

## Rubrics

Descrições das transferências financeiras que acontecem no sistema, podendo fornecer justificativas, relatórios e processos específicos para estes pagamentos e descontos.

Nome do layout: Rubricas

### Schema no NavegaLibs

```typescript
{
  number: { type: String, required: true, maxLength: 200, trim: true },
  name: { type: String, required: true, maxLength: 100, trim: true },
  description: { type: String, maxLength: 500, trim: true },
  support: {
    type: new Schema({
      category: {
        type: RubricSupportSchema,
        required: true,
      },
      type: { type: RubricSupportSchema }, // the "type" key is also used by mongoose, so we need to specify the type of "type" this way
      group: RubricSupportSchema,
      paymentOwner: RubricSupportSchema,
    }),
    required: true,
  },
  status: { type: createExtendedReferenceWithName('configuration_status'), required: true },
  months: [{ type: String, required: true, maxLength: 10, enum: MONTHS }],
  order: { type: Number, min: 1 },
  rubricNumberSource: { type: String, maxLength: 500, trim: true },
  accountingTarget: { type: String, maxLength: 500, trim: true },
  rule: {
    ...createRuleRefDefinition(),
    description: { type: String, trim: true, required: false },
  },
  configurations: rubricConfigurationsDefinition,
  participationClasses: [createExtendedReference<IRubric['participationClasses'][0]>('participation_classes',
    pick(['type', 'plan', 'status', 'needs'], participationClassDefinition))],
  judicialPension: {
    discountRubric: { type: createExtendedReferenceWithName('rubrics'), required: false }, // abono
    earningRubric: { type: createExtendedReferenceWithName('rubrics'), required: false }, // provento
  },
}
```

Schema de ``rubricConfigurationsDefinition``

```typescript
{
  isAnnualAllowance: { type: Boolean, required: true, default: false },
  isAdvancePayment: { type: Boolean, required: true, default: false },
  isRetroactiveAccounting: { type: Boolean, required: true, default: false },
  viewPayment: { type: Boolean, required: true, default: false },
  isRequired: { type: Boolean, required: true, default: false },
  isTaxCalculated: { type: Boolean, required: true, default: false },
  isTaxIncomeIndex: { type: Boolean, required: true, default: false },
  isDiscount: { type: Boolean, required: true, default: false },
}
```

### Modelagem base

A base é o arquivo de "RubricaUnificada", que combina as rubricas dos arquivos do cliente de "FichaFinanceira" e "Movimentos", apoiados com um arquivo [mock](../apoio/Mock.md) chamado "RubricasProprias", que existe para e é mantido pelo time de Regras.

Existe um atributo booleano chamado "DaCarga". Esse atributo atesta se essa rubrica vem das RubricasPróprias (false) ou dos arquivos do cliente (true).

### Origem dos campos

- **identificador**: Combinação do número e nome da rubrica.
- number: Campo "numero", com o número da rubrica
- name: Campo "nome", com o nome da rubrica
- description: Campo "nome"
- support: Referências aos 4 tipos de [RubricSupport](#rubricsupport) (category, type, group, payment_responsible)
- status: Referência para [ConfigurationStatus](./Configurações%20Gerais.md#configurationstatus), onde rubricas com a coluna "DaCarga" true são ativas, e false inativas.
- months: O campo mês aparece na RubricaUnificada. A planilha de RubricasPróprias inclui essa coluna. Para RubricasPróprias com esse valor faltando, e para rubrica da carga (que não vem com essa coluna), o valor correspondente a Janeiro é atribuído como valor padrão.
- order: Valor "1" fixo
- rubricNumberSource: Null
- accountingTarget: Envio de string vazia, não previsto na carga
- rule: Dicionário com chaves "ref" e "type", ambos null
- configurations: Todos os campos com booleano "false" fixo, exceto o "isDiscount". Esse campo foi mapeado conforme duas metodologias:
  - Para rubricas próprias, existe a opção de preencher um campo que determina se o isDiscount será true ou false
  - Para rubricas da carga ou próprias com esse campo vazio, é analisada uma somatória das transações de cada rubrica. Rubricas com somatória positiva (pagamento) recebem esse isDiscount false, e rubricas com somatória negativa (desconto) recebem true.
- participationClasses: Não enviado na carga.
- judicialPension: Não enviado na carga.

## RubricSupport

Descrição dos atributos das rubricas: se essa rubrica marca transações de participante ou patrocinador, se elas são de arrecadação ou folha, se são mensais ou de abono, etc...

Nome do layout: Suporte_Rubrica

### Modelagem base

Existe um [mock](../apoio/Mock.md) de SuporteRubrica, que descreve as rubricas e é tanto utilizado em RubricSupport quanto [Rubric](#rubrics).

Em Rubrics, ele é usado linearmente por rubrica, ou seja, cada rubrica ocupa uma linha, com seus suportes nas colunas category, type, group e payment_responsible. Exemplo:

| Numero | Nome                          | category           | type   | group          | payment_responsible     |
| :----- | :---------------------------- | :----------------- | :----- | :------------- | :---------------------- |
| 0      | Imposto de Renda Resgate 3223 | Folha de Pagamento | Mensal | Pagamento      | Pagamento de Benefício |
| 1      | Básica Participante          | Arrecadação      | Mensal | Contribuição | Participante            |
| 2      | Adicional Participante        | Arrecadação      | Mensal | Contribuição | Participante            |
| 2      | Básica Participante          | Arrecadação      | Mensal | Contribuição | Participante            |

Já em RubricSupport, ele é transposto, para que cadaname linha seja ocupada por um suporte, com seu tipo impresso no campo type. Exemplo:

| name                    | type                |
| :---------------------- | :------------------ |
| Folha de Pagamento      | category            |
| Arrecadação           | category            |
| Mensal                  | type                |
| Pagamento               | group               |
| Contribuição          | group               |
| Pagamento de Benefício | payment_responsible |
| Participante            | payment_responsible |

Os planos são agregados a partir dos registros que utilizam essas rubricas, sejam em Arrecadação ou Folha.

### Schema no NavegaLibs

```typescript
{
  name: { type: String, required: true, maxLength: 100, trim: true },
  description: { type: String, required: true, maxLength: 500, trim: true },
  type: { type: String, required: true, enum: Object.values(RubricSupportType) },
  plans: [ createExtendedReference<IRubricSupport['plans'][0]>('plan', pick(['name'], planDefinition)) ],
}
```

Origem dos campos: [Mock](../apoio/Mock.md)

## WaitingPeriods

Aplicações de carências

Nome do layout: Carencia

### Schema no NavegaLibs

```typescript
{
  name: { type: String, required: true, maxLength: 150, trim: true },
  type: createConfigurationExtendedReference('waiting_period_types'),
  status: createExtendedReference<IWaitingPeriod['status']>('configuration_status', pick(['name'], configurationStatusDefinition)),
  plan: {
    type: createExtendedReference('plans', {
      name: { type: String, required: true },
      benefitType: createExtendedReference<IWaitingPeriod['plan']['benefitType']>('benefit_types', pick(['name'], benefitTypeDefinition)),
      instituteType: createExtendedReference<IWaitingPeriod['plan']['instituteType']>(INSTITUTE_TYPES_COLLECTION_NAME, pick(['name'], instituteTypeDefinition)),
    }),
    required: true,
  },
  rule: createRuleRefDefinition(),
  effectiveDate: new EffectiveDate(),
  description: { type: String },
  tags: [{ type: createExtendedReference<IWaitingPeriod['tags'][0]>('tags', pick(['name'], tagDefinition)) }],
  parameter: {
    type: { type: String, required: true, enum: Object.values(WaitingPeriodParameterType) },
    value: { type: Number, required: true },
  },
}
```

Origem dos campos: Conforme [Mock](../apoio/Mock.md), com excessão de campos fora do escopo da migração (rule, tags e parameter)

## WaitingPeriodTypes

Tipos de carência previdenciária (resgate, aposentadoria...)

Nome do layout: Tipo_Carencia

Schema no NavegaLibs: [Schema Base](./Schema%20Base.md)

Origem dos campos: [Mock](../apoio/Mock.md)
