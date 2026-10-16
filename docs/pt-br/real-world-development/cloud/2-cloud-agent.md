---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações deixam de enfrentar dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi desenvolvido para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request (PR) em rascunho para uma pessoa desenvolvedora revisar. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e liberar a equipe para se concentrar em atividades de maior impacto.

Neste exercício, você vai explorar como o agente de nuvem do Copilot permite:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam executadas com segurança;
- entender a importância de issues com escopo claro;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está completamente ausente. Como primeiro passo, a equipe gostaria de adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. A equipe ainda não precisa de uma implementação refinada; quer algo que permita testar a experiência do usuário para aceitação. Embora isso seja um bloqueio no momento, há outras issues com prioridade maior.

Esses dois exemplos podem ser facilmente adiados e são boas opções para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar neles de forma assíncrona, enquanto a pessoa desenvolvedora se concentra em outras tarefas e depois retorna para revisar o trabalho e confirmar que está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa da equipe, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode abrir a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo adequado

O GitHub Copilot não faz mágica: não elimina a necessidade de pensar. Até operações aparentemente simples podem ter complexidades ocultas. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos com a IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada. Antes de atribuir trabalho ao agente de nuvem do Copilot, examine somente para leitura os arquivos de instruções incluídos no repositório para perceber o efeito deles mais adiante.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace que já esteja em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o metadado `applyTo` no frontmatter, que define o escopo dessas instruções para arquivos `**/*.test.ts`.

Quando atribuir a issue `Code lacks documentation` ao agente de nuvem no próximo exercício, observe no pull request resultante os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções; voltaremos a destacá-las ao revisar PRs em uma lição posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em boas condições. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de modo semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo criando um [workflow especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que será executado antes do início do trabalho. Assim, o agente terá acesso às ferramentas e dependências de desenvolvimento necessárias. A configuração foi preparada antes do laboratório para facilitar o fluxo e permitir essa experiência de aprendizagem. Ela garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório como parte das etapas de configuração, por exemplo para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e popula o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente pela guia **Actions** do repositório. Isso é útil para testar se ele funciona corretamente sem esperar que o Copilot o execute.

## Adicionar documentação

Embora todas as pessoas reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é especialmente útil para criar documentação, e esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Você voltará para revisar o trabalho em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request gerado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão concluídas.
14. Role a linha do tempo do pull request; você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot resolve o problema: explora o código, entende o estado atual, pausa para pensar e decidir o plano adequado e cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona e liberar você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, escreveremos código para adicionar funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é poder dividir o trabalho: você se concentra em um conjunto de tarefas enquanto ele cuida de outro. Mesmo que adicionar uma seção de jogos relacionados para a equipe de design não leve muito tempo, esse tempo ainda poderia ser usado para outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot!

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na segunda solicitação. O agente de nuvem do Copilot funciona de maneira semelhante a uma pessoa engenheira de software: você não precisa monitorá-lo ativamente, mas deve revisar o trabalho quando estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para que trabalhe nelas de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar recursos ou ajudar a migrar código de um framework para outro.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam executadas com segurança;
- entender a importância de issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, podemos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
