---
title: "レッスン 2 - 星評価の追加: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、差分をレビューし、Codespaces のブラウザーで確認して、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次の内容を学びます。

- 機能ブランチで Copilot との会話を開始する。
- プロジェクトに小さな変更を加えるようエージェントに依頼する。
- `/diff` で変更をレビューする。
- アプリを起動し、Codespaces で転送されたブラウザーから確認する。
- 最初の pull request を作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小さく自己完結した変更なので、最初のセッションに最適です。

## 会話の構成

**会話**では、Copilot CLI と一緒に作業を進めます。Copilot app とは異なり、通常の CLI 会話では、専用の worktree は作成されません。現在ターミナルでチェックアウトしているリポジトリと Git ブランチを使います。保存した会話に後から戻ることもできます。ファイルとブランチは通常の Git の状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルの調査や編集に使ったツールの動作、`/diff` で確認できる変更の3つを確認できます。

## 会話を開始して変更を依頼する

新しい会話を開始して、機能を実装します。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot がまだ起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   一覧ページのゲームカードに、各ゲームの starRating を5点満点で表示してください。評価が null の場合は "No rating yet" と表示します。カードのレイアウトは変更せず、テストを追加して、関連するチェックを実行してください。
   ```

Copilot がプロジェクトを調査し、ゲームカードの表示に使うファイルを見つけて、必要なコードを作成します。これで Copilot CLI を使った新機能の追加は完了です。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot CLI 内で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方のケースを確認していることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して diff 画面を終了します。

> [!NOTE]
> Copilot は他の生成 AI ツールと同様に確率的に動作するため、生成されるコードは異なる場合があります。特定の実装と一致するかではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動くと判断せず、Copilot に Web サイトを起動させ、Codespaces から転送されたブラウザーで UI を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   星評価の変更をブラウザーで確認できるよう、アプリを起動してください。URL を教えて、サーバーは起動したままにしてください。
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲームカードに5点満点の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   起動した開発サーバーを停止してください。
   ```

## 最初の pull request を作成してマージする

機能を追加できました。次は、新しいコードをプロジェクトにマージするための pull request (PR) を作成します。

1. 既定のエージェントに、変更をコミットするよう依頼します。

   ```plaintext
   レビュー済みの星評価の変更を、適切なコミットメッセージでコミットしてください。
   ```

2. `/pr create` を実行します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` を実行して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って、最初の変更をリリースしました。具体的には、次の作業を行いました。

- 機能ブランチで Copilot との会話を開始した。
- ゲームカードに小さな変更を加えるようエージェントに依頼した。
- `/diff` で変更をレビューした。
- アプリを起動して、転送されたブラウザーで星評価を確認した。
- 最初の pull request を作成してマージした。

次は、[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
