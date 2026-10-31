---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、変更漏れがあったファイルを見つけて、さらに更新することになります。作業は次々に増えていきます。

Copilot Agent Mode は、こうした作業に役立ちます。

Copilot Agent Mode は統合開発環境 (IDE) 内でより自律的に動作します。開発者と同様に、まず既存のプロジェクト構造を調べ、必要な変更を加え、テストなどのタスクを実行し、見つかった間違いを自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる利用体験を提供します。エージェントモードは IDE 内でタスクを実行するため、すばやいフィードバックと対話が可能です。Cloud agent はチームメンバーのように非同期で作業し、Issue や pull request (PR) を通じてやり取りするペアプログラマーとして設計されています。

この演習では、次の方法を確認します。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更します。
- GitHub Copilot Agent Mode でユーザーインターフェイス (UI) とデータレイヤーにまたがる新機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習ですでにパブリッシャーヘルパーを追加しました。次は、Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を理解するために Tailspin Toys の Web サイトを確認します。

この Web サイトは、ボードゲーム向けの開発者テーマのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を確認できます。このサイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページはフロントマターで Drizzle ORM を通じてローカルの SQLite データベースを直接クエリします。独立したバックエンドのアプリケーション・プログラミング・インターフェイス (API) やクライアント側ユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブ機能には標準の DOM API を使う小規模な Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内で新しいターミナルウィンドウを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーをホストする Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトを作成した際に Issue のバックログも自動的に作成されました。GitHub Copilot にバックログを表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は自律的に計画を立て、複数のファイルにわたって変更を実装し、ターミナルコマンドを実行して、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。このワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。安定した体験を得るために、比較的一貫したコード提案が得られることを期待しています。ただし、LLM は確率的に動作するため、説明と異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を基準に、優先順位も付けてくれたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に提供する指示ファイルを確認します。Web サイトに機能を追加する方法が記載された [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加のガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - 次のようなファイルの種類別の指示へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルとプロジェクト内のファイルの両方を参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用しやすく、小さく管理しやすい単位に分け、コード生成時に参照する例を示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、アプリケーションに少なくとも 3 つの更新が必要です。

- データレイヤー (`src/lib/`) にフィルタリングロジックを追加または調整します。
- フィルタリングの動作をテストするテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを指定します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 引き続き **Claude Sonnet 4.5** がモデルとして選択されていることを確認します。
5. バックログにある関連 Issue の要件に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを特定する様子を確認します。データレイヤーのヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示される場合があります。Copilot が作業を完了すると、すべての変更に対して **Keep** または **Undo** を選べるようになるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示された画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode のチャットセッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を確認します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次の作業を行いました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装しました。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更しました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューしました。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### おまけの演習 — ページネーションを実装する

ゲーム一覧が増えるにつれて、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションの実装を依頼してみてください。コードに関する検討事項は次のとおりです。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ユーザーがページサイズを選べるようにするか、ページサイズを固定するかなど、ページネーションの実装方法を検討します。
- 希望する実装に必要なガイダンスを Copilot に提供するプロンプトを作成します。
- Copilot に変更を依頼し、コンテキストを提供して反復的に改善する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
