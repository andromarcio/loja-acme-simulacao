# Tópico "Data de hoje"

O carimbo de todo artefato leva a data do dia (`atualizado: AAAA-MM-DD`). O modelo não sabe a data por conta própria, então o agente pede ao tópico. As instruções do agente já mandam usar este tópico e perguntar a data à pessoa se ele falhar.

## Como criar

1. No agente, abra **Tópicos** e clique em **+ Adicionar um tópico** → **Do zero**.
2. Dê o nome `Data de hoje`.
3. No nó de gatilho, escolha **O agente escolhe** (*The agent chooses*) e, em **Descrição**, cole:

   ```text
   Devolve a data de hoje no formato AAAA-MM-DD, no horário de Brasília. Use antes de gravar o carimbo de um artefato.
   ```

4. Abaixo do gatilho, clique em **+** → **Gerenciamento de variáveis** → **Definir valor da variável**.
5. Em **Definir variável**, crie uma variável nova chamada `DataHoje`.
6. Em **Para o valor**, abra a aba **Fórmula** e cole:

   ```text
   Text(DateAdd(Now(), -3, TimeUnit.Hours), "yyyy-mm-dd")
   ```

   O `-3` converte de UTC para o horário de Brasília. Sem ele, entre 21h e meia-noite o carimbo sairia com a data do dia seguinte.

7. Abra **Detalhes** do tópico → **Entrada e saída** (*Input and output*) → **Criar uma nova variável** de **saída** e escolha `DataHoje`. Na descrição da saída, escreva `Data de hoje, AAAA-MM-DD`.
8. Clique em **Salvar**.

## Como testar

No painel **Testar seu agente**, escreva `Que dia é hoje? Use o tópico Data de hoje.` A resposta tem de ser a data de hoje no formato `2026-10-29`. Se vier outro formato ou outra data, confira a fórmula do passo 6.
