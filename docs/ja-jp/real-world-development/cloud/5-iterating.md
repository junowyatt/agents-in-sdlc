---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

このラボでは、GitHub Copilot と協力してユーザー体験を改善し、機能を追加してきました。コードのドキュメント追加、関連ゲーム機能の実装、ハイコントラストとライトモードを含むアクセシビリティ機能を依頼しました。変更を確認し、必要に応じて Copilot に修正を依頼しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チームメンバーがレビューできるようにする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督なしで非同期に作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みはコード用ブランチに限られます。
- cloud agent は GitHub Actions 内に一時的な独立環境を作成して実行されます。
- GitHub Actions のワークフローは、人が承認してから実行されます。
- [外部リソースへのアクセスは既定で制限][agent-firewall]されています。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初の pull request (PR)、コードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスを既定で制限されているため、これは**想定どおり**です。必要であれば、Copilot cloud agent の[ファイアウォールを設定または無効化][agent-firewall]できます。

1. 新しいブラウザータブで github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は作業の完了を待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントなど、追加されたドキュメントを確認します。正確な変更内容は異なります。レビューでは、TSDoc、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 で確認したカスタム命令ファイルの内容に基づいています。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. ワークフローが PR のチェックに追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR で Copilot とやり取りする方法は、レビューだけではありません。チームメンバーに依頼するのと同じように、PR やコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は予測できないため、必ずしも特定の文面が必要なわけではありません。たとえば、次のような改善を依頼できます。

- 各コードファイルの先頭に、ファイルの概要を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使って、PR に変更を依頼するコメントを追加し、ユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![pull request に戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで改善を繰り返します。準備ができたら、PR を Draft から Ready に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェックにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて関連ゲーム機能を手動テストできます。Codespace のターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot は関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、命令を更新する前に割り当てた作業にコメントヘッダーと TSDoc コメントを追加したりできます。PR の **Conversation** タブにコメントすると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でミッションコントロールから依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は作業の完了を待ちます。数分かかる場合があります。

4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持する local storage の利用
   - ハイコントラストとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード
6. 内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェックにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace のターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリを起動してブラウザーでハイコントラストとライトモードの切り替えを試し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装を導いたことを確認します。改善が必要な点があれば、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼できます。

## 任意の演習 - さらに作業を委任する

cloud agent に実際のバックログ作業を任せ、その間に別の作業を進める方法を練習します。リポジトリに Issue を追加して Copilot に割り当ててみましょう。次のようなアイデアがあります。

- ゲーム詳細ページに支援者向けの問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ワークショップを完了しました。各レッスンでは、次の作業を行いました。

- このリポジトリに含まれるカスタム命令ファイルを確認し、cloud agent の出力に与える影響を確認した。
- GitHub Copilot cloud agent に Issue を割り当て、非同期に環境を準備し、計画して作業する様子を確認した。
- アクセシビリティ用カスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替え機能を追加した。
- Copilot agents ページをミッションコントロールとして使い、作業中にアクセシビリティのセッションを監視して調整した。
- Copilot cloud agent の pull request をレビューし、**@copilot** をメンションして変更を依頼し、ワークフローを承認した。

## レビューと次のステップ

Cloud agent ワークショップを完了しました。さらに学びたい場合は、他のワークショップも確認してください。

- **[VS Code ワークショップ](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を試します。
- **[CLI ワークショップ](../../cli/)** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの2つの変更をリリースします。

自分のリポジトリでは、次のアイデアを試してみましょう。

- リファクタリングやテスト範囲の拡張に関する Issue を cloud agent に割り当てる。
- 別の分野に特化したカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
