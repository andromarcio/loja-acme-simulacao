# Docqui Caixa no Copilot Studio

Pacote para levar o método a um agente do Microsoft Copilot Studio: o analista especifica tickets conversando com o agente, que lê o método do repositório de documentação do sistema e grava a especificação na branch do ticket, pelo GitHub. É um estudo, ainda não testado num Copilot Studio real: o `roteiro-piloto.md` diz como validar antes de adotar.

**Comece pelo `LEIA-ME.html`**, aberto no navegador: o guia passo a passo, para quem nunca configurou nada disso, do GitHub ao Copilot Studio.

| Arquivo | Para quê |
|---|---|
| `LEIA-ME.html` | O guia de configuração, com os textos prontos para copiar e a marcação do progresso |
| `agentes/1-agente-docqui-caixa.md` | Nome, descrição e instruções do agente Docqui Caixa |
| `agentes/2-topico-data-de-hoje.md` | O tópico que dá ao agente a data do carimbo |
| `agentes/3-agente-retro.md` | O agente filho Retro e como a retro funciona no Copilot Studio |
| `agentes/alternativa-child-agents/` | O desenho alternativo, com um agente filho por skill |
| `github/.github/workflows/regenera-gerados.yml` | O workflow que a instância precisa para regenerar os arquivos derivados |
| `diagrama/` | O diagrama da arquitetura, interativo, gerado pelo archify, e a entrada dele |
| `roteiro-piloto.md` | O piloto: viabilidade, cenários, comparação com o Claude Code e critérios de decisão |
| `fonte/` | O gerador do `LEIA-ME.html` |

## Como as peças se ligam

O agente não guarda o método: lê, do próprio repositório da instância, a skill certa (`.claude/skills/`) e o prompt inteiro da opção escolhida (`engine/prompts/`). Por isso as instruções do agente guardam só o que é do Copilot Studio, e uma instância numa versão anterior do engine é atendida com os prompts da versão dela. Os scripts que o Claude Code roda não rodam no Copilot Studio: o que eles verificavam fica nos checks do PR, e o que eles geravam fica com o workflow `regenera-gerados`, que grava na branch com um GitHub App próprio, para que os checks rodem de novo sobre o commit dele.

O workflow não está nos templates de `.github/` que o `init-instance` instala: sem o GitHub App e os segredos, ele falharia a cada push. A instância que for usar o Copilot Studio o copia daqui, como o guia manda.

## Ao mudar o pacote

- As instruções dos agentes citam prompts, skills, scripts e caminhos do engine. Quando um deles mudar de nome ou de comportamento, revise `agentes/` na mesma versão.
- O `LEIA-ME.html` é gerado: edite `agentes/`, o workflow ou `fonte/guia.template.html` e rode, a partir desta pasta, `python3 fonte/monta.py`. O gerador copia os textos dos arquivos para o guia, e assim o que se copia do guia é sempre o que está nos arquivos.
- Cada bloco de instruções tem de caber em 8.000 caracteres, o limite do Copilot Studio; o guia mostra o tamanho de cada um.
