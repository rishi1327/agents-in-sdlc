---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチに合意し、Autopilot で Issue からフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大規模な変更には、堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存のフローに沿って作業できます。このレッスンから数回にわたり、Issue から新機能を作成し、コードと動作を検証して、プロジェクトにマージするエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能のワークフローでは、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは中心となる概念に集中できるよう、手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から Copilot との新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが充実し、訪問者がカテゴリーとパブリッシャーでゲームを絞り込める機能が必要になりました。バックログの Issue に機能は記載されていますが、カテゴリーの組み合わせ方などはコーディング前に合意が必要です。Plan モードで決定事項を整理してから、Autopilot による範囲を限定した実装を承認します。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。多くの開発者は、次のような流れで作業します。

1. 必要な作業の詳細が記載された Issue を開きます。
2. 実装する内容の計画を立てます。
3. コードを構築してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 正確な手順はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものです。

この標準的なアプローチに従うことで、AI が生成したコードが要件を満たし、人が作成したコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントの自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: 開発者とエージェントが共同で作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成し、プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントがコードを書き、テストを実行して、タスクが完了するまで自律的に反復します。

Plan モードで開始し、計画をレビューしてから、Autopilot で実装します。

## Issue から開始する

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトには `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の Issue や pull request (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てることで、Copilot に実装を任せる前にアプローチを決められます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプト下のモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調査し、実装方法を検討します。

3. フィルターの動作について質問されたら、希望する動作を回答します。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot で実装します。Autopilot は、タスクが完了したと判断するまで Copilot に反復させます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで表示される同等の項目を選択します。
2. プロンプト下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があるため、先に計画をレビューしてください。依存関係の不足やポートの競合が報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードも、他のコードと同じようにマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替え、モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容に照らし合わせます。
4. レビューが終わったら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を確認する

コードに問題がないように見えても、実際に動くか確認します。Codespaces の転送ポートを使ってサイトを開きます。

1. 次のプロンプトでアプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリーとパブリッシャーの絞り込み、およびその組み合わせを試します。
4. リセットと該当項目がない場合の動作が、Issue と決定内容に一致することを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

複数の会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot との新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]コードを文書化されたプラクティスに従わせる方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
