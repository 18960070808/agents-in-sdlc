---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e operações como executar testes. Em geral, a pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente foi criado para trabalhar com mais autonomia no IDE. Ele se comporta de forma semelhante a uma pessoa desenvolvedora: primeiro explora a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE e permite ciclos rápidos de interação. O agente de nuvem funciona de forma assíncrona, como uma pessoa da equipe, interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados;
- revisar as alterações e os testes gerados antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, queremos permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você já adicionou um helper de editoras; agora, usará o modo de agente do Copilot para concluir o trabalho restante na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir seus detalhes. O site é um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o ORM Drizzle — não há uma API de backend separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno elemento Astro `<script>` com escopo próprio, usando as APIs DOM padrão.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace:

1. Volte ao codespace. Você deverá estar na branch `main` após o exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, deverá aparecer um aviso com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir `http://localhost:4321` em uma janela do navegador, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar um link `localhost` no terminal redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse túnel privado direciona ao codespace que agora hospeda o servidor Web.

### Explorar o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e modelo varia por exercício; cada etapa indicará o que for necessário.

A implementação inicial do site funciona, mas queremos adicionar recursos. Vamos começar revisando o backlog. Quando você configurou o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Na lista de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações autonomamente em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela da lista de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo específico. O workshop foi criado usando Claude Sonnet 4.5 e o nome do modelo foi incluído para que as sugestões de código sejam relativamente consistentes e a experiência seja boa. Como os modelos de linguagem grandes são probabilísticos, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem grandes, o Copilot poderá usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie este prompt ao Copilot para consultar o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens com base na utilidade que espera que tenham para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Vamos usar o arquivo de [instruções de interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, que incluem:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções específicos para vários tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos à localização do próprio arquivo de instruções. Isso facilita a reutilização, permite dividir instruções complexas em partes menores e possibilita apontar para exemplos e modelos.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, será necessário fazer pelo menos três atualizações distintas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento da filtragem;
- atualizar a página da lista de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para escolher o arquivo de instruções.

   ![Captura de tela de exemplo da seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes da visualização Chat.

   ![Captura de tela da lista de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue correspondente do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto e localiza os arquivos do recurso — helpers da camada de dados, interface e testes. Em seguida, ele começa a modificar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias etapas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat que pede confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas em alguns testes durante o processo. Tudo bem: o Copilot alterna entre gerar código e executar testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat com o modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados;
- o modo de agente explora o projeto, identifica arquivos relevantes e coordena alterações;
- revisar alterações e testes gerados antes de mesclá-los à base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra — implementar paginação

À medida que a lista de jogos crescer, será necessário adicionar paginação. Use as habilidades aprendidas neste exercício e peça ao Copilot que atualize o site para implementar paginação. Considere:

- seguir as práticas existentes, incluindo o uso dos arquivos de instruções disponíveis;
- decidir como implementar a paginação: permitir que a pessoa usuária escolha o tamanho da página ou defini-lo no código;
- fornecer ao Copilot orientações suficientes para criar a implementação desejada;
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: para que servem e quando usá-los][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
