---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加を目的とする複数のタスクで、GitHub Copilot と協力してきました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を作成し、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。そこで、残りのチーム向けにレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能を作成する GitHub Copilot の提案を確認します。
## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監督なしでタスクを実行するため、安全を確保するためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、コード用のブランチに対してのみ書き込みアクセスを持ちます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されており](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)、Model Context Protocol (MCP) サーバーも対象です。
## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初の pull request (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、pull request の一覧を開きます。
3. **Add missing documentation** などのタイトルの pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request に **[WIP]** フラグが表示されます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。休憩を取ったり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択して、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 更新されたコードを確認します。新しい TSDoc のドキュメントコメントや、その他のドキュメントが含まれます。具体的な変更内容は状況によって異なります。

    変更を確認する際は、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. pull request のチェック欄にワークフローがキューに追加されたことを確認します。すべて順調に進めば、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

pull request で Copilot と作業するのは、一方通行ではありません。チームメンバーに依頼する場合と同じように、pull request のコメントやコードのインラインコメントで Copilot をタグ付けできます。Copilot はコメントを確認し、指摘に対応するためのセッションを新たに開始します。結果は予測できないため、具体的な依頼文をあらかじめ指定することはできません。たとえば、次のような変更を Copilot に依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加します。
- Astro アプリの構造を説明する README を作成します。

1. 上記の例のいずれか、またはコードベースに追加したいドキュメントに関する別の提案を使い、**@copilot** をタグ付けして、生成されたドキュメントへの変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が新しいセッションを開始して更新する様子を確認してください。
3. **Back to pull request** を選択すると pull request に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更をレビューします。

納得できるまで反復的に改善できます。問題がなければ、ドラフトの PR をレビュー可能な状態に変更して、`main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. pull request のチェック欄にワークフローがキューに追加されたことを確認します。すべて順調に進めば、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```
   
Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に新しいコメントを追加して更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます (この作業は、カスタム指示を更新する前に依頼したことを思い出してください)。**Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタムエージェントを使って実装したアクセシビリティ機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission control で依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request に **[WIP]** フラグが表示されます。Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用のユーザーインターフェイス (UI) コンポーネント。
   - ユーザー設定を維持するためのローカルストレージの使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. pull request のチェック欄にワークフローがキューに追加されたことを確認します。すべて順調に進めば、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティのカスタムエージェントが、アクセシビリティのベストプラクティスに従って Copilot の機能実装をどのように支援したか確認してください。アクセシビリティに懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をタグ付けして更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログの項目を任せ、その間に別の作業に取り組むと効果的です。この習慣を身につけるために、リポジトリにさらにいくつか Issue を作成し、Copilot に割り当ててみてください。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次の作業を行いました。

- このリポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力にどのように反映されるかを確認しました。
- Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ向けのカスタムエージェントを作成して使用し、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot エージェントページを Mission control として使い、実行中のアクセシビリティセッションを監視して誘導しました。
- cloud agent の pull request をレビューし、`@copilot` をタグ付けして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き学ぶ場合は、ほかのハーネスで今回の内容をさらに掘り下げられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) から Copilot Chat のエージェントモードと MCP の統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — コマンドラインインターフェイス (CLI) から、Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、オプションの `/delegate` による cloud agent への連携を使って、レビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
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
