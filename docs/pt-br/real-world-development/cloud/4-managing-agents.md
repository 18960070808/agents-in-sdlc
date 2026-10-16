---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse Mission Control, você pode ver todas as tarefas de agentes com pull requests (PRs) abertos. Também pode explorar as operações executadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem;
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como o trabalho já atualizaria o estilo do site e adicionaria controles de alternância, pareceu lógico incluir essa funcionalidade. Você orientará o trabalho do agente para garantir que ele adicione tanto o modo claro quanto o alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos conferir o status atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Ela deve incluir as tarefas atribuídas ao Copilot, como:
    - atualizar a documentação da base de código;
    - gerar APIs para modificar jogos;
    - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento. Revise as etapas executadas pelo Copilot, que podem incluir:
    - obter o código do repositório;
    - criar o ambiente de trabalho do Copilot;
    - configurar servidores MCP;
    - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request (PR), exibido à direita. Nele, você pode ver o PR e os arquivos alterados para monitorar o trabalho.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, vamos pedir ao Copilot que inclua a alternância do modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão que trata da adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que o Copilot indique que terminou a configuração e começou a trabalhar. Você perceberá isso quando começarem a aparecer mensagens semelhantes às abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo de orientação da sessão ativa em destaque](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot continuará trabalhando na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo assim que concluir a etapa em andamento no momento em que você enviou a mensagem.

Isso levará alguns minutos. Aproveite a pausa para refletir sobre tudo o que aprendeu e explorou até agora.

## Resumo e próximos passos

Nesta lição, você explorou a página de agentes do Copilot, seu espaço central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Nesse Mission Control, é possível acompanhar todas as tarefas ativas e concluídas, revisar o trabalho em andamento e redirecionar tarefas para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- explorar o Mission Control e a página de agentes para monitorar tarefas do agente de nuvem;
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
