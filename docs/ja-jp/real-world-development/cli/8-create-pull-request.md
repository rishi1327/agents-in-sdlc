---
title: "レッスン 8 - 機能の PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示とスキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、pull request を開きます。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示、スキル、QA エージェントの変更を複数の PR に分けます。ワークショップを簡潔にするため、フィルター機能と品質に関する一連の作業を1つの会話とブランチにまとめています。

このレッスンでは、次の作業を行います。

- Agent Merge とマージのライフサイクルを自動化する仕組みを学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は pull request を取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR の阻害要因 (失敗した継続的インテグレーション (CI) チェックやレビューコメントへの対応、必要に応じたリベースなど) を解決し、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge に作業を任せることもできますが、有効にする前に内容をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

すべてのコードをまとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. PR を開き、変更内容とチェック結果をレビューします。レビューが完了してから続行します。
6. `/pr agentmerge` で Agent Merge を有効にします。
7. Copilot が PR の継続的インテグレーションを監視します。すべての条件が満たされると、マージが実行されます。
8. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されていればレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge とマージのライフサイクルを自動化する仕組みを学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、オプションのクラウドへの委任に役立つ[Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
