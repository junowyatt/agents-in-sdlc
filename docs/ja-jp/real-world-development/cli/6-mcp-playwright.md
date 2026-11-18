---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能をテストします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が追加のツールを使えるようにすることも必要です。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスも利用できます。

このレッスンでは、次の内容を学びます。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解する。
- まだ利用できない場合は Playwright MCP サーバーを追加する。
- エージェントにブラウザーを操作させ、フィルター機能を確認する。

## シナリオ

単体テストや E2E テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Web サイトを使うツールを Copilot に与えることで、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部のツールやサービスと通信できます。最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

MCP サーバーは、エージェントと外部のツールやサービスとの間をつなぎます。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、pull request を管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにもさまざまな MCP サーバーがあります。GitHub が公開している [MCP registry][mcp-registry] から、サーバーを見つけたりエコシステムに貢献したりできます。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定している場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類 **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP の画面を閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動くことを手動で確認しました。次は Copilot が利用できる Playwright MCP サーバーを使って、同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って、Issue と計画に照らしながらフィルター機能を確認してください。変更は加えず、期待どおりに動く点と、そうでない点を報告してください。確認が終わったら、自分で起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーの利用方法を自分で判断するため、プロンプトで指定する必要はありません。ただし、使用するツールが分かっている場合は明示すると、結果が安定し、不要なトークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を見ます。
3. 報告を読み、機能が期待どおりに動くことを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。このレッスンでは、次の内容を学びました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法
- Playwright MCP サーバーが未設定の場合に追加する方法
- エージェントにブラウザーを操作させてフィルター機能を確認する方法

次は[QA カスタムエージェントを作成し][next-lesson]、スキルとブラウザーツールを専門の役割にまとめます。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
