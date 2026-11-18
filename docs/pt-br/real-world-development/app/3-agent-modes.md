---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem com base em uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas alterações maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para trabalhar com o fluxo existente da organização e ajudar você a criar os recursos certos da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com uma issue para gerar uma nova funcionalidade, confirmará se o código é válido e se o recurso se comporta como esperado e, por fim, fará a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma sessão ao longo do fluxo de trabalho da funcionalidade. Normalmente, haveria sessões ou PRs diferentes para os vários tipos de arquivo, mas vamos simplificar para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova sessão de agente com base em uma issue do GitHub;
- definir requisitos no modo **Plan**;
- implementar a funcionalidade no modo **Autopilot**;
- revisar o código;
- validar manualmente a funcionalidade em um canvas de navegador.

Ao continuar este trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo e as pessoas precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como combinar categorias precisam ser definidos antes de começar a programar. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação com escopo definido no modo Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. A maioria das pessoas desenvolvedoras segue um processo parecido com este:

1. Abrir uma issue registrada com os detalhes do trabalho necessário.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação no processo de integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria segue uma variação do processo acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo da sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo quando quiser:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano. Você revisa e aprova o plano antes da execução.
- **Autopilot**: o agente trabalha de forma totalmente autônoma: escreve código, executa testes e itera sem aguardar orientação.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Inicie uma sessão pela issue

Antes de começar, confirme se o PR da avaliação por estrelas foi mesclado e se o `main` local está atualizado.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada no backlog; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada no `main` atualizado.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue foi anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o `PATH` do aplicativo ou a configuração do gerenciador de versões e encerre e abra o aplicativo novamente. Repita a operação e as verificações com falha. Não reinstale o npm apenas porque o agente não consegue encontrá-lo.

## Planeje a funcionalidade de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você começou pela issue, o Copilot já tem a solicitação como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele abrange a filtragem por categoria e editora, os controles acessíveis, as alterações no acesso a dados e os testes. Esclareça qualquer comportamento incerto, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes unitários, testes de ponta a ponta e verificação de tipos com as ferramentas existentes do projeto. Mantenha o escopo focado na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e guarde a URL da issue e quaisquer esclarecimentos acordados para usar na validação.

## Aprove explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Verifique o modo ativo em vez de presumir que a aprovação o altera. Confirme se o indicador do modo mostra **Autopilot**.

O Copilot começará a implementação. Ele seguirá o plano acordado, gerará código e executará testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto, revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Encerre somente os servidores que você iniciou.

## Revise e verifique a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes da mesclagem. Vamos revisar o código e executar o site para confirmar se tudo está correto.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo as combinações de categorias e editoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Inspecione os resultados do lint, dos testes unitários, dos testes de ponta a ponta e da verificação de tipos. Uma verificação ignorada não significa aprovação.
4. Corrija as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração de testes de ponta a ponta do Playwright compila e serve uma visualização e pode reutilizar um servidor local. Confirme se o servidor testado pertence a esta worktree, e não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como fizemos antes e abrir o site em um canvas de navegador.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme se os resultados correspondem a essa editora.
4. Combine filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento acordado para a combinação.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que encerre o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem mescle o PR de filtragem. Mantenha esta sessão, a worktree e o branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias à skill e ao perfil de QA antes de criar e mesclar o PR combinado na Lição 8. Depois de encerrar o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma nova sessão de agente com base em uma issue do GitHub;
- definiu requisitos no modo **Plan**;
- implementou a funcionalidade no modo **Autopilot**;
- revisou o código;
- validou manualmente a funcionalidade em um canvas de navegador.

Em seguida, vamos examinar mais a fundo como o código é gerado e garantir que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
