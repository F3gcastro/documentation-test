# Primeiros Passos
Lorem ipsum.

Segue abaixo uma legenda do formato utilizado para descrever cada collection nessa documentação.
# Estrutura da documentação de cada collection
## Nome da collection no NavegaLibs
Descrição do propósito ou ideia por traz da collection (sempre em inglês)

Nome do layout: Nome do layout de migração (sempre em pt-br)

### Schema no NavegaLibs
```
{
  atributo: { type: tipo, required: obrigatoriedade (booleana), outras_propriedades: N},
  outro_atributo: {...},
  mais_um_atributo: {...},
  [...]
}
```

### Modelagem base
(Opcional, somente se aplicável)

O processo de combinação de entidades (arquivos do cliente, mocks, apis... usados na migração) para formar a massa base dessa collection.

### Origem dos campos
(Opcional, somente se aplicável)

- **identificador**: O conjunto de campos que forma o identificador único dessa collection na migração de dados.
- atributo: Descrição da origem na migração da obtenção de cada campo dessa collection.
- outro_atributo: [...]
- mais_um_atributo: [...]