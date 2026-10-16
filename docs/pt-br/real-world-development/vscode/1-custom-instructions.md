---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot deve conhecer informações de contexto — é importante garantir que elas estejam disponíveis. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que permitem orientar a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos com escopo por caminho;
- gerar a primeira parte de dados da filtragem (um helper de editoras) com as instruções *atuais*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o código gerado incorporar o novo padrão;
- fazer commit das instruções atualizadas e da etapa de filtragem em `main` para que o Copilot use as orientações no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo do exercício é observar a *tendência* de mudança no comportamento depois de atualizar as instruções, não obter um resultado idêntico caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- a camada de dados sempre precisa de testes unitários;
- a interface deve usar o modo escuro e ter uma aparência moderna;
- o código deve incluir documentação em comentários TSDoc;
- cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Você pode especificar convenções, bibliotecas e até os tipos de comentários que prefere. Também pode criar instruções para todo o repositório ou para tipos de arquivo específicos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações do projeto relevantes para a maioria das solicitações de chat ou da CLI, como a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para definir diretrizes para linguagens como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou um conjunto de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com metadados `applyTo` no frontmatter ao gerar código.
>
> O Copilot também aceita arquivos de instruções em outros padrões, como AGENTS.md e CLAUDE.md. Consulte [os padrões aceitos para arquivos de instruções][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está fora do escopo deste workshop. Os exemplos do projeto demonstram, porém, uma abordagem representativa. De modo geral:

- Mantenha o conteúdo de `copilot-instructions.md` focado em orientações do projeto, como sua finalidade, sua estrutura e padrões globais de código.
- Use arquivos `*.instructions.md` para orientações específicas sobre tipos de arquivo (testes unitários, componentes Astro ou a camada de dados) ou tarefas.
- Use linguagem natural. Mantenha as orientações claras e inclua exemplos de como o código deve (e não deve) ser escrito.

Não existe uma única maneira de criar arquivos de instruções, assim como não existe uma única maneira de usar IA. Com a experimentação, você descobrirá o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da interface usada indicará quando ela for relevante.
>
> Está procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos no projeto inicial: há um arquivo principal, `copilot-instructions.md`, e outros arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**. Elas se aplicam às interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore-a. Há instruções para arquivos Astro, para a camada de dados do Drizzle, para testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob, relativo à raiz do repositório, que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript — por exemplo, um que corresponda a `**/*.test.ts` — será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório. Assim, você pode dividir conjuntos maiores de orientações em arquivos menores e reutilizáveis e apontar o Copilot para exemplos.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma nova branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e modelo varia por exercício; cada etapa indicará o que for necessário.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar os resultados.

1. Feche todas as guias do editor abertas nos exercícios anteriores para que o Copilot receba apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deve ficar.
3. Selecione **Agent** na lista de agentes da visualização Chat para permitir que o Copilot aplique alterações aos arquivos.

   ![Captura de tela da lista de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações, geralmente ao helper e aos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras. Isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicáveis a `src/lib/*.ts`).
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode adicionar comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem: o objetivo é observar a melhoria de *consistência* depois de atualizar as instruções.

## Adicionar um novo padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece informações sobre o projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de código, mas ainda não define uma regra para documentação no código — por isso, o helper gerado pode não ter comentários de documentação.
3. Adicione as linhas Markdown a seguir logo depois dos padrões existentes, para instruir o Copilot a incluir cabeçalhos de comentário e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem estar no nível do repositório (`.github/copilot-instructions.md`), para fornecer orientações globais, ou usar o padrão `*.instructions.md`, para linguagens, tipos de arquivo ou tarefas específicas. O arquivo no nível do repositório é o lugar certo para padrões que abrangem o projeto inteiro, como a regra de comentários de documentação.

## Enviar o prompt novamente e observar a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` para indicar ao Copilot o arquivo relevante.
3. Envie **o mesmo prompt** usado antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são ativos do repositório e devem ser gerenciados pelo controle de versão como qualquer outro arquivo. Vamos fazer commit das alterações e enviar a branch.

1. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações das instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções atualizadas e de todos os arquivos modificados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações de suporte (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Agora que a branch foi enviada, vamos criar um pull request e vinculá-lo à issue sobre o padrão de documentação no backlog. O Copilot pode encontrar a issue, criar um PR associado a ela e fazer o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O modelo do projeto já configura as ferramentas do GitHub no espaço de trabalho, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma delas, o VS Code talvez solicite que você entre no GitHub. Siga as instruções para autorizar. Em um exercício posterior, você aprenderá como essa conexão funciona (Model Context Protocol).

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que encontre a issue sobre a atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo tanto as atualizações das instruções quanto a nova base do helper de editoras:

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot pedir permissão para executar ações do GitHub, revise o comando e selecione **Allow** conforme apropriado.
6. Depois da criação do PR, peça ao Copilot que faça o merge e volte a branch local para `main`:

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot pedir permissão para executar ações do GitHub e comandos de shell, revise o comando e selecione **Allow** conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot usa as instruções do projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o código proposto incorporar o novo padrão;
- fazer commit das instruções atualizadas e da base do helper em `main` para que o próximo exercício possa aproveitá-las.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
