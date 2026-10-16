---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como o trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorrem. Você pode revisar cada diff no contexto, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivos propostas;
- orientar o agente durante a sessão para adicionar uma alternância do modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez outra solicitação: uma alternância do modo claro para quem considera o tema padrão escuro demais. Em vez de começar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização do Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep**/**Undo**.

1. Volte à visualização do Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos, além dos comandos de terminal que ele solicitar.
3. Quando o agente propuser a edição de um arquivo, o diff aparecerá na conversa:
    - selecione **Keep** para aceitar a alteração;
    - selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente pausará e mostrará uma solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Uma mensagem final resumirá as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, vamos ampliar a mesma conversa para adicionar uma alternância do modo claro. Como você continua na mesma sessão, o agente mantém todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará de onde parou, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição à medida que aparecer, usando **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos controles de alternância de uma forma que não agrada — interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS; use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois controles").

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações em componentes Astro, estilos e testes relacionados, se houver.
3. Abra alguns arquivos alterados e examine os diffs. Confirme que foram aplicados os padrões de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição de arquivo, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como monitorar o trabalho de um agente em tempo real;
- como aceitar ou rejeitar edições individuais de arquivos;
- como orientar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
