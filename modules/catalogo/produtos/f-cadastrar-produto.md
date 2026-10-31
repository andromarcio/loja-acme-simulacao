<!-- docqui: 8.3.1 | prompt: PROMPT_CRUD | atualizado: 2026-10-31 -->
---
id: CAT-PRD-02
agrupamento_funcional: CAT-PRD
dominio: CAT
entidade: Produto
data_model_ref: data-models/catalogo.md#produto
endpoints: []
error_codes: []
depende_de: []
origem:
  tipo: servicenow
  chave: STRY0013150
estado: rascunho
gates:
  requisitos: { aprovado: false, por: "", em: "", pr: "" }
  modelo-dados: { aprovado: false, por: "", em: "", pr: "" }
  testes: { aprovado: false, por: "", em: "", pr: "" }
  codigo: { aprovado: false, por: "", em: "", pr: "" }
contagem:
  pendente: true
  revisada_em: ""
  revisada_ate: ""
---

# Cadastrar Produto
> **Nível 3** - Agrupamento Funcional: Produtos — Domínio: Catálogo - `CAT-PRD-02`

## Descrição

Permite que o gestor de catálogo cadastre um produto com nome único, preço, quantidade em estoque e categoria, além de uma descrição opcional, que passa a existir como ativo no catálogo.

Na Pesquisa de Produtos, o gestor aciona **Cadastrar produto**; no modal, informa os dados e aciona **Salvar**. O produto cadastrado aparece na pesquisa e fica à venda na loja, desde que a sua categoria esteja ativa.

## Regras de negócio

- **RN-01** — O nome do produto é único entre todos os produtos, ativos ou inativos, sem diferenciar maiúsculas, minúsculas e acentos.
- **RN-02** — Todo produto nasce ativo.
- **RN-03** — O preço do produto é maior que zero.
- **RN-04** — A quantidade em estoque é um número inteiro maior ou igual a zero.
- **RN-05** — Todo produto pertence a uma única categoria. → ver N1 Catálogo: Regras transversais de negócio: 1

## Cenários

```gherkin
Feature: Cadastrar Produto

  Background:
    Given que o gestor de catálogo abriu o cadastro de produto a partir da Pesquisa de Produtos
    And que existem as categorias Cozinha, Vestuário, Papelaria, Acessórios e Decoração
    And que existe o produto "Caneca de cerâmica ACME 300 ml"

  # Caminho feliz
  @RN-02
  @RN-05
  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Produto cadastrado com os dados obrigatórios
    When o gestor informa "Caneca de vidro ACME 250 ml" em Nome do produto
    And informa 29,90 em Preço
    And informa 40 em Quantidade em estoque
    And escolhe "Cozinha" em Categoria
    And aciona "Salvar"
    Then o sistema exibe "Registro salvo com sucesso."
    And o produto aparece na Pesquisa de Produtos com a situação "Ativo"

  @RN-04
  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Produto cadastrado com descrição e sem estoque
    When o gestor informa "Caneca de vidro ACME 250 ml" em Nome do produto
    And informa "Vidro temperado, própria para bebidas quentes" em Descrição
    And informa 29,90 em Preço
    And informa 0 em Quantidade em estoque
    And escolhe "Cozinha" em Categoria
    And aciona "Salvar"
    Then o sistema exibe "Registro salvo com sucesso."

  # Erros de validação
  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Nome do produto vazio
    When o gestor deixa o Nome do produto vazio
    And aciona "Salvar"
    Then o sistema exibe abaixo do campo "Campo obrigatório."
    And não cadastra o produto

  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Preço vazio
    When o gestor deixa o Preço vazio
    And aciona "Salvar"
    Then o sistema exibe abaixo do campo "Campo obrigatório."
    And não cadastra o produto

  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Quantidade em estoque vazia
    When o gestor deixa a Quantidade em estoque vazia
    And aciona "Salvar"
    Then o sistema exibe abaixo do campo "Campo obrigatório."
    And não cadastra o produto

  @RN-05
  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Categoria vazia
    When o gestor deixa a Categoria vazia
    And aciona "Salvar"
    Then o sistema exibe abaixo do campo "Campo obrigatório."
    And não cadastra o produto

  @RN-03
  # ← FIELD-DICTIONARY: Valor monetário
  Scenario: Preço igual a zero
    When o gestor informa 0 em Preço
    And aciona "Salvar"
    Then o sistema exibe abaixo do campo "Informe um valor válido."
    And não cadastra o produto

  @RN-04
  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Quantidade em estoque negativa
    When o gestor informa -1 em Quantidade em estoque
    And aciona "Salvar"
    Then o sistema exibe abaixo do campo "Formato inválido."
    And não cadastra o produto

  # Conflitos com dados existentes
  @RN-01
  # ← MESSAGE-DICTIONARY: CAT_PRODUTO_DUPLICADO
  Scenario: Nome igual ao de outro produto
    When o gestor informa "caneca de ceramica acme 300 ml" em Nome do produto
    And aciona "Salvar"
    Then o sistema exibe abaixo de Nome do produto "Já existe um produto com este nome."
    And não cadastra o produto

  @RN-01
  # ← MESSAGE-DICTIONARY: CAT_PRODUTO_DUPLICADO
  Scenario: Nome igual ao de um produto inativo
    Given que o produto "Chaveiro ACME de metal" está inativo
    When o gestor informa "Chaveiro ACME de metal" em Nome do produto
    And aciona "Salvar"
    Then o sistema exibe abaixo de Nome do produto "Já existe um produto com este nome."
    And não cadastra o produto

  # Restrições de acesso
  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Tentativa sem acesso à funcionalidade
    Given que a pessoa não tem acesso à funcionalidade
    When tenta cadastrar um produto
    Then o sistema exibe "Você não tem permissão para esta ação."
    And não cadastra o produto

  # Estados especiais
  # ← MESSAGE-DICTIONARY: BASELINE
  Scenario: Falha inesperada ao salvar
    When o cadastro não pode ser concluído por falha inesperada
    Then o sistema exibe "Ocorreu um erro. Tente novamente."
    And mantém no modal os dados informados
```

## Campos

| Label PO | Entidade | Preenchimento | Edição | Tipo | Obrigatório | Validação |
|---|---|---|---|---|---|---|
| Nome do produto | Produto | entrada do usuário | imutável | texto | sim | Único entre os produtos, ativos ou inativos, sem diferenciar maiúsculas, minúsculas e acentos |
| Descrição | Produto | entrada do usuário | editável | texto | não | Texto livre, exibido na página do produto |
| Preço | Produto | entrada do usuário | editável | valor | sim | Maior que zero; → ver FIELD-DICTIONARY: Valor monetário |
| Quantidade em estoque | Produto | entrada do usuário | editável | número | sim | Inteiro maior ou igual a zero |
| Categoria | Categoria | entrada do usuário | editável | seleção → Categoria | sim | Uma única categoria do cadastro, ativa ou inativa, de primeiro nível ou subcategoria |

## Campos automáticos

| Label PO | Valor | Quando |
|---|---|---|
| Situação | Ativo | Ao salvar o cadastro |

## Superfície

**Modal** — origem: Pesquisar Produtos (Pesquisa de Produtos, `/catalogo/produtos`).

## Comportamento de tela

### Onde fica

O cadastro abre em modal sobre a Pesquisa de Produtos, pelo botão **Cadastrar produto** no topo da lista, com os campos Nome do produto, Descrição, Preço, Quantidade em estoque e Categoria e os botões **Cancelar** e **Salvar**. Ao salvar, o modal fecha e a lista é atualizada.

### Estados da tela

| Estado | Comportamento |
|---|---|
| Inicial | Modal com todos os campos vazios e sem mensagens de erro |
| Loading | Mostrar "Carregando…" no botão **Salvar** e evitar novo acionamento enquanto salva |
| Erro de validação | Mostrar a mensagem literal abaixo do campo; o modal continua aberto |
| Erro de servidor | Mostrar "Ocorreu um erro. Tente novamente." no modal, mantendo os dados informados |
| Sucesso | Fechar o modal, mostrar o aviso "Registro salvo com sucesso." e atualizar a lista |
| Cancelado | Fechar o modal sem cadastrar nada |

## Critérios de sucesso

| # | Critério mensurável | Origem |
|---|---|---|
| SC-01 | Todo produto cadastrado tem nome, preço, quantidade em estoque e categoria e aparece na pesquisa com a situação Ativo | CA-1, RN-02 e RN-05 |
| SC-02 | Nenhum nome repetido é aceito, ignorando maiúsculas e acentos, inclusive contra produtos inativos | CA-2 e RN-01 |

## Métricas de tamanho

| Função de Transação | Papel | Tipo | ALR | DER | Complexidade | PF | Data |
|---|---|---|---|---|---|---|---|
| Cadastrar Produto | principal | — | — | — | — | — | — |

A contagem está pendente e deve nascer da análise desta funcionalidade via CT, com memória de cálculo.

## Origem

| Ticket (AIMM) | Tipo | Critérios cobertos |
|---|---|---|
| [`STRY0013150`](../../../analise-impacto/AIMM-STRY0013150.md) | Criação | `CA-1, CA-2` — cadastro com nome, preço e quantidade em estoque obrigatórios e nome sem repetição |

## Changelog

| Data | Autor | Tipo | Descrição |
|---|---|---|---|
| 2026-10-31 | Claude | N3 criado | Cadastro de produto em modal, fonte canônica do CRUD, a partir do ticket fictício STRY0013150; gates sem aprovação e contagem pendente. |

## Suposições a confirmar

- ⚠️ O preço é maior que zero: o FIELD-DICTIONARY deixa em aberto se o valor aceita zero, e a loja não prevê produto gratuito.
- ⚠️ A unicidade do nome segue a de Categoria: vale contra produtos inativos e ignora maiúsculas e acentos. O CA-2 diz apenas que não pode haver dois produtos com o mesmo nome.
- ⚠️ A Categoria aceita categorias inativas, porque a massa tem a Almofada em Decoração inativa; o produto fica fora da vitrine até a categoria voltar.
- ⚠️ Categoria é obrigatória pelo modelo de entidades e pelo N1, embora o CA-1 cite só nome, preço e quantidade em estoque.
