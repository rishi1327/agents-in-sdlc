---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查结果，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次更改了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将让智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

本课将介绍如何：

- 在功能分支上启动专注于单项任务的 Copilot 对话。
- 让智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，详情页已经显示评分。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，你将让智能体在每张卡片上显示现有评分。这项更改小而独立，很适合作为第一次会话的任务。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，不会创建专用工作树。保存的对话可供日后继续使用，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你发送的提示和智能体的回复；智能体探索和编辑文件时执行的工具操作；以及可通过 `/diff` 检查的更改。

## 启动对话并请求更改

先启动新对话，再开始实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在列表页的游戏卡片中显示每款游戏的 starRating（满分为 5 分）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试，并运行相关检查。
   ```

Copilot 会探索项目，找到显示游戏详情所用的文件，并创建所需代码。现在，你已使用 Copilot CLI 添加了新功能。

## 审查差异

所有人工智能 (AI) 生成的更改都应在合并前审查，即使是小改动也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff`，检查每个更改过的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 审查 Copilot 运行的检查结果，并要求它修复失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此生成的代码可能不同。应审查实际行为，而不是要求某种固定实现。

## 检查更改

不要只看代码就假定功能正常。让 Copilot 启动网站，再通过 Codespaces 转发的浏览器检查更新后的用户界面 (UI)。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，并让它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已完成，现在可以创建拉取请求 (PR)，将新代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   提交已审查的星级评分更改，并使用恰当的提交说明。
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改。具体来说，你：

- 在功能分支上启动了专注于单项任务的 Copilot 对话。
- 让智能体修改了游戏卡片。
- 使用 `/diff` 审查了更改。
- 在转发的浏览器中运行应用并确认了星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选功能议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
