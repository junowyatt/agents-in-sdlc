---
title: "Lição 8 - Criar e mesclar o PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em um único branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você mesmo mesclou o pull request (PR) da avaliação por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e um branch, e incluiremos todo o trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- inspecionar todo o PR da funcionalidade e as evidências de verificação;
- autorizar o Agent Merge somente depois da revisão e confirmar a mesclagem do PR.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização da mesclagem sob controle da pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve os bloqueios — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e o mescla assim que o GitHub permitir. O processo é executado em segundo plano, continua mesmo que o aplicativo seja reiniciado e é desativado quando o pull request é mesclado.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de sua autorização explícita para editar o código e mesclar o PR. Antes de conceder permissão de mesclagem, revise as ações permitidas e o trabalho.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para escolher **Default agent**.
2. Selecione a seta ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar apenas a opção no menu suspenso não o inicia.

O processo do Agent Merge começa. Ele:

- cria o pull request com título e descrição;
- se você iniciou a sessão a partir de uma issue, menciona a issue correspondente no corpo da descrição;
- faz rebase ou resolve possíveis conflitos de mesclagem com o branch de destino;
- acompanha o processo de CI para garantir que todas as verificações sejam aprovadas;
- acompanha o PR para detectar comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e atualiza o código para atendê-los;
- pode mesclar automaticamente o PR depois que todas as verificações forem aprovadas.

Vamos permitir que o Agent Merge também mescle o PR depois que todas as verificações forem aprovadas.

5. Antes de conceder permissão de mesclagem, revise o diff do PR e as evidências de verificação.
6. Selecione a seta ao lado de **Agent Merge**.
7. Confirme se a opção **Merge pull request** está marcada.
8. Peça explicitamente ao agente que faça a mesclagem:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios informados. Antes de continuar, confirme se o status do PR é **Merged**. A permissão de mesclagem habilitada ou um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- inspecionou todo o PR da funcionalidade e as evidências de verificação;
- autorizou o Agent Merge somente depois da revisão e confirmou a mesclagem do PR.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma maneira mais rica de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
