<!-- docqui: 8.3.1 | prompt: PROMPT_AIMM | atualizado: 2026-10-31 -->
---
tipo: ticket
ticket: STRY0013150
ferramenta: ServiceNow fictício
link: ../mentoria/backlog.md#stry0013150--cadastro-de-produtos
titulo: Cadastro de produtos
estado: rascunho
aberta-na-entrega: false
sprint: ""
avalizado-por: ""
aberta-em: 2026-10-31
---

# AIMM STRY0013150

## Descrição do ticket

> Transcrição do ticket fictício do backlog da demonstração; não é um registro do ServiceNow real.

A equipe de vendas precisa manter o catálogo de produtos no próprio site: cadastrar, consultar, editar e retirar de venda os produtos, com nome, preço e quantidade em estoque.

## Critérios de aceite

> **Numeração**: a fonte fictícia numera os critérios; os números CA-n abaixo são os mesmos do backlog.

```gherkin
# ── CA-1 ───────────────────────────────────────────────────
Scenario: Produto cadastrado com os dados obrigatórios
  Given a equipe de vendas está cadastrando um produto
  When informa nome, preço e quantidade em estoque
  Then o produto é cadastrado
  And sem qualquer um dos três o produto não é cadastrado

# ── CA-2 ───────────────────────────────────────────────────
Scenario: Nome do produto não se repete
  Given já existe um produto com um certo nome
  When a equipe tenta cadastrar outro produto com o mesmo nome
  Then o segundo produto não é cadastrado

# ── CA-3 ───────────────────────────────────────────────────
Scenario: Produto retirado de venda some da loja e fica nos pedidos antigos
  Given um produto que consta em pedidos antigos
  When a equipe o retira de venda
  Then ele não aparece mais na loja
  And continua nos pedidos antigos em que consta
```

## Funcionalidades

| Funcionalidade (N3) | Domínio · Agrupamento Funcional | Operação | Critérios cobertos | Status |
|---|---|---|---|---|
| [`CAT-PRD-01`: Pesquisar Produtos](../modules/catalogo/produtos/f-pesquisar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | — | 📋 A especificar |
| [`CAT-PRD-02`: Cadastrar Produto](../modules/catalogo/produtos/f-cadastrar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | `CA-1, CA-2` | 📋 A especificar |
| [`CAT-PRD-03`: Editar Produto](../modules/catalogo/produtos/f-editar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | — | 📋 A especificar |
| [`CAT-PRD-04`: Inativar Produto](../modules/catalogo/produtos/f-inativar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | `CA-3` | 📋 A especificar |
| [`CAT-PRD-05`: Visualizar Produto](../modules/catalogo/produtos/f-visualizar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | — | 📋 A especificar |
| [`VIT-NAV-01`: Listar Destaques](../modules/vitrine/navegacao/f-listar-destaque.md) | Vitrine · Navegação | Alteração | — | 📋 Especificado |
| [`VIT-NAV-02`: Pesquisar Produtos na Vitrine](../modules/vitrine/navegacao/f-pesquisar-produto-vitrine.md) | Vitrine · Navegação | Alteração | — | ✏️ Rascunho |
| [`VIT-NAV-03`: Visualizar Produto na Vitrine](../modules/vitrine/navegacao/f-visualizar-produto-vitrine.md) | Vitrine · Navegação | Alteração | — | 📋 Especificado |
| [`VND-CAR-01`: Adicionar Item ao Carrinho](../modules/vendas/carrinho/f-adicionar-item-carrinho.md) | Vendas · Carrinho | Alteração | — | 📋 Especificado |

As quatro funcionalidades alteradas aplicam o efeito do `CA-3` (produto retirado de venda não aparece mais na loja); o critério fica atribuído a CAT-PRD-04 — Inativar Produto, que o realiza. "Retirar de venda" é tratado como inativar o produto, o verbo que a loja já usa em ADM-USR-04 — Inativar Usuário.

## Artefatos impactados

| Artefato | Tipo | Operação | Seção | Natureza | O quê | Proveniência |
|---|---|---|---|---|---|---|
| `modules/<dom>/<fs>/f-<slug>.md` | N3 | alterar | Campos/Regras/Cenários | funcional | [o que muda] | derivado: âncora |

## Alterações em funcionalidades por Agrupamento Funcional

### [Agrupamento Funcional]

| Funcionalidade | CA-n | Natureza | Mudança | Regras | Cenários |
|---|---|---|---|---|---|
| `SIGLA-SAF-NN`<br>**[Nome da Funcionalidade]** | CA-1 | alterada | **[Verbo]** de [o quê]. *Antes* [como era]. *Agora* [como fica]. | +1 | +2 |

## Entidades alteradas

[`### <Entidade>` com a mudança em Label PO, natureza e contraste — sem ALI/AIE, RLR/DER ou PF; ou "Nenhuma."]

## Impacto em dicionários

- [ou "Nenhum."]

## Decisões pendentes

- [ou "Nenhuma."]

## Reconciliação

> Preenchida no fechamento (estado `concluído`): declarado × tocado. Escreva o resultado abaixo desta citação.

## Changelog

| Data | Autor | Tipo | Descrição |
|---|---|---|---|
| 2026-10-31 | Andro Márcio (Docqui Caixa) | AIMM aberta | Ticket fictício `STRY0013150` registrado a partir do backlog da demonstração; 9 funcionalidades mapeadas: 5 criadas em Produtos e 4 alteradas em Navegação e Carrinho. |
| 2026-10-20 | Docqui Caixa | Terminologia | Adoção de AIMM; histórico anterior preservado. |
