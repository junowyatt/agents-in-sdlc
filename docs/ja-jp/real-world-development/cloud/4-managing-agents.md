---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザー体験の改善と機能追加を目的とする3つの作業を GitHub Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。このミッションコントロールでは、未完了の pull request (PR) があるエージェントタスクを一覧できます。実行中の作業を確認し、セッションに追加の指示を送って進め方を調整することもできます。

このレッスンでは、次の内容を学びます。

- エージェントページで cloud agent のタスクを監視する。
- 作業中のセッションに指示を追加し、機能を拡張する。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが自然です。エージェントにライトモードも追加するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような内容があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための各種作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側の pull request (PR) ペインも確認します。PR と変更されたファイルを確認し、作業を監視できます。

## cloud agent に追加の指示を送る

実行中のタスクを確認できたので、ハイコントラストモードに取り組んでいる Copilot にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの作成に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールに表示されたアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードの作業に加えて、ライトモードも追加してください。ユーザーが表示モードを選べるように、ライトモード用の切り替えも追加してください。
    ```

    ![Agents ページで、作業中の cloud agent に追加指示を送るダイアログを示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が追加の指示を認識し、作業に反映することを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼をワークフローに組み込みます。

今回も数分かかります。待つ間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot agents ページを使って GitHub Copilot cloud agent のタスクを監視し、ガイドする方法を確認しました。ミッションコントロールでは、実行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

この演習では、次の内容を学びました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視する。
- 作業中のセッションに追加の機能を依頼する。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成した pull request を[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
