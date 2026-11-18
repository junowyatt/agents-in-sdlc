---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能の要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人がレビューできるドラフトの pull request (PR) を作成します。面倒な作業を委任し、開発者がより重要な作業に集中できるようにします。

この演習では、Copilot cloud agent を使って次のことを学びます。

- コード生成の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- Issue の範囲を明確に定義する重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、改善したい技術的負債があります。最初のサイトを作成した開発者がドキュメントを十分に追加しておらず、エクスポートされた関数に TSDoc コメントがありません。まず、アプリケーション内のエクスポート関数に TSDoc コメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーの他のゲームを表示する関連ゲーム機能を追加し、UX の受け入れテストに使えるようにします。この作業はブロッカーですが、現時点では優先度の高い Issue がほかにあります。

こうした作業は優先順位が下がりやすいため、Copilot cloud agent に任せるのに適しています。Copilot が非同期に作業している間、開発者は別の作業に集中できます。後から戻り、Copilot の成果を確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると Copilot は進行状況を追跡するドラフト PR を作成し、作業環境を準備してタスクを開始します。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、PR で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot は魔法ではなく、考える作業を省略できるわけではありません。簡単に見える操作にも、掘り下げると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、適宜調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と Copilot への影響を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで簡単に確認します。cloud agent に作業を依頼する前に、このリポジトリに含まれる命令ファイルを読むだけにとどめ、後で出力への影響を確認できるようにします。

GitHub Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この命令は `**/*.test.ts` に適用されます。

次の演習で **Code lacks documentation** Issue を cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript の規約、コメントヘッダーがあるかを確認します。これらの命令ファイルから反映される内容は、後の演習で再びレビューします。

## Copilot cloud agent の開発環境を設定する

コードの作成では、必要な開発ツールや依存関係を使えるように環境を準備します。Copilot にタスクを割り当てる場合も同様です。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に[セットアップワークフロー][setup-workflow]を作成すると、必要なツールや依存関係を事前にインストールできます。このラボ用のワークフローは、環境を準備し学習を進めやすくするため、あらかじめ設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、マイグレーションと初期データ投入済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップを手動でテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot は自身のトークンを操作に使います。セットアップ手順に必要な最小限の権限だけを設定してください。
    permissions:
      # たとえば依存関係のインストールなどでセットアップ手順からリポジトリをクローンする場合は、`contents: read` が必要です。クローンしない場合は、セットアップ完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Node.js を使ったフロントエンド / アプリのセットアップ
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

      # ローカルの SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub Actions ワークフローと似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、セットアップが成功するか確認できます。

## ドキュメントを追加する

ドキュメントが重要だと分かっていても、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。これは開発を遅らせ、コードベースの保守や新しいメンバーの受け入れを難しくする技術的負債です。Copilot はドキュメントの作成に役立つため、cloud agent に任せるのに適した作業です。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の画面を開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    組織では、役に立つ場合に関数とメソッドに TSDoc コメントを付けることを求めています。しかし、最近の更新ではこの標準に従っていません。動作を明確にするために必要な箇所に、既存コードのドキュメントコメントを追加してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

    ![Issue に Copilot を割り当てる操作](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が PR に todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかる場合があります。前述のワークフローを実行して環境を準備し、プロジェクトを分析して、作業方法を検討しているためです。

13. todo リストと、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッションビューを開くボタン](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. 実行中のセッションを確認します。Copilot はコードを調査して状態を把握し、計画を立て、コードを作成します。

非同期で作業できることは、Copilot cloud agent の主な利点です。別の作業に集中できるよう、もう1つタスクを cloud agent に割り当て、その間にアプリに機能を追加します。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点は、作業を分けられることです。デザインチーム向けの関連ゲーム機能に取り組む間に、別の作業を進められます。cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示して、ユーザーが他のゲームも見つけられるようにします。デザインチームは UX を確認し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータアクセスヘルパーを追加し、指定したゲームを除く同じカテゴリーのゲームを返す
   - ヘルパーを使って、ゲーム詳細ページに "Related games" セクションを表示する
   - 関連ゲームがない場合に対応する
   - 新しいヘルパーの単体テストを作成する
   - PR を作成する前に、すべてのテストが成功することを確認する
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

    ![Issue に Copilot を割り当てる操作](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに目の絵文字が表示されると、Copilot が作業を開始したことを示します。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。cloud agent は開発者と同様に作業するため、完了するまで常に監視する必要はありません。後で結果をレビューします。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当て、非同期に作業させることができます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などに利用できます。

この演習では、次の内容を学びました。

- コード生成の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- Issue の範囲を明確に定義する重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
