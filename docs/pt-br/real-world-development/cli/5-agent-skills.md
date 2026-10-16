---
title: "Lição 5 - Personalizar e usar uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desenvolver software envolve mais do que escrever código. Já validamos manualmente que o código funciona e usamos arquivos de instruções para garantir que siga os padrões. Mas e os testes? O lint? E as outras partes da integração contínua (CI)?

Para esse tipo de tarefa, **skills de agente** são a melhor opção. Elas ajudam o Copilot a entender como executar operações desse tipo.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente;
- personalizar o formato dos resultados;
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para executar testes unitários, lint e verificação de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

Skills de agente agrupam instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega sob demanda. Basicamente, são uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. O Markdown contém um frontmatter com `name` e `description` para definir a skill, uma visão geral da finalidade e orientações sobre quando ela deve ser chamada. A pasta também pode conter subpastas com scripts e outros recursos que a skill usará quando for chamada.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios para uma skill. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto, como um recurso do repositório compartilhado e reutilizado pela equipe, ou na pasta de skills do usuário em `~/.copilot/skills`.

## Explorar a skill

Vamos explorar a skill criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificação de tipos, chamada `quality-checks`.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a saber quando chamar a skill.
3. Leia as instruções e observe como orientam o Copilot no processo de testes e lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Executar a skill antes da alteração

As skills podem ser chamadas diretamente pelo Copilot CLI ou por linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório no final.

## Personalizar o relatório

Queremos um relatório melhor, que informe o que foi executado, se houve sucesso e o que as ferramentas realmente comunicaram. Vamos atualizar a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

Agora vamos ver a alteração em ação. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificação de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [usando o servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
