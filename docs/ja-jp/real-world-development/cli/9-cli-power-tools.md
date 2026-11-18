---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュコマンドがあります。高度な機能、内部の情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の内容を学びます。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の主なワークフローを完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑な作業では、モデルの最大コンテキストウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動で圧縮します。また、スラッシュコマンドを使って内容を確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分のタイミングで圧縮する場合は `/compact` を使います。関係のない別の作業に切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルの一覧を表示し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールをより効果的に使えます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで `/share` が利用できる方法を確認します。
2. セッションを共有する場合は、ローカルの Markdown へのエクスポートに使う `/share file` など、目的に合った共有先を選びます。
3. 誰かに送信したり公開したりする前に、エクスポート内容を確認します。セッションにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業の並列化や委任

ワークショップの主な内容は完了しました。Copilot CLI には、大規模な作業に使える次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分配して並行実行します。
- `/worktree` は、別の作業用に分離された Git worktree を作成します。
- `/delegate` は、Copilot cloud agent に作業を送り、非同期で進めて pull request を作成できるようにします。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試すときは、範囲の明確な新しい作業から始め、通常のワークフローで結果をレビューしてください。非同期エージェントについて詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の状態を確認したりできます。このレッスンでは、次の内容を学びました。

- `/context` と `/compact` で、Copilot のコンテキスト管理を確認した。
- `/model` で使用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。最後に[学んだ内容を振り返り][next-lesson]、次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
