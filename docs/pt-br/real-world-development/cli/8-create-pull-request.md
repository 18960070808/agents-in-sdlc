---
title: "Lição 8 - Criar e mesclar o PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; em seguida, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Sua implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do PR de avaliação por estrelas manualmente; desta vez, o **Agent Merge** cuidará do processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, todo o fluxo de filtragem e qualidade ficou em uma conversa e uma branch; esse trabalho será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionar a alteração completa do recurso e as evidências de verificação;
- criar o PR da filtragem;
- habilitar o Agent Merge somente após a revisão e confirmar o merge do PR.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o restante do trabalho no PR, mantendo a autorização de merge sob o controle da pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para incorporar um pull request. Quando habilitado, o Copilot resolve o que estiver bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub se o repositório permitir.

Até agora, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitá-lo.

## Usar o Agent Merge para gerenciar o PR

Com o código pronto, vamos revisá-lo em conjunto, criar o PR e permitir que o Agent Merge cuide das próximas etapas.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie um PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando todas as verificações forem aprovadas, ele fará o merge.
7. Confirme que aparece uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branches, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e faça o merge revisado manualmente quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do desenvolvimento: geração de código, testes, validação e agora o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionou a alteração completa do recurso e as evidências de verificação;
- criou o PR da filtragem;
- habilitou o Agent Merge somente depois da revisão e confirmou o merge do PR.

Em seguida, você [explorará outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
