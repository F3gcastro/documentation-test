# Schema Base
O schema base é um "schema" padrão para collections simples. 

Seguindo o princípio da reutilização de código, collections baseadas apenas nos campos do schema base não tem uma "definition" própria, economizando código, tempo de manutenção e organização.

---

Quando usamos o schema base, o **identificador** gerado na migração pode ser gerado por uma de duas maneiras:
- Via nome (padrão):
Usando o campo "name" como chave base do id único.
- Customizado:
Um ID customizado, cuja origem será descrita caso a  caso.

---
### Casos de uso comuns
Como collections de configurações ([gerais](../processos/Configurações%20Gerais.md), [prevideniciárias](#)...) geralmente são mais simples, a maioria delas implementam o schema base.

Além disso, muitas dessas collections são baseadas em mock, porque configurações que são dinâmicas no Navega geralmente são chumbadas em sistemas legado. Com isso, uma boa parte das collections [mockadas](Mock.md) são baseadas em schema base.

### Schema atual no NavegaLibs
```
{
  name: { type: String, required: true, maxLength: 50, trim: true },
  isDefault: { type: Boolean, required: true, default: false },
  status: { type: Schema.Types.ObjectId, ref: 'configuration_status', required: true },
}
```