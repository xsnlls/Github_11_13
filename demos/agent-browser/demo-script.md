# agent-browser Live Demo Runbook / 现场演示流程

- **Project / 项目:** [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser): "Browser automation CLI for AI agents"
- **Why now / 上榜数据:** GitHub weekly trending, week of 2026-11-13: about 50.4k stars, **+4,601 this week** / 2026-11-13 当周周榜：约 5.04 万 star，**本周新增 4,601**
- **Demo style / 演示方式:** the project runs unchanged; you type **one sentence** into its built-in dashboard Chat and the audience watches the browser do it live. / 项目原样运行；你在它自带面板的聊天框里输入**一句话**，大家实时看着浏览器完成。
- **Platform / 平台:** Windows (x64), PowerShell
- **Length / 时长:** about 10 minutes / 约 10 分钟
- **Tested / 已验证:** agent-browser 0.43.0. The dashboard, live view, sessions, and tabs were tested. **The AI chat itself was not tested (no key) — rehearse it.** / 面板、实时画面、会话和标签页已测试。**AI 聊天本身未测试（没有 key），务必彩排。**

---

## The idea in one picture / 一图看懂

```
PowerShell (2 commands, just to start)
        │
        ▼
Browser tab: http://localhost:4848   ← share only this tab / 只共享这个标签页
┌──────────┬──────────────────────────┬───────────────────────────┐
│ Sessions │  Live browser view       │  Chat  | Activity | ...   │
│ 会话列表  │  实时浏览器画面            │  聊天框 | 操作记录 | ...    │
└──────────┴──────────────────────────┴───────────────────────────┘
```

---

## Part A: Before the meeting / 会前准备

**1. Install Node.js (LTS)** from https://nodejs.org.
**1. 安装 Node.js（LTS 版）**：https://nodejs.org

**2. Install agent-browser and Chrome for Testing**, then check that everything passes (in PowerShell):
**2. 安装 agent-browser 和 Chrome for Testing**，然后检查各项是否通过（在 PowerShell 中）：

```powershell
npm install -g agent-browser
```

```powershell
agent-browser install
```

```powershell
agent-browser doctor
```

**3. Get an AI Gateway key.** Create a Vercel account, then create an AI Gateway API key. No Vercel project or deployment is needed. The free tier includes **$5 of credit per month**, but only for a subset of models; other models need purchased credits.
**3. 获取 AI Gateway key。** 注册 Vercel 账号，创建 AI Gateway API key。不需要 Vercel 项目或部署。免费额度为**每月 5 美元**，但仅限部分模型；其他模型需要购买额度。

**4. Save the key permanently**, then **close and reopen PowerShell**:
**4. 永久保存 key**，然后**关闭并重新打开 PowerShell**：

```powershell
setx AI_GATEWAY_API_KEY "your_key_here"
```

**5. Rehearse Part B once, end to end.** Check which model the dashboard's model picker uses, and make sure it works with your credits.
**5. 完整彩排一遍 Part B。** 检查面板模型选择器用的是哪个模型，确认你的额度可以使用它。

**6. Record the rehearsal as a backup video** with the Windows Snipping Tool (screen recording) or Xbox Game Bar (`Win + Alt + R`).
**6. 把彩排录成备份视频**，可以用 Windows 截图工具的屏幕录制，或 Xbox Game Bar（`Win + Alt + R`）。

**Checklist / 检查清单**

- [ ] `agent-browser doctor` shows no failures / 没有失败项
- [ ] The key works: a test sentence in Chat gets a response / key 可用：在聊天框里试一句能得到回应
- [ ] Rehearsal done, and you know roughly how long each sentence takes / 已彩排，大致知道每句话要多久
- [ ] Backup video saved / 备份视频已保存
- [ ] Notifications off, browser zoom set so the back row can read / 关闭通知，调好浏览器缩放保证后排看得清

---

## Part B: The live demo / 现场演示

### Step 0: Opening (1 min) / 开场（1 分钟）

**Say / 讲解:**

> For an AI to use a website, it has to "see" the page first. Screenshots are expensive and imprecise; raw HTML is far too long. agent-browser, from Vercel Labs, turns a page into a short list of elements, each with a ref, and lets an AI act on them. It's open source, written in Rust, and gained over 4,600 stars this week. Today I won't type a single command: I'll just tell it what I want.
>
> AI 要使用网站，首先得"看见"页面。截图成本高、不精确，原始 HTML 又太长。Vercel Labs 的 agent-browser 把网页转成一份简短的元素清单，每个元素带一个引用，让 AI 直接操作。它开源、用 Rust 编写，本周新增 4,600 多个 star。今天我一条命令都不敲，只告诉它我想要什么。

### Step 1: Start the dashboard (1 min) / 启动面板（1 分钟）

In PowerShell / 在 PowerShell 中：

```powershell
agent-browser dashboard start
```

```powershell
agent-browser open https://demo.playwright.dev/todomvc
```

Open `http://localhost:4848` in Chrome or Edge and **share that tab**. Briefly point out the three areas: sessions on the left, the live browser in the middle, Chat and Activity on the right.
在 Chrome 或 Edge 中打开 `http://localhost:4848`，**共享这个标签页**。简单介绍三个区域：左边会话，中间实时浏览器，右边聊天和操作记录。

### Step 2: First sentence (2 min) / 第一句话（2 分钟）

Type into Chat / 在聊天框输入：

> Add three to-dos: Prepare demo, Rehearse, Present at meeting. Then mark the first one as done.

Then open the **Activity** tab.
然后打开 **Activity** 标签页。

**Say / 讲解:**

> Here's what the AI actually did: it took a `snapshot` to see the page, got refs like `e3` for the input box, then ran `fill`, `press Enter`, and `check`. After each action it looks again, because the page has changed. That loop — look, act, check — is the core of agent-browser.
>
> 这就是 AI 实际做的事：先用 `snapshot` 看页面，拿到输入框的引用（比如 `e3`），然后执行 `fill`、`press Enter`、`check`。每次操作后它都会重新看一遍，因为页面已经变了。"看 → 做 → 检查"这个循环，就是 agent-browser 的核心。

### Step 3: Second sentence (1 min) / 第二句话（1 分钟）

> Show only the completed to-dos, then take a screenshot.

**Say / 讲解:**

> Besides clicking and typing, it can capture evidence: screenshots, PDFs, even video. When an AI tests a site for you, it can hand back proof of what it did.
>
> 除了点击和输入，它还能留下证据：截图、PDF，甚至视频。AI 帮你测试网站时，可以交回它做了什么的证明。

### Step 4: Third sentence — built-in QA skill (3 min) / 第三句话：内置测试 skill（3 分钟）

> Dogfood this app. Focus on adding, completing, and filtering to-dos, and give me a short report.

**Say / 讲解:**

> "Dogfood" is one of the project's 10 built-in skills. It tells the AI how to explore an app like a QA tester and write a report with screenshots and reproduction steps for every issue.
>
> "Dogfood" 是项目自带的 10 个 skill 之一。它教 AI 像测试工程师一样探索应用，并为每个问题写出带截图和复现步骤的报告。

> ⚠️ This may take a while. If it runs long in rehearsal, narrow the focus further or skip this step. / 这一步可能比较久。如果彩排时太慢，就进一步缩小范围，或者跳过这一步。

### Step 5: Feature overview (2 min) / 功能总览（2 分钟）

Click through the **Console**, **Network**, and **Storage** tabs, then summarize:
依次点开 **Console**、**Network**、**Storage** 标签页，然后总结：

- **Operate pages / 操作网页:** click, fill, select, drag, upload, download, tabs, iframes, pop-up dialogs
- **Capture and compare / 记录与对比:** screenshots (including numbered labels for vision models), PDF, video, page and screenshot diffs
- **Test and debug / 测试与调试:** console errors, network mocking, Core Web Vitals, accessibility audits, React component tree
- **Stay logged in / 保持登录:** parallel sessions, saved state, a login vault, reusing your Chrome profile
- **Works with any agent / 对接任何 agent:** Claude Code, Codex, Qoder, and others, via built-in skills or its MCP server (33 tools)
- **Safety / 安全:** domain allowlists, output limits, action policies

**Say / 讲解:**

> Today I used its built-in chat, but the same tool can be driven by any AI agent you already use. The AI is the brain; agent-browser is the hands and eyes.
>
> 今天我用的是它自带的聊天功能，但同一个工具也可以由你已经在用的任何 AI agent 来驱动。AI 是大脑，agent-browser 是手和眼。

### Step 6: Q&A / 问答

**Q: How is this different from Playwright or Selenium? / 和 Playwright、Selenium 有什么区别？**
> Those are libraries for writing test scripts in code. agent-browser is built for an AI's step-by-step loop: compact snapshots, short refs, and a browser that stays open between steps.
> 它们是用代码写测试脚本的库。agent-browser 专为 AI 的逐步循环设计：精简快照、简短引用，步骤之间浏览器保持打开。

**Q: Why not use Claude's or Qoder's built-in computer use? / 为什么不用 Claude 或 Qoder 内置的 computer use？**
> Built-in tools are great for interactive help while you're at the computer. agent-browser is better for automation: it runs anywhere (terminal, server, CI), works with any agent, uses far fewer tokens than screenshots, and can be repeated reliably. It only covers the browser, though, not the whole desktop.
> 内置工具适合你坐在电脑前时的交互式协助。agent-browser 更适合自动化：在哪都能跑（终端、服务器、CI），任何 agent 都能用，比截图省很多 token，还能稳定重复执行。不过它只管浏览器，不管整个桌面。

**Q: Does it cost money? / 收费吗？**
> The tool is free and open source (Apache-2.0). Only the built-in chat uses paid AI tokens through Vercel's AI Gateway, which has a $5/month free tier for some models.
> 工具本身免费开源（Apache-2.0）。只有内置聊天会通过 Vercel AI Gateway 消耗 AI token，部分模型有每月 5 美元的免费额度。

**Q: Do I need Vercel to use it? / 必须用 Vercel 吗？**
> No. Vercel is only needed for the built-in chat. Any agent, such as Claude Code or Qoder, can drive it without Vercel.
> 不需要。只有内置聊天需要 Vercel。Claude Code、Qoder 等任何 agent 都可以不经过 Vercel 直接驱动它。

---

## Appendix A: Troubleshooting / 故障排查

| Problem / 问题 | Fix / 解决办法 |
|---|---|
| Chat says `AI_GATEWAY_API_KEY not set` / 提示未设置 key | Run `setx` again, open a **new** PowerShell, then `agent-browser dashboard stop` and `agent-browser dashboard start`. / 重新执行 `setx`，打开**新的** PowerShell，再停止并重新启动面板。 |
| The live view is empty / 实时画面是空的 | Run `agent-browser open https://demo.playwright.dev/todomvc`. / 执行这条 open 命令。 |
| Chat reports the model is unavailable / 提示模型不可用 | Pick a free-tier model in the model picker, or buy credits. / 在模型选择器里换一个免费模型，或购买额度。 |
| The AI takes too long / AI 太慢 | Use a shorter, more specific sentence. / 换一句更短、更具体的话。 |
| Network or AI fails during the meeting / 开会时网络或 AI 出问题 | Play the backup video. / 播放备份视频。 |

## Appendix B: How big is it? / 项目有多大？

| Part / 部分 | Size / 大小 | Notes / 说明 |
|---|---|---|
| CLI (npm package) / 命令行工具 | ~21 MB | Measured after install. / 安装后实测。 |
| Chrome for Testing | ~400 MB (estimate / 估算) | Based on a comparable Chromium build. / 参照同类 Chromium 版本估算。 |
| Source code / 源代码 | ~33 MB | Not needed for the demo. / 演示用不到。 |

See also `feature-checklist.md` for every feature and its test status. / 全部功能及测试状态见 `feature-checklist.md`。
