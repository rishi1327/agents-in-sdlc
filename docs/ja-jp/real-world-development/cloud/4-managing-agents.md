---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザーエクスペリエンスの改善や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。その 1 つが GitHub.com の[エージェントページ][agents-page]です。この Mission control では、オープンな pull request (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次の作業を行います。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションを誘導し、機能を追加で依頼します。

## シナリオ

エージェントにハイコントラストモードを作成するよう依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能を更新しているため、ライトモードも追加するのが合理的です。エージェントの作業を誘導し、ハイコントラストに加えてライトモードも追加できるようにします。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインの両方にあるタスク一覧を確認します。Copilot に割り当てた次のタスクが表示されます。
    - コードのドキュメントを更新する。
    - ゲームを変更するためのアプリケーション・プログラミング・インターフェイス (API) を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行したタスクを確認します。次のような操作が含まれることがあります。
    - リポジトリをチェックアウトする。
    - Copilot の作業環境を作成する。
    - Model Context Protocol (MCP) サーバーを設定する。
    - 割り当てられたタスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状況や選択した進め方によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。PR や変更されたファイルを表示し、進行状況をさらに監視できます。

## Cloud agent を誘導する

現在実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。Copilot が使うタイトルと現在の作業状況によって、具体的な名前は異なります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが示されるまで数分間セッションを確認します。以下のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、Steer active session while Copilot is working ダイアログが強調表示された cloud agent タスクの画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に追加したことを確認します。

## Copilot の作業を待つ

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業していた手順が終わると、新しい依頼を進行中の作業に組み込みます。

今回も数分かかります。これまでに学んだことを振り返りながら、少し待ちましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して誘導するための中心的な場所である Copilot エージェントページを確認しました。この Mission control では、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクに追加の指示を出せます。

次の内容を確認しました。

- Mission control とエージェントページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能に取り組んでいる間に、次は Copilot が作成した[pull request を反復的に改善します][next-lesson]。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
