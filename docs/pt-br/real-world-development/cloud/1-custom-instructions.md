---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot deve conhecer informações de contexto — é importante garantir que elas estejam disponíveis. Os [arquivos de instruções][instruction-files] permitem fornecer essa orientação, para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo por caminho;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos percursos do VS Code e da CLI, você não executará aqui um prompt *antes/depois*: o agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante neste percurso, ao revisar os pull requests produzidos pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- a camada de dados sempre precisa de testes unitários;
- a interface deve usar o modo escuro e ter uma aparência moderna;
- o código deve incluir documentação em comentários TSDoc;
- cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. Também pode criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter orientações no nível do projeto — contexto relevante para a maioria das solicitações de chat ou da CLI enviadas ao Copilot. Pode incluir a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para definir diretrizes para linguagens como TypeScript ou Astro ou para tarefas como criar um componente de interface ou um novo conjunto de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções no nível do repositório quanto os arquivos `*.instructions.md` com metadados `applyTo` no frontmatter.
>
> O Copilot também aceita arquivos de instruções em outros padrões, como AGENTS.md e CLAUDE.md. Consulte [os padrões aceitos para arquivos de instruções][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está fora do escopo deste workshop. Os exemplos do projeto demonstram, porém, uma abordagem representativa. De modo geral:

- Mantenha o conteúdo de `copilot-instructions.md` focado em orientações para o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para orientações específicas sobre tipos de arquivo (testes unitários, componentes Astro ou a camada de dados) ou sobre tarefas específicas.
- Use linguagem natural. Mantenha as orientações claras e inclua exemplos de como o código deve (e não deve) ser escrito.

Não existe uma única maneira de criar arquivos de instruções, assim como não existe uma única maneira de usar IA. Com a experimentação, você descobrirá o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá arquivos para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da interface que você estiver usando indicará quando ela for relevante.
>
> Está procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos no repositório: há um arquivo principal, `copilot-instructions.md`, e um conjunto de arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. A seção **Code standards** também contém orientações aninhadas para **GitHub Actions Workflows**. Essas orientações se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, para a camada de dados do Drizzle, para testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript — por exemplo, um que corresponda a `**/*.test.ts` — será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório. Assim, você pode dividir conjuntos maiores de orientações em arquivos menores e reutilizáveis e apontar o Copilot para exemplos.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicione um novo padrão para o repositório

Esta é a única edição que você fará neste exercício: adicione uma regra para todo o projeto que determine que a documentação deve estar no código, em comentários TSDoc e em um cabeçalho no nível do arquivo. O agente de nuvem aplicará essa regra ao trabalhar nas issues que você atribuir a ele mais adiante neste percurso.

Antes de editar, crie uma branch para trabalhar. O agente de nuvem lerá as instruções da branch de destino da issue, mas você ainda fará commit das alterações de forma organizada:

1. No terminal do codespace, crie uma nova branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado, `.github/copilot-instructions.md` fornece informações sobre o projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de código do projeto — ainda não há uma regra para documentação no código, por isso o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo depois dos padrões existentes, para instruir o Copilot a incluir cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem estar no nível do repositório (`.github/copilot-instructions.md`), para fornecer orientações gerais, ou usar o padrão `*.instructions.md`, para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões que abrangem o projeto inteiro, como a regra de comentários de documentação.

## Faça commit, push e merge da atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Por isso, as alterações nas instruções precisam chegar a `main` para que o agente de nuvem possa usá-las.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem lerá estas instruções quando trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante este workshop, você pode ignorar a etapa da branch e fazer commit diretamente em `main`. A branch foi incluída para que o workshop reflita como você trabalharia em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa os arquivos de instruções deste projeto e adicionou um novo padrão para todo o repositório a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente de nuvem ao longo do restante deste percurso.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
