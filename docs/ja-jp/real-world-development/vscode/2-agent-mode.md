---
title: "演習 2 - Copilot agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリの小さな更新でも、複数ファイルの変更やテストの実行が必要になることがあります。開発者は通常、必要なファイルを探し、変更し、テストを実行し、問題を調査し、変更し忘れたファイルを見つけて、さらに更新するという手順を繰り返します。

Copilot Agent Mode を使うと、この作業を進めやすくなります。

Copilot Agent Mode は、IDE 内でより自律的に作業します。プロジェクトの構造を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途の機能です。agent mode は IDE 内で動作し、すばやいフィードバックと対話を行います。cloud agent はチームメンバーのように非同期で作業し、Issue や pull request を通じてやり取りします。

この演習では、次の内容を学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて、複数の変更を調整する方法
- GitHub Copilot Agent Mode が UI とデータレイヤーにまたがる新機能を実装する方法
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法

## シナリオ

ゲームが増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習でパブリッシャー用ヘルパーを追加しました。残りのデータレイヤー、UI、テストを Copilot Agent Mode で完成させます。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリであり、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使う小規模な Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. Codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. Codespace 内で新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーの URL `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーを実行している Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに、Issue のバックログも自動的に用意されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンで **Agent** を選択します。Agent は自律的に計画を立て、複数ファイルを変更し、ターミナルコマンドを実行してツールを使います。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは、特定のモデルを推奨するものではありません。作成時には Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高める意図がありますが、LLM は確率的に動作するため、結果が異なる場合があります。これは通常の動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP コマンドを使う場合がありますが、同じタスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   GitHub リポジトリのバックログを表示してください。ユーザーに最も役立つものを基準に優先順位を付けてください。
   ```

4. **Continue** を選択して、すべての Issue を一覧するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーに役立つと考えられる項目に基づいて優先順位も付けます。

## 命令ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える命令ファイルを確認します。ここでは、Web サイトに機能を追加する方法を説明する [user interface (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの命令へのリンク

> [!TIP]
> 命令ファイルから、他の命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルからの相対パスです。命令を再利用しやすい小さなファイルに分け、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルターロジックを追加または調整する。
- フィルターの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 命令ファイルを追加します。

   ![UI 命令ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   関連する GitHub Issue の要件に基づいて、サイトにパブリッシャーとカテゴリーによるフィルターを追加してください。前の演習で publishers ヘルパーを作成済みなので、必要に応じて維持または調整しながら、残りのデータレイヤー、UI、テストを完成させてください。完了前にすべてのテストが成功することを確認してください。サーバーはすでに起動しているため、起動し直す必要はありません。
   ```

6. Copilot がプロジェクトを調査し、機能に関係するファイルを探します。データレイヤーのヘルパー、UI、テストを見つけて、ファイルの変更とテストの実行を始めます。

   ![Copilot がプロジェクトファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、複雑さによっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。作業が完了すると、すべての変更に対して **Keep** または **Undo** を選べるので、処理中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat のテスト実行確認ダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の会話が完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を確認します。
11. すべて動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。

- GitHub Copilot Agent Mode が UI とデータレイヤーにまたがる新機能を実装する方法
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて変更を調整する方法
- コードベースにマージする前に、生成された変更とテストをレビューする方法

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### ボーナス演習 - ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションを追加するよう依頼します。次の点を検討してください。

- 既存の命令ファイルを使うなど、現在のベストプラクティスに従う。
- ページサイズをユーザーが選べるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- Copilot に変更やコンテキストを伝えながら、反復して調整する。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent mode と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
