<!-- docqui: 9.1.0 | prompt: MIGRA_AIMM_ESTADO | atualizado: 2026-10-31 -->
---
tipo: ticket
ticket: STRY0013150
ferramenta: ServiceNow fictício
link: ../mentoria/backlog.md#stry0013150--cadastro-de-produtos
titulo: Cadastro de produtos
estado: em-análise
estado-por: "Andro Márcio Correa Louredo"
estado-em: 2026-10-31 16:09 -03:00
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
| [`CAT-PRD-02`: Cadastrar Produto](../modules/catalogo/produtos/f-cadastrar-produto.md) | Catálogo · Produtos | Criação | `CA-1, CA-2` | ✏️ Rascunho |
| [`CAT-PRD-03`: Editar Produto](../modules/catalogo/produtos/f-editar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | — | 📋 A especificar |
| [`CAT-PRD-04`: Inativar Produto](../modules/catalogo/produtos/f-inativar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | `CA-3` | 📋 A especificar |
| [`CAT-PRD-05`: Visualizar Produto](../modules/catalogo/produtos/f-visualizar-produto.md) *(a confirmar no 3A)* | Catálogo · Produtos | Criação | — | 📋 A especificar |
| [`VIT-NAV-01`: Listar Destaques](../modules/vitrine/navegacao/f-listar-destaque.md) | Vitrine · Navegação | Alteração | — | 📋 Especificado |
| [`VIT-NAV-02`: Pesquisar Produtos na Vitrine](../modules/vitrine/navegacao/f-pesquisar-produto-vitrine.md) | Vitrine · Navegação | Alteração | — | ✏️ Rascunho |
| [`VIT-NAV-03`: Visualizar Produto na Vitrine](../modules/vitrine/navegacao/f-visualizar-produto-vitrine.md) | Vitrine · Navegação | Alteração | — | 📋 Especificado |
| [`VIT-DST-02`: Cadastrar Destaque](../modules/vitrine/destaques/f-cadastrar-destaque.md) | Vitrine · Destaques | Alteração | — | 📋 Especificado |
| [`VND-CAR-01`: Adicionar Item ao Carrinho](../modules/vendas/carrinho/f-adicionar-item-carrinho.md) | Vendas · Carrinho | Alteração | — | 📋 Especificado |

As cinco funcionalidades alteradas aplicam o efeito do `CA-3` (produto retirado de venda não aparece mais na loja); o critério fica atribuído a CAT-PRD-04 — Inativar Produto, que o realiza. "Retirar de venda" é tratado como inativar o produto, o verbo que a loja já usa em ADM-USR-04 — Inativar Usuário.

## Artefatos impactados

O changeset abaixo é a previsão do analista, ainda sem o aval da PO. A AIMM foi preenchida depois de já existirem na branch o N2 de Produtos, o N3 de CAT-PRD-02 — Cadastrar Produto, a Situação do Produto no modelo e a mensagem `CAT_PRODUTO_DUPLICADO`: esses artefatos são propostas sujeitas ao mesmo aval, e nenhum deles constitui aprovação. As linhas derivadas que a análise concluiu que não mudam saíram: `global/data-models/vitrine.md` e `global/data-models/vendas.md` (Destaque e ItemPedido não ganham atributo), `global/FIELD-DICTIONARY.md` (nenhum campo canônico novo), `global/RULES-DICTIONARY.md` (nenhuma regra canônica nova) e `global/NFR.md` (a inativação segue as qualidades vigentes, inclusive a autorização por perfil de SEG-01). No perfil `requisitos` o changeset não leva QA.

| Artefato | Tipo | Operação | Seção | Natureza | O quê | Proveniência |
|---|---|---|---|---|---|---|
| `modules/catalogo/produtos/README.md` | N2 | criar | Todas | funcional | Agrupamento Funcional Produtos, com as cinco funcionalidades, a tela e a matriz de permissões | elicitado |
| `modules/catalogo/produtos/f-pesquisar-produto.md` | N3 | criar | Todas | funcional | Pesquisa por nome, categoria e situação, ponto de entrada do cadastro | elicitado |
| `modules/catalogo/produtos/f-cadastrar-produto.md` | N3 | criar | Todas | funcional | Cadastro em modal, com nome único, preço, quantidade em estoque e categoria; nasce Ativo | derivado: âncora |
| `modules/catalogo/produtos/f-editar-produto.md` | N3 | criar | Todas | funcional | Edição dos dados em modal, com o nome imutável e a devolução à venda pela Situação | elicitado |
| `modules/catalogo/produtos/f-inativar-produto.md` | N3 | criar | Todas | funcional | Retirada de venda, mantendo o produto nos pedidos antigos | elicitado |
| `modules/catalogo/produtos/f-visualizar-produto.md` | N3 | criar | Todas | funcional | Consulta dos dados do produto em modal | elicitado |
| `modules/catalogo/README.md` | N1 | alterar | Agrupamentos Funcionais/Integrações com outros domínios | funcional | Produtos deixa de ser planejado, e a Situação do Produto passa a ser lida pela Vitrine e por Vendas | elicitado |
| `modules/vitrine/README.md` | N1 | alterar | Regras transversais de negócio | funcional | A regra 1 passa a exigir também o produto ativo | elicitado |
| `modules/vitrine/navegacao/f-listar-destaque.md` | N3 | alterar | Regras/Cenários/Critérios de sucesso | funcional | Destaque de produto inativo não aparece na página inicial | derivado: âncora |
| `modules/vitrine/navegacao/f-pesquisar-produto-vitrine.md` | N3 | alterar | Regras/Cenários/Critérios de sucesso | funcional | Produto inativo não é encontrado na busca | derivado: âncora |
| `modules/vitrine/navegacao/f-visualizar-produto-vitrine.md` | N3 | alterar | Regras/Cenários/Comportamento de tela/Critérios de sucesso | funcional | Produto inativo não tem página na loja | derivado: âncora |
| `modules/vitrine/destaques/f-cadastrar-destaque.md` | N3 | alterar | Campos/Regras/Cenários/Critérios de sucesso | funcional | A seleção do destaque oferece só produtos ativos | elicitado |
| `modules/vendas/carrinho/f-adicionar-item-carrinho.md` | N3 | alterar | Regras/Cenários | funcional | Produto inativo não entra no carrinho | derivado: âncora |
| `global/data-models/catalogo.md` | DATA-MODEL | alterar | Produto | dados | Atributo Situação do Produto | derivado: ## Campos → coluna |
| `global/DIAGRAMA-ER.md` | DATA-MODEL | alterar | Catálogo | derivado | Situação do Produto no diagrama, a partir do fragmento negocial | derivado: modelo |
| `global/CONTAGEM-PF.md` | MÉTRICA | alterar | — | derivado | Recontagem pelo CT depois dos N3 e do DATA-MODEL; a contagem da instância segue pendente | derivado: delta de dados |
| `global/MESSAGE-DICTIONARY.md` | MESSAGE-DICT | alterar | Catálogo/Vitrine | funcional | `CAT_PRODUTO_DUPLICADO` e a ampliação de `VIT_PRODUTO_INDISPONIVEL` para produto inativo | derivado: referência no N3 |
| `prototypes/catalogo/produtos/flow.html` | PROTÓTIPO | criar | — | funcional | Fluxo de Produtos: pesquisa, modais de cadastro, edição e visualização e inativação na linha | derivado: ## Superfície (Modal) |
| `prototypes/vitrine/navegacao/flow.html` | PROTÓTIPO | alterar | — | funcional | Produto inativo fora dos destaques, da busca e da página do produto | derivado: ## Superfície (Tela própria) |
| `prototypes/vitrine/destaques/flow.html` | PROTÓTIPO | alterar | — | funcional | Seleção de produto do destaque só com produtos ativos | derivado: ## Superfície (Tela própria) |
| `prototypes/vendas/carrinho/flow.html` | PROTÓTIPO | alterar | — | funcional | Tentativa de adicionar produto que saiu de venda com a página aberta | derivado: ## Superfície (Tela própria) |

**Derivados de publicação**: regenerar `analise-impacto/AIMM-STRY0013150.html`, `assets/tree.js`, `assets/mapa-data.js`, `rastreabilidade/data.js` e o espelho em `modules/INDEX.md` pelos scripts canônicos, sem edição manual.

**Sem alteração**: a Gestão de Pedidos (VND-PED) continua mostrando o produto inativo, porque o ItemPedido mantém a referência ao Produto e o preço unitário, o que realiza a segunda metade do `CA-3`. Editar Destaque (VIT-DST-03) não muda, porque o produto do destaque é imutável. Excluir Categoria (CAT-CTG-04) não muda, porque o produto inativo continua vinculado à categoria e bloqueia a exclusão pela RC-09 — Registro vinculado não pode ser excluído.

## Alterações em funcionalidades por Agrupamento Funcional

### Produtos

| Funcionalidade | CA-n | Natureza | Mudança | Regras | Cenários |
|---|---|---|---|---|---|
| `CAT-PRD-01`<br>**Pesquisar Produtos** | — | nova | **Inclusão** da pesquisa por nome, categoria e situação. *Antes*: o produto só existia como massa fictícia, sem tela de manutenção. *Agora*: o gestor de catálogo encontra os produtos, ativos ou inativos. | a definir no 3A | a definir no 3A |
| `CAT-PRD-02`<br>**Cadastrar Produto** | CA-1, CA-2 | nova | **Inclusão** do cadastro. *Antes*: não havia cadastro. *Agora*: o gestor informa nome único, preço, quantidade em estoque e categoria, e o produto nasce ativo. | +5 | +12 |
| `CAT-PRD-03`<br>**Editar Produto** | — | nova | **Inclusão** da edição. *Antes*: os dados não podiam ser mantidos. *Agora*: o gestor altera os dados, exceto o nome, e devolve à venda um produto inativo. | a definir no 3A | a definir no 3A |
| `CAT-PRD-04`<br>**Inativar Produto** | CA-3 | nova | **Inclusão** da retirada de venda. *Antes*: não havia como tirar um produto da loja. *Agora*: o produto inativo some da loja e continua nos pedidos antigos. | a definir no 3A | a definir no 3A |
| `CAT-PRD-05`<br>**Visualizar Produto** | — | nova | **Inclusão** da consulta. *Antes*: não havia consulta dos dados na manutenção. *Agora*: o gestor e o administrador veem o produto sem alterá-lo. | a definir no 3A | a definir no 3A |

### Navegação

| Funcionalidade | CA-n | Natureza | Mudança | Regras | Cenários |
|---|---|---|---|---|---|
| `VIT-NAV-01`<br>**Listar Destaques** | — | alterada | **Restrição** dos destaques exibidos. *Antes*: só a categoria inativa ocultava o destaque. *Agora*: o destaque de produto inativo também não aparece. | +1 | +1 |
| `VIT-NAV-02`<br>**Pesquisar Produtos na Vitrine** | — | alterada | **Restrição** do resultado. *Antes*: só a categoria inativa ocultava o produto. *Agora*: o produto inativo também não é encontrado. | +1 | +1 |
| `VIT-NAV-03`<br>**Visualizar Produto na Vitrine** | — | alterada | **Restrição** da página. *Antes*: só o produto de categoria inativa ficava sem página. *Agora*: o produto inativo também mostra "Este produto não está disponível na loja." | +1 | +1 |

### Destaques

| Funcionalidade | CA-n | Natureza | Mudança | Regras | Cenários |
|---|---|---|---|---|---|
| `VIT-DST-02`<br>**Cadastrar Destaque** | — | alterada | **Restrição** da seleção de produto. *Antes*: a seleção oferecia os produtos de categorias ativas. *Agora*: oferece só os produtos ativos dessas categorias. | +1 | +1 |

### Carrinho

| Funcionalidade | CA-n | Natureza | Mudança | Regras | Cenários |
|---|---|---|---|---|---|
| `VND-CAR-01`<br>**Adicionar Item ao Carrinho** | — | alterada | **Restrição** da inclusão. *Antes*: só o estoque zero impedia a entrada no carrinho. *Agora*: o produto inativo também não entra, inclusive quando saiu de venda com a página aberta. | +1 | +1 |

## Entidades alteradas

Perfil `requisitos`: a alteração é descrita no modelo de entidades de negócio, em Label PO. Não há migração nem tabela ou coluna de banco, e a coluna Migração sai `—`; a camada física fica para o desenvolvimento.

### Produto

| Migração | Tabela / atributo | Natureza | Mudança |
|---|---|---|---|
| — | Situação | atributo incluído | **Inclusão** da lista Ativo e Inativo, obrigatória. *Antes*: o produto não tinha situação própria, e só a categoria inativa o tirava da vitrine. *Agora*: o produto nasce Ativo e, inativo, fica fora de venda, não aparece na loja e continua nos pedidos antigos. |

## Impacto em dicionários

- `global/MESSAGE-DICTIONARY.md`: `CAT_PRODUTO_DUPLICADO`, "Já existe um produto com este nome.", incluída como ⚠️ proposta desta história.
- `global/MESSAGE-DICTIONARY.md`: `VIT_PRODUTO_INDISPONIVEL`, com o texto mantido, passa a valer também para produto inativo e ganha os usos de VIT-NAV-03 — Visualizar Produto na Vitrine e VND-CAR-01 — Adicionar Item ao Carrinho.
- `global/FIELD-DICTIONARY.md`, `global/RULES-DICTIONARY.md` e `global/ERROR-DICTIONARY.md`: nenhum impacto previsto.

## Decisões pendentes

- **Aval do escopo**: a PO precisa avalizar o changeset, inclusive o que já foi escrito na branch antes desta análise.
- **Retirar de venda**: confirmar que é inativar o produto, e não excluí-lo.
- **Volta à venda**: o ticket não pede, mas o N2 propõe devolver o produto à venda em CAT-PRD-03 — Editar Produto. Confirmar.
- **Funcionalidades sem critério**: CAT-PRD-01 — Pesquisar Produtos, CAT-PRD-03 — Editar Produto e CAT-PRD-05 — Visualizar Produto vêm da descrição do ticket ("consultar, editar"), sem `CA-n`. Confirmar que entram.
- **Produto já no carrinho**: decidir o que acontece com o item de um produto inativado depois de entrar no pedido aberto. Se ele for sinalizado ou retirado, entram VND-CAR-02 — Listar Itens do Carrinho e VND-CAR-03 — Alterar Quantidade do Item.
- **Destaque do produto inativo**: decidir se a gestão continua mostrando a situação Vigente, sem o destaque aparecer na loja, ou se sinaliza o produto. Se sinalizar, entram VIT-DST-01 — Pesquisar Destaques e VIT-DST-05 — Visualizar Destaque.
- **Mensagem do carrinho**: confirmar o uso de "Este produto não está disponível na loja." ao adicionar um produto que saiu de venda.
- **Suposições de CAT-PRD-02 — Cadastrar Produto**: confirmar o preço maior que zero, o nome único contra produtos inativos e sem diferenciar maiúsculas e acentos, a escolha de categoria inativa e a Categoria obrigatória, que o `CA-1` não cita.

## Reconciliação

> Preenchida no fechamento (estado `concluído`): declarado × tocado. Escreva o resultado abaixo desta citação.

## Changelog

| Data | Autor | Tipo | Descrição |
|---|---|---|---|
| 2026-10-31 | Docqui Caixa | Ajuste | Front-matter ganha `estado-por` e `estado-em` (9.1.0), preenchidos pelo histórico do git com quem levou a AIMM ao estado atual e quando. |
| 2026-10-31 | Claude | Em análise | Changeset derivado e completado depois do N2 e do primeiro N3, por isso sem aval prévio; acrescenta VIT-DST-02 — Cadastrar Destaque, chegando a 10 funcionalidades; sem aval da PO. |
| 2026-10-31 | Andro Márcio (Docqui Caixa) | AIMM aberta | Ticket fictício `STRY0013150` registrado a partir do backlog da demonstração; 9 funcionalidades mapeadas: 5 criadas em Produtos e 4 alteradas em Navegação e Carrinho. |
| 2026-10-20 | Docqui Caixa | Terminologia | Adoção de AIMM; histórico anterior preservado. |

