---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定の作業や分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業に必要な指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に与えます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストを維持するテストエージェントなど、チームの知識を再利用できるエージェントにまとめられます。

カスタムエージェントは、プロジェクト内の `.github/agents`、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整しながら作業できます。このラボのアクセシビリティエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、pull request のワークフロースキルや、テストを実行・管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせてテストと改善を繰り返してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次の内容を学びます。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを利用できるようにすることを重視しています。ユーザーから、文字色と背景色のコントラストが不十分で、現在のダークテーマが読みにくいという意見が寄せられました。このアクセシビリティの問題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成しましょう。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに用意されています。内容を確認して、Copilot をどのように導くかを理解しましょう。

Codespace に戻り、アクセシビリティエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な役割
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と避けるべき点

## Issue を作成して割り当てる

ミッションコントロールは、環境内のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、追加の指示を送れます。まず、ハイコントラストモードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の画面を開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。ユーザーがハイコントラストを切り替えられるようにし、ブラウザーの local storage に設定を保存してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当て画面を開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントの割り当て画面で、Accessibility agent を選択する箇所を示したスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定の作業や分野に合わせた AI アシスタントです。チームの知識や標準を再利用できる形にまとめ、Copilot が作業を効果的に進められるように導けます。

この演習では、次の内容を学びました。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て

Copilot がハイコントラストモードを実装している間に、ミッションコントロールから[エージェントのセッションを監視して調整します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [Organization でカスタムエージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
