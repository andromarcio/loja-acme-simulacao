<!-- docqui: 8.3.1 | prompt: PROMPT_AIMM | atualizado: 2026-10-31 -->
---
tipo: ticket
ticket: STRY0004042
ferramenta: ServiceNow fictício
link: ""
titulo: Percentual máximo de desconto da categoria
estado: em-análise
aberta-na-entrega: false
sprint: ""
avalizado-por: ""
aberta-em: 2026-10-31
---

# AIMM STRY0004042

## Descrição do ticket

> Transcrição da descrição do ticket `STRY0004042`, informada pelo analista na sessão; não é um registro do ServiceNow real.

Como gestor de catálogo, quero definir o percentual máximo de desconto de cada categoria, para que a loja tenha registrado até quanto cada grupo de produtos pode ser descontado.

## Critérios de aceite

> **Numeração**: a fonte numera os critérios; os números CA-n abaixo são os mesmos do ticket.

```gherkin
# ── CA-1 ───────────────────────────────────────────────────
Scenario: Percentual informado no cadastro da categoria
  Given o gestor de catálogo está cadastrando uma categoria
  When informa o Percentual máximo de desconto
  Then a categoria é cadastrada com esse percentual

# ── CA-2 ───────────────────────────────────────────────────
Scenario: Percentual informado, alterado ou retirado na edição da categoria
  Given o gestor de catálogo está editando uma categoria
  When informa, altera ou retira o Percentual máximo de desconto
  Then a categoria fica com o novo valor ou sem percentual

# ── CA-3 ───────────────────────────────────────────────────
Scenario: Percentual fora do intervalo de 0 a 100
  Given o gestor de catálogo informou um Percentual máximo de desconto fora do intervalo de 0 a 100
  When tenta salvar a categoria
  Then a categoria não é salva

# ── CA-4 ───────────────────────────────────────────────────
Scenario: Percentual na visualização da categoria
  Given uma categoria
  When ela é visualizada
  Then o Percentual máximo de desconto aparece na visualização

# ── CA-5 ───────────────────────────────────────────────────
Scenario: Percentual próprio de cada categoria
  Given uma categoria com subcategorias
  When o Percentual máximo de desconto da categoria muda
  Then o percentual das subcategorias não muda
```

## Funcionalidades

| Funcionalidade (N3) | Domínio · Agrupamento Funcional | Operação | Critérios cobertos | Status |
|---|---|---|---|---|
| [`CAT-CTG-02`: Cadastrar Categoria](../modules/catalogo/categorias/f-cadastrar-categoria.md) | Catálogo · Categorias | Alteração | `CA-1, CA-3` | ✏️ Rascunho |
| [`CAT-CTG-03`: Editar Categoria](../modules/catalogo/categorias/f-editar-categoria.md) | Catálogo · Categorias | Alteração | `CA-2, CA-5` | ✏️ Rascunho |
| [`CAT-CTG-05`: Visualizar Categoria](../modules/catalogo/categorias/f-visualizar-categoria.md) | Catálogo · Categorias | Alteração | `CA-4` | ✏️ Rascunho |

O `CA-3` também vale na edição, e o `CA-5` também vale no cadastro de subcategoria e na ficha; cada critério fica atribuído uma só vez, à funcionalidade onde o comportamento nasce.

## Artefatos impactados

O changeset abaixo é a previsão do analista, ainda sem o aval da PO, e nenhum artefato foi alterado além desta AIMM. As linhas que a análise concluiu que não mudam saíram: `global/RULES-DICTIONARY.md` (nenhuma regra canônica nova), `global/ERROR-DICTIONARY.md` (o perfil `requisitos` não especifica erros técnicos) e `global/NFR.md` (o percentual segue as qualidades vigentes, inclusive a autorização por perfil de SEG-01). No perfil `requisitos` o changeset não leva QA.

| Artefato | Tipo | Operação | Seção | Natureza | O quê | Proveniência |
|---|---|---|---|---|---|---|
| `modules/catalogo/categorias/f-cadastrar-categoria.md` | N3 | alterar | Campos/Regras/Cenários/Comportamento de tela/Critérios de sucesso/Origem | funcional | Percentual máximo de desconto opcional no cadastro, de 0 a 100, sem herdar o valor da Categoria superior | derivado: âncora |
| `modules/catalogo/categorias/f-editar-categoria.md` | N3 | alterar | Campos/Regras/Cenários/Comportamento de tela/Critérios de sucesso/Origem | funcional | Informar, alterar ou retirar o percentual, sem mudar o das subcategorias | derivado: âncora |
| `modules/catalogo/categorias/f-visualizar-categoria.md` | N3 | alterar | Campos/Cenários/Comportamento de tela/Critérios de sucesso/Origem | funcional | Percentual máximo de desconto na ficha, com "—" quando não houver | derivado: âncora |
| `global/data-models/catalogo.md` | DATA-MODEL | alterar | Categoria | dados | Atributo Percentual máximo de desconto, opcional | derivado: ## Campos → coluna |
| `global/DIAGRAMA-ER.md` | DATA-MODEL | alterar | Catálogo | derivado | Novo atributo no diagrama, a partir do fragmento negocial | derivado: modelo |
| `global/CONTAGEM-PF.md` | MÉTRICA | alterar | Pendências de contagem | derivado | Recontagem pelo CT depois dos N3 e do DATA-MODEL; a contagem da instância segue pendente | derivado: delta de dados |
| `global/FIELD-DICTIONARY.md` | FIELD-DICT | alterar | Percentual | funcional | Entrada detalhada do campo canônico Percentual, que hoje só consta no índice | elicitado |
| `global/MESSAGE-DICTIONARY.md` | MESSAGE-DICT | alterar | Catálogo | funcional | Mensagem `CAT_PERCENTUAL_INVALIDO` para valor fora do intervalo | derivado: referência no N3 |
| `prototypes/catalogo/categorias/flow.html` | PROTÓTIPO | alterar | — | funcional | Percentual máximo de desconto nos modais de cadastro, edição e visualização | derivado: ## Superfície (Modal) |

**Derivados de publicação**: regenerar `analise-impacto/AIMM-STRY0004042.html`, `assets/tree.js`, `assets/mapa-data.js`, `rastreabilidade/data.js`, `global/DIAGRAMA-ER.md`, os índices "Usado em" dos dicionários e o espelho em `modules/INDEX.md` pelos scripts canônicos, sem edição manual.

**Sem alteração**: Pesquisar Categorias (CAT-CTG-01) não muda, porque o ticket não pede o percentual como coluna nem como filtro. Excluir Categoria (CAT-CTG-04) não muda, porque o percentual não cria vínculo. O N1 Catálogo não muda, porque nenhum outro domínio consome o percentual. Cupom de desconto (STRY0013135) e o total do pedido não mudam, porque o ticket pede o registro do percentual, sem aplicá-lo a nenhum preço.

## Alterações em funcionalidades por Agrupamento Funcional

### Categorias

| Funcionalidade | CA-n | Natureza | Mudança | Regras | Cenários |
|---|---|---|---|---|---|
| `CAT-CTG-02`<br>**Cadastrar Categoria** | CA-1, CA-3 | alterada | **Inclusão** do Percentual máximo de desconto. *Antes*: a categoria não tinha percentual. *Agora*: o gestor pode informá-lo, de 0 a 100, e a subcategoria nasce sem herdar o da superior. | +2 | +4 |
| `CAT-CTG-03`<br>**Editar Categoria** | CA-2, CA-5 | alterada | **Inclusão** do Percentual máximo de desconto. *Antes*: não havia percentual a manter. *Agora*: o gestor informa, altera ou retira o percentual, e o valor das subcategorias não muda. | +2 | +5 |
| `CAT-CTG-05`<br>**Visualizar Categoria** | CA-4 | alterada | **Inclusão** do percentual na ficha. *Antes*: a ficha não o mostrava. *Agora*: mostra o percentual da própria categoria, ou "—" quando não houver. | 0 | +2 |

## Entidades alteradas

Perfil `requisitos`: a alteração é descrita no modelo de entidades de negócio, em Label PO. Não há migração nem tabela ou coluna de banco, e a coluna Migração sai `—`; a camada física fica para o desenvolvimento.

### Categoria

| Migração | Tabela / atributo | Natureza | Mudança |
|---|---|---|---|
| — | Percentual máximo de desconto | atributo incluído | **Inclusão** do percentual, opcional, de 0 a 100. *Antes*: a categoria não registrava até quanto seus produtos podem ser descontados. *Agora*: cada categoria tem o próprio valor, sem herança entre superior e subcategorias; as categorias existentes ficam sem percentual. |

## Impacto em dicionários

- `global/FIELD-DICTIONARY.md`: entrada `### Percentual` incluída como ⚠️ proposta desta história, porque o campo canônico só consta no índice e a citação exige a entrada.
- `global/MESSAGE-DICTIONARY.md`: `CAT_PERCENTUAL_INVALIDO`, "Informe um percentual de 0 a 100.", incluída como ⚠️ proposta desta história.
- `global/RULES-DICTIONARY.md` e `global/ERROR-DICTIONARY.md`: nenhum impacto previsto.

## Decisões pendentes

- **Aval do escopo**: a PO precisa avalizar o changeset antes de qualquer N3 ou modelo de entidades mudar.
- **Link do ticket**: o ticket não está em `mentoria/backlog.md` nem traz URL; `link` fica vazio até haver uma referência.
- **Casas decimais**: o ticket só diz "de 0 a 100". A proposta é aceitar até 2 casas decimais, como Valor monetário; confirmar.
- **Percentual vazio e zero**: a proposta é tratá-los como coisas diferentes: vazio significa percentual não definido, e 0 significa que os produtos da categoria não podem ter desconto. Confirmar.
- **Uso do percentual**: o ticket pede o registro. Confirmar que ele não limita nada agora, inclusive o Cupom de desconto (STRY0013135), que trata o desconto no checkout.
- **Herança na subcategoria**: o `CA-5` diz que cada categoria tem o próprio valor. A proposta é que a subcategoria sem percentual não use o da superior nem o mostre na ficha; confirmar.

## Reconciliação

> Preenchida no fechamento (estado `concluído`): declarado × tocado. Escreva o resultado abaixo desta citação.

## Changelog

| Data | Autor | Tipo | Descrição |
|---|---|---|---|
| 2026-10-31 | Claude | AIMM aberta | Ticket fictício `STRY0004042` registrado a partir do texto informado pelo analista; 3 funcionalidades mapeadas e 9 artefatos no changeset; sem aval da PO. |
