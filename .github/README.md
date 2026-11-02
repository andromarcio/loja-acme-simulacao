# Aprovação da especificação — perfil requisitos

Copie CODEOWNERS, o template de PR e os workflows de validação desta pasta para a instância. Substitua os responsáveis fictícios pelos usuários ou times reais.

## Checkpoints

CP1 aprova os requisitos; CP2 aprova o modelo de entidades negocial. A esteira termina em `especificado`. A revisão pode cobrir ambos no mesmo PR; registre `por` e `em` em cada gate, sem atribuir aprovação por inferência.

No PR, inclua os artefatos revisados e regenere `modules/INDEX.md` com `python scripts/gates.py promote --write`. O gate-check confere transições e o espelho; o CODEOWNERS encaminha a revisão ao responsável. Configure proteção da branch e exigência de review no GitHub, com os checks obrigatórios **Esteira de gates — check** e os jobs **estrutura**, **rastreabilidade** e **entrega** do `spec-guard.yml`. O `spec-guard` roda em todo PR, então exigi-lo não trava PR que não mexe na especificação; fora de uma branch `historia/` ou `sprint/`, o `entrega` é pulado e conta como aprovado. O check só aparece na busca depois de rodar uma vez no repositório.

## Entrega

Na cadência sprint, commits de especificação citam uma única chave de ticket. As AIMMs registram o escopo e os resultados; `valida-entrega` confere o merge para a main. A main recebe somente especificações entregues. Não há aprovação de testes nem de código neste perfil.

No merge para main, registre o trailer `Approved-by: Nome <email>` do responsável real. Não use os autores dos exemplos como se tivessem revisado o artefato.
