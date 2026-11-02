<!-- docqui: {{VERSION}} | prompt: {{PROMPT_ID}} | atualizado: {{YYYY-MM-DD}} -->
# DATA-MODEL.md
> **Modelo de entidades (negocial)** — índice de entidades, atributos e relacionamentos de negócio, sem camada física de banco. Os modelos detalhados estão em `global/data-models/`; use `PROMPT_DATA_MODEL_negocio` para criá-los ou atualizá-los.

---

## Convenção de nomenclatura

| Camada | Convenção | Exemplo | Onde aparece |
|---|---|---|---|
| Entidade | PascalCase singular, português | `ModeloEmail` | **data-models/[dominio].md** (cabeçalho), "Modelos por domínio" |
| Label PO | Português, title case, sem jargão | `Nome completo` | N3 (campos), Gherkin, telas |

---

## Modelos por domínio

<!--
  Tipo do fragmento: `entidades` (tabelas relacionais — padrão) ou `artefatos`
  (datasets/caches/checkpoints — modelo `data-models/_template-artefatos.md`,
  típico do arquétipo ml-dados). Um domínio pode ter os dois fragmentos.
-->

| Domínio | Arquivo | Tipo | Entidades/Artefatos |
|---|---|---|---|
| [Nome do Domínio] | [data-models/[dominio].md](./data-models/[dominio].md) | entidades | [Entidade A, Entidade B] |
| [Nome do Domínio] | [data-models/[dominio]-artefatos.md](./data-models/[dominio]-artefatos.md) | artefatos | [Artefato A, Artefato B] |

---


---

## Relacionamentos

```
[Entidade A] 1──N [Entidade B]    ([descrição da relação])
```

---

## Relacionamentos de seleção (comboboxes)

Os campos de seleção dos N3 referenciam entidades deste modelo pelo nome de negócio. Registre aqui a entidade e o atributo exibido; nomes físicos, consultas e operações de API ficam para o desenvolvimento.

| Campo (Label PO) | Entidade de origem | Atributo exibido (Label PO) | Critério de seleção |
|---|---|---|---|
| [campo de negócio] | [Entidade] | [atributo de negócio] | [restrição de negócio] |

---

## Arquivos Lógicos (APF)

> Registro central de ALIs e AIEs do sistema.
> Mantido via atualização dos fragmentos `global/data-models/[dominio].md`.
> O Docqui não faz a contagem: as funções de dados são dimensionadas pelo agente de contagem, fora do Docqui, conforme `global/SIZING.md`, que registra os resultados nas tabelas abaixo.

### ALIs — Arquivos Lógicos Internos

| ALI | Domínio | Entidades constituintes | RLR | DER | Complexidade | PF | Data |
|---|---|---|---|---|---|---|---|
| [Nome] | [Domínio] | [Entidade] | 1 | [N] | a definir ⚠️ | a definir | [AAAA-MM-DD] |

**Total ALIs: a definir** *(dimensionado pelo agente de contagem, fora do Docqui / SIZING.md)*

### AIEs — Arquivos de Interface Externa

| AIE | Sistema externo | Entidades / estruturas usadas | RLR | DER | Complexidade | PF | Data |
|---|---|---|---|---|---|---|---|
| [Nome] ⚠️ | [sistema externo] | [estrutura usada] | a definir | a definir | a definir | a definir | [AAAA-MM-DD] |

**Total AIEs: a definir**

---

> ℹ️ **Como manter esta seção:** a opção DM registra entidades e relacionamentos, sem dimensionar funções de dados. As tabelas de ALIs/AIEs são preenchidas e atualizadas pelo agente de contagem, fora do Docqui.
