# Roteiro da simulação — do zero ao primeiro ticket entregue

Você vai começar a documentação de um sistema que ainda não tem nada escrito, como um analista da CAIXA faria com o agente do Copilot Studio: conversando, sem abrir o GitHub. O agente é o GitHub Copilot (CLI ou VS Code), com as instruções do `AGENTS.md` deste repositório, que seguem o agente do Docqui Caixa 9.2.2. Quem revisa é a conta `@androlouredo`, que faz o papel da PO e do DBA.

| Papel | Conta | Onde age |
|---|---|---|
| Analista | `@andromarcio` | Na conversa com o agente |
| PO e DBA (revisão) | `@androlouredo` | Na conversa com o agente, com a conta dele ativa no `gh` |
| Code owner (merge) | `@androlouredo` | No site do GitHub, no fim |

A instância está em branco: o MASTER sem sigla e sem nome, a visão do produto por escrever e nenhum domínio. O sistema é a **Loja ACME**, uma loja virtual fictícia. O primeiro ticket é o **STRY0013170 — Categorias de produtos**: ele cria o primeiro domínio (Catálogo), o primeiro agrupamento (Categorias), um cadastro completo e a primeira entidade, e é nele que a identidade do sistema e a visão do produto são escritas.

## Antes de começar

### No GitHub, uma vez

1. **Colaborador.** Em *Settings → Collaborators*, convide `androlouredo` com papel **Write**. Entre com a conta `androlouredo` e aceite o convite (chega por e-mail e aparece em github.com/notifications). Sem isso, o CODEOWNERS não vale e a revisão não pode ser pedida a ele. Confira que o `.github/CODEOWNERS` aponta para ele: os responsáveis do modelo são fictícios (`@org/…`).
2. **Merge com a descrição.** Em *Settings → General → Pull Requests*: deixe marcado **Allow merge commits** e, em *Default commit message*, escolha **Pull request title and description**. É o que leva a linha `Approved-by:` da descrição do PR para a mensagem do merge.
3. **Proteção da `main`.** Em *Settings → Branches → Add branch protection rule* (ou *Rules → Rulesets*), para `main`:
   - **Require a pull request before merging**, com **Require approvals** (1), **Dismiss stale pull request approvals when new commits are pushed** e **Require review from Code Owners**;
   - **Require status checks to pass**, com `Esteira de gates — check`, `estrutura`, `rastreabilidade` e `entrega` (eles só aparecem na busca depois de rodar uma vez);
   - **Do not allow bypassing the above settings**, para que nem a conta dona passe por cima.
4. **GitHub App do `docqui-bot`.** O workflow *Regenera arquivos gerados* grava os arquivos derivados na branch com um GitHub App. Instale o App neste repositório e crie, em *Settings → Secrets and variables → Actions*, os segredos `DOCQUI_APP_ID` e `DOCQUI_APP_PRIVATE_KEY` (o passo a passo está no guia `engine/copilot-studio/LEIA-ME.html`, parte 1). Sem eles, as conferências ficam esperando o commit do `docqui-bot`.
5. **Pages.** Em *Settings → Pages*, escolha **GitHub Actions** como fonte e, em *Settings → Environments → github-pages*, permita `main` e `historia/*`. É o que publica a prévia da branch do ticket.

### Na sua máquina

1. Instale o `git`, o GitHub CLI (`gh`) e uma das duas opções:
   - **Copilot CLI:** `npm install -g @github/copilot` (pede Node 22 ou mais novo) e, na primeira vez, `copilot` e `/login`;
   - **VS Code:** a extensão GitHub Copilot Chat, com o chat no modo **Agent**.
2. Entre nas duas contas no `gh` (uma de cada vez, no navegador):

```bash
gh auth login
```

3. Clone o repositório e entre nele:

```bash
gh repo clone andromarcio/loja-acme-simulacao-novo
```

```bash
cd loja-acme-simulacao-novo
```

4. Antes de cada parte, confira qual conta está ativa e troque quando o roteiro mandar:

```bash
gh auth status
```

```bash
gh auth switch --user andromarcio
```

Quem age no GitHub é a conta ativa do `gh` — como na CAIXA, onde a ferramenta do GitHub usa a conta de quem conversa.

5. Abra o agente na pasta do repositório (`copilot`, ou o VS Code com a pasta aberta). Se ele não começar pelo sistema, diga "leia o AGENTS.md e siga".

## Respostas prontas

O agente vai perguntar, uma coisa por vez. Use estas respostas; o que não estiver aqui, diga "não sei, marque para a PO".

**Identidade do sistema**

- Sigla: LACME
- Nome: Loja ACME
- Descrição: Loja virtual fictícia em que o cliente compra sozinho, conhecendo o valor da entrega antes de confirmar.

**Visão do produto**

- Propósito: o cliente da ACME compra por telefone e só descobre o custo da entrega quando o pedido já foi fechado. A Loja ACME existe para que ele compre sozinho, a qualquer hora, sabendo quanto vai pagar antes de confirmar.
- Proposta de valor: encontrar o produto e comprar em poucos passos, com o frete à vista antes da confirmação; a equipe da loja mantém o catálogo e a vitrine sem depender da equipe de sistemas.
- Personas: Visitante (navega sem se identificar), Cliente (compra na loja), Gestor de catálogo (organiza o que a loja mostra) e Administrador (cuida da operação).
- Objetivos: levar 60% dos pedidos para o autoatendimento; eliminar a cotação de frete por telefone.
- Domínios previstos: Catálogo (produtos e categorias), Vitrine (o que o visitante vê), Vendas (carrinho, checkout e pedidos) e Administração (usuários e faixas de frete).
- Fora do escopo: marketplace de outros vendedores; cobrança e operação da transportadora; controle físico do estoque.

**O ticket STRY0013170 — Categorias de produtos**

> A equipe de vendas precisa organizar os produtos da loja em categorias, para que o cliente encontre o que procura: pesquisar, cadastrar, alterar, excluir e consultar as categorias.
>
> CA-1 A equipe pesquisa as categorias por nome e por situação.
> CA-2 A categoria é cadastrada com nome e uma descrição opcional; não pode haver duas categorias com o mesmo nome.
> CA-3 A equipe altera a descrição e a situação da categoria, mas não o nome; os produtos de uma categoria inativa deixam de aparecer na vitrine.
> CA-4 Categoria com produtos não pode ser excluída.
> CA-5 A equipe consulta os dados de uma categoria e quantos produtos ela tem.

## Parte 1 — Analista: começar e abrir o primeiro ticket

Conta ativa: `andromarcio`. Uma conversa nova.

> **1.1** Quero documentar a Loja ACME.

Esperado: o agente lê o MASTER e percebe que a instância está em branco (sem sigla, sem nome, sem visão do produto, sem domínios). Ele diz isso em linguagem simples e pede o primeiro ticket, porque tudo o que se grava vai no trabalho de um ticket. Não deve inventar sigla nem nome, nem falar de branch.

> **1.2** O primeiro ticket é o STRY0013170 — Categorias de produtos. *(cole o texto e os critérios das Respostas prontas)*

Esperado: o agente diz que vai fazer a análise de impacto do ticket e segue, uma pergunta por vez. Pontos em que ele deve parar e perguntar, ou sinalizar com ⚠️:

- que a identidade do sistema e a visão do produto ainda não existem e entram no trabalho deste ticket;
- que o domínio Catálogo e o agrupamento Categorias são novos;
- que o CA-3 e o CA-4 falam de produtos e da vitrine, que ainda não existem: a regra fica registrada, mas a ligação com o Produto é uma pendência para quando ele for especificado;
- se a exclusão é definitiva ou desativação (é definitiva, bloqueada enquanto houver produto vinculado: CA-4).

Ao fim, ele mostra o que o ticket cria — as cinco funcionalidades do cadastro (Pesquisar, Cadastrar, Editar, Excluir e Visualizar Categoria) — e pede a sua confirmação.

> **1.3** Confirmo.

Esperado: ele grava a análise de impacto já completa, em `em-análise`, com a coluna Status de cada artefato como `Pendente`, e abre o PR em rascunho. A lista de artefatos tem de incluir a identidade (`global/MASTER.md`), a visão do produto, o domínio, o agrupamento, as cinco funcionalidades, o modelo de entidades e os dicionários. Ele diz que a análise está pronta e que as conferências estão rodando, sem sugerir comando e sem falar em branch, commit, PR ou nome de prompt.

Depois de um ou dois minutos, peça "pode conferir?". A conferência de estrutura fica vermelha até a visão do produto ser escrita: é esperado nesta altura.

> **1.4** Como está o meu ticket?

Esperado: "em especificação", o que falta (o aval da PO no escopo) e o link da prévia (`https://andromarcio.github.io/loja-acme-simulacao-novo/historia/STRY0013170/`).

## Parte 2 — PO: avalizar o escopo

Troque a conta: `gh auth switch --user androlouredo`. Conversa nova.

> **2.1** Sou a PO da Loja ACME. Quero ver o escopo do ticket STRY0013170.

Esperado: o agente reconhece que a conta não é a autora, entra na parte de revisão, resume a análise de impacto em linguagem de negócio — inclusive que a identidade e a visão do produto nascem neste ticket — e não grava nada.

> **2.2** O escopo está de acordo. Registre um comentário dizendo: "Escopo avalizado pela PO. A exclusão é definitiva, bloqueada enquanto a categoria tiver produtos."

Esperado: mostra o texto, pede o "sim" e registra um comentário (não uma aprovação: nada foi especificado ainda).

## Parte 3 — Analista: especificar

Volte: `gh auth switch --user andromarcio`. Conversa nova — o agente não lembra da anterior.

> **3.1** Continue o ticket STRY0013170 de onde parou.

Esperado: ele lê o que está gravado, encontra o comentário da PO e propõe registrar o aval antes de qualquer especificação: `escopo-aprovado`, com `avalizado-por` e `estado-por` com o nome da PO e `estado-em` com a data e a hora.

> **3.2** Sim, registre o aval e siga.

> **3.3** Pode especificar.

Esperado, nesta ordem e um documento por vez:

1. a identidade do sistema no MASTER (use as Respostas prontas);
2. a visão do produto;
3. o domínio Catálogo, com os agrupamentos previstos (Categorias agora; Produtos, planejado);
4. o agrupamento Categorias;
5. as cinco funcionalidades do cadastro;
6. a entidade Categoria no modelo de entidades;
7. os dicionários (campos, regras, mensagens e erros) que as funcionalidades citam.

Ao terminar cada documento, ele grava, marca `Especificado` na análise de impacto, diz que está pronto para você revisar e pergunta se pode especificar os demais — **sem pedir aprovação**. Não pode faltar nada da lista: domínio, agrupamento, funcionalidades, dicionários e modelo de entidades são responsabilidade dele. Isso leva tempo: siga até ele dizer que acabou.

Depois de cada gravação, ele deve dizer que as conferências estão rodando e parar. Espere um ou dois minutos e peça:

> **3.4** Pode conferir?

Esperado: o resultado das conferências. Com a visão do produto escrita, a de estrutura fica verde. Se alguma reprovar por forma, ele corrige e diz o que corrigiu.

Revise um dos documentos (no site de prévia ou pedindo ao agente que o mostre) e diga:

> **3.5** Revisei a visão do produto e aprovo.

Esperado: ele marca a visão do produto como `Validado` na análise de impacto, com a sua aprovação.

> **3.6** O que falta para mandar para a revisão?

Esperado: a lista dos documentos ainda `Especificado` (por revisar) ou `Pendente`.

## Parte 4 — Analista: concluir e mandar para a revisão

> **4.1** Revisei os demais documentos e aprovo todos.

Esperado: todos `Validado`. Se algum artefato previsto não foi necessário, ele pergunta se pode marcá-lo como `Não se aplica`.

> **4.2** Pode mandar para a revisão.

Esperado, num passo só: leva a análise de impacto a `concluído` (com o seu nome em `estado-por`), grava os checkpoints CP1 e CP2 nas cinco funcionalidades com o nome de quem revisa, põe a linha `Approved-by:` no fim da descrição e diz que as conferências estão rodando. Com algum documento ainda por validar, ele diz quais faltam e para. Depois de um ou dois minutos, peça "pode conferir e mandar?": com tudo verde, ele tira do rascunho, pede a revisão ao `@androlouredo` e diz para quem foi.

> **4.3** Quais tickets estão comigo?

Esperado: o STRY0013170, "em revisão", com `@androlouredo`.

## Parte 5 — Revisão: aprovar ou pedir ajuste

Troque: `gh auth switch --user androlouredo`. Conversa nova.

> **5.1** Quero revisar o ticket STRY0013170.

Esperado: o resumo do que mudou, por documento, com as funcionalidades como "ID — Nome", e a situação das conferências.

Peça um ajuste pequeno — por exemplo:

> **5.2** Peça para ajustar: a pesquisa precisa mostrar, em cada categoria, quantos produtos ela tem.

Esperado: escreve o pedido em linguagem de negócio, apontando o documento e o trecho, mostra o texto, pede o "sim" e registra o pedido de mudanças. Volte à conta do analista, peça "como está o meu ticket?" e "vamos fazer o ajuste": ele conduz a correção, grava, diz que o documento está pronto para revisar e, com a sua aprovação e as conferências verdes, pede a revisão de novo. Troque outra vez para a PO e diga:

> **5.3** Aprovo.

Esperado: confere que a conta não é a autora e que as conferências estão verdes, pede a confirmação e registra a aprovação. O PR é sempre aprovado por alguém diferente de quem o criou.

## Parte 6 — Entrega (merge, à mão)

No site do GitHub, com a conta `androlouredo`, abra o PR e clique em **Merge pull request** (*Create a merge commit*). Confira que a mensagem termina com a linha `Approved-by:`. O agente não faz merge: na CAIXA, a entrega é do code owner.

Depois do merge, confira na aba *Actions* que o `spec-guard` do push na `main` passou, e no site da `main` que o MASTER mostra a Loja ACME, a visão do produto está publicada e o índice mostra as cinco funcionalidades de Categorias como 📋 especificadas.

## Parte 7 — Analista: retro

Volte: `gh auth switch --user andromarcio`. Na última conversa do analista:

> **7.1** Quer registrar o que aprendemos?

(Ou espere ele propor.) O que for do método vira uma issue no `docqui-caixa-engine`; o que for da Loja ACME vai para as regras do `CLAUDE.md`.

## O que observar

| # | Pergunta | Onde |
|---|---|---|
| O1 | Ele falou de branch, commit, PR ou check sem você usar esses termos? | Todas |
| O2 | Inventou sigla, nome ou visão em vez de perguntar? | 1, 3 |
| O3 | Fez mais de uma pergunta por mensagem, pediu aprovação ao fim de cada documento (em vez de perguntar se pode seguir) ou marcou `Validado` sem você aprovar? | 1, 3, 4 |
| O4 | A análise de impacto listou a identidade, a visão do produto, o domínio, o agrupamento, as funcionalidades, o modelo de entidades e os dicionários? Faltou algo no fim? | 1.3, 4.2 |
| O5 | Sinalizou que o CA-3 e o CA-4 dependem do Produto e da vitrine, que ainda não existem? | 1.2, 3 |
| O6 | Depois da gravação, disse que as conferências estavam rodando e esperou você pedir "pode conferir?", sem dizer que terminou? | 1.3, 3 |
| O7 | Rodou algum script do repositório? (não devia: no Copilot Studio, nenhum roda) | Todas |
| O8 | Citou o arquivo de um prompt (`PROMPT_…`) ou um código do menu (`3A`, `TR`) em vez de dizer o que ia fazer? | Todas |
| O9 | A análise de impacto registrou quem aprovou cada troca de estado (`estado-por`) com o nome certo, e nunca o do agente? | 1.3, 3.2, 4.2 |
| O10 | Quanto tempo levou cada parte, e onde você se perdeu? | Todas |

## Se der errado

| O que acontece | O que fazer |
|---|---|
| O agente não começa pelo sistema | Diga "leia o AGENTS.md e siga". No VS Code, confira que o chat está no modo Agent |
| Ele inventa a sigla ou o nome | Corrija com as Respostas prontas e anote no O2 |
| Ele não consegue pedir a revisão ao `@androlouredo` | O convite de colaborador não foi aceito, ou o CODEOWNERS ainda aponta para `@org/…` |
| O commit do `docqui-bot` não chega | Veja a aba *Actions*: o *Regenera arquivos gerados* precisa do GitHub App instalado e dos segredos (Antes de começar, item 4) |
| A conferência de estrutura fica vermelha | Até a visão do produto ser escrita, é esperado. Depois, peça ao agente para conferir e corrigir |
| O check `entrega` fica vermelho durante a especificação | É esperado até o passo 4.2: ele só passa com a análise de impacto `concluído` e as funcionalidades especificadas |
| O portão da entrega acusa arquivo "tocado e não declarado" | A análise de impacto não listou um documento que a especificação gravou (a identidade, a visão ou um dicionário): peça para acrescentá-lo e anote no O4 |
| Quer recomeçar do zero | Feche o PR sem merge, apague a branch `historia/STRY0013170` e comece pela Parte 1 |
