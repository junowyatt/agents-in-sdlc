---
slug: pt-br/real-world-development/app
title: "Aplicativo GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** é um aplicativo para desktop baseado no Copilot CLI que reúne o desenvolvimento com agentes em um único espaço de trabalho focado. Ele adiciona sessões de agentes paralelas, modos de sessão alternáveis, canvases compartilhados e gerenciamento nativo de issues e pull requests do GitHub — incluindo o **Agent Merge**, que conduz um pull request por rebases, comentários de revisão, correções de integração contínua (CI) e mesclagem.

O workshop acompanha um fluxo de trabalho contínuo da Tailspin Toys:

1. Prepare o projeto, instale o aplicativo, conecte o repositório e explore o espaço de trabalho e o backlog criado.
2. Faça uma alteração focada nas avaliações por estrelas, revise-a no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo **Plan**, desenvolva no modo **Autopilot** e depois revise no modo **Interactive**.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill `quality-checks` existente e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise toda a alteração de filtragem e use o Agent Merge para o segundo PR.
9. Verifique a disponibilidade de canvas, use o Database Explorer e crie e teste um canvas de triagem vinculado ao repositório. Se os recursos necessários não estiverem disponíveis, registre o bloqueio e use a alternativa somente leitura ou avance para o encerramento.

Para manter o workshop focado, você criará dois PRs: primeiro, avaliações por estrelas; depois, filtragem com as atualizações de instruções, a atualização da skill, o perfil de QA e os testes. O fluxo de filtragem e qualidade compartilha uma sessão, uma worktree e um branch para que você aproveite o trabalho realizado ao explorar cada ferramenta. O exercício final de canvas permanece na sessão atual para que você se concentre em criar e testar a superfície compartilhada sem repetir o fluxo de PR.

## Lições

| Lição | Tópico | Descrição |
|--------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Instale o Node.js e crie sua cópia do projeto Tailspin Toys |
| [1. Instalar o aplicativo Copilot][ex1] | Configuração | Instale o aplicativo, conecte o projeto e conheça o espaço de trabalho |
| [2. Adicionar avaliações por estrelas: uma vitória rápida][ex2] | Primeira alteração | Exiba as avaliações existentes e o estado sem avaliação; depois mescle o PR 1 |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje a funcionalidade com base em uma issue, desenvolva no Autopilot e revise no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalizar e usar uma skill de verificações de qualidade][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute a skill |
| [6. Validar a funcionalidade com MCP do Playwright][ex6] | Observação no navegador | Configure o MCP pela opção Customize e inspecione o comportamento da filtragem |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna as evidências finais de verificação |
| [8. Criar e mesclar o PR da funcionalidade][ex8] | Revisão e mesclagem | Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois use o Agent Merge para o segundo PR |
| [9. Explorar e criar canvases][ex9] | Colaboração | Use o Database Explorer e crie e teste um canvas de triagem vinculado ao repositório |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho, os artefatos e outros recursos |

## Pré-requisitos

Antes de participar deste workshop, certifique-se de ter:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] Um computador com **macOS, Linux ou Windows**
- [ ] [Git instalado][install-git] no computador

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop — portanto, você pode concluir todas as trilhas com ele.

> [!NOTE]
> Como o aplicativo Copilot é executado no seu próprio computador, e não em um codespace, o [exercício de pré-requisitos][ex0] explica como instalar o Node.js e criar sua cópia do projeto antes de instalar o aplicativo.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**. Essa política é separada da política do Copilot CLI.

## Comece

**[Comece pelos pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
