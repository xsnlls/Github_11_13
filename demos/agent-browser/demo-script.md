# agent-browser Demo Script / 演示脚本

- **Project / 项目:** [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser): "Browser automation CLI for AI agents"
- **Why now / 上榜数据:** GitHub weekly trending, week of 2026-11-13: about 50.4k stars, **+4,601 stars this week** / 2026-11-13 当周 GitHub 周榜：约 5.04 万 star，**本周新增 4,601**
- **Tested with / 验证版本:** agent-browser 0.43.0. Every command in Parts 1–4 was run successfully before this script was written. / 第 1–4 部分的每条命令都在写脚本前实际跑通过。
- **Total time / 总时长:** about 12 minutes / 约 12 分钟

---

## Before the meeting / 会前准备

**Install (one time) / 安装（只需一次）**

You need Node.js. The second command downloads Chrome for Testing.
需要先装 Node.js。第二条命令会下载 Chrome for Testing。

```bash
npm install -g agent-browser
```

```bash
agent-browser install
```

**Checklist / 检查清单**

- [ ] Rehearse the whole script once, start to finish. / 从头到尾完整彩排一遍。
- [ ] Screen layout: terminal on the left, browser window on the right. / 屏幕布局：左边终端，右边浏览器窗口。
- [ ] Make the terminal font large enough for the back row. / 终端字体调大，保证后排看得清。
- [ ] Run `agent-browser close --all` so no old browser session is running. / 执行 `agent-browser close --all`，确保没有残留的浏览器会话。
- [ ] Keep backups from the rehearsal: `todo.png`, `page.pdf`, `demo.webm`. If the network fails, show these. / 保留彩排产出：`todo.png`、`page.pdf`、`demo.webm`。网络出问题就展示这些。
- [ ] If you'll do Part 5, check that Claude Code (or your agent) is logged in. / 如果要做第 5 部分，确认 Claude Code（或你的 agent）已登录。

---

## Part 0: Opening (1 min) / 开场（1 分钟）

**Say / 讲解:**

> For an AI to use a website, it first has to "see" the page. Screenshots are expensive and imprecise, and raw HTML is far too long. agent-browser, from Vercel Labs, takes a third path: it turns the page into a short list of elements an AI can read, gives each one a ref, and lets the AI act on those refs from the command line.
>
> AI 要使用网站，首先得"看见"页面。截图成本高、不精确，原始 HTML 又太长。Vercel Labs 的 agent-browser 走了第三条路：把网页转成一份 AI 能读懂的简短元素清单，给每个元素一个引用，让 AI 在命令行里对这些引用执行操作。

> It's written in Rust and is open source under Apache-2.0. It gained over 4,600 stars this week. Today I'll drive it by hand, so you see exactly what an AI agent does with it.
>
> 它用 Rust 编写，基于 Apache-2.0 开源，本周新增 4,600 多个 star。今天我会亲手操作它，让大家看清 AI agent 到底是怎么用它的。

---

## Part 1: The core loop (2 min) / 核心循环（2 分钟）

**1.** Open a page. `--headed` shows the browser window; without it, the browser is invisible.
打开网页。`--headed` 让浏览器窗口可见；不加的话浏览器在后台不可见。

```bash
agent-browser open example.com --headed
```

**2.** Take a snapshot. This is what the AI "sees."
获取快照。这就是 AI "看到"的内容。

```bash
agent-browser snapshot -i
```

Expected output / 预期输出:

```
- link "Learn more" [ref=e1]
```

**Say / 讲解:**

> This one line is the whole page, as far as the AI is concerned: one link, named "Learn more," with the ref `e1`. A screenshot of the same page would cost far more tokens and still need the AI to guess where to click.
>
> 对 AI 来说，这一行就是整个页面：一个名为 "Learn more" 的链接，引用是 `e1`。同一页面的截图要花多得多的 token，而且 AI 还得猜该点哪里。

**3.** Act on the ref, then check the result.
对引用执行操作，然后检查结果。

```bash
agent-browser click @e1
```

```bash
agent-browser get url
```

Expected / 预期: `https://www.iana.org/help/example-domains`

**Say / 讲解:**

> That's the whole loop: snapshot, act, check, repeat. The browser stays open between commands, so each step is one short command. That's exactly how an AI agent works with it.
>
> 这就是整个循环：快照、操作、检查、重复。命令之间浏览器一直开着，所以每一步只是一条短命令。AI agent 正是这样使用它的。

---

## Part 2: A real task with refs (3 min) / 用引用完成真实任务（3 分钟）

We'll use Playwright's TodoMVC demo page. It's a test site, so it's stable and has no ads or pop-ups.
我们用 Playwright 的 TodoMVC 演示页面。它是测试网站，很稳定，没有广告和弹窗。

**1.** Open the page and take a snapshot.
打开页面并获取快照。

```bash
agent-browser open https://demo.playwright.dev/todomvc
```

```bash
agent-browser snapshot -i
```

Look for / 找到这一行: `textbox "What needs to be done?" [ref=e3]`

**2.** Add a to-do. **Use the ref from your own snapshot**; it may not be `e3`.
添加一条待办。**用你自己快照里的引用**，不一定是 `e3`。

```bash
agent-browser fill @e3 "Prepare demo script"
```

```bash
agent-browser press Enter
```

**3.** Snapshot again. Each to-do now has a checkbox with its own ref.
再次快照。每条待办都有了带独立引用的复选框。

```bash
agent-browser snapshot -i
```

Expected (refs may differ) / 预期（引用可能不同）:

```
- checkbox "Toggle Todo" [checked=false, ref=e7] hint="in:Prepare demo script"
```

**Say / 讲解:**

> Notice that the snapshot changed after our action. Refs only describe the page at the moment of the snapshot, so the AI always looks again before its next move, just like a person would.
>
> 注意，操作之后快照变了。引用只代表快照那一刻的页面，所以 AI 每次行动前都会重新看一遍，就像人一样。

---

## Part 3: Readable commands and checks (2 min) / 可读的命令与状态检查（2 分钟）

Refs aren't the only way. You can target elements by what people see on screen.
引用不是唯一的方式，也可以按屏幕上看得到的内容来定位元素。

**1.** Add two more to-dos by placeholder text, with no refs needed.
按占位文字再添加两条待办，不需要引用。

```bash
agent-browser find placeholder "What needs to be done?" fill "Rehearse demo"
```

```bash
agent-browser press Enter
```

```bash
agent-browser find placeholder "What needs to be done?" fill "Present at meeting"
```

```bash
agent-browser press Enter
```

**2.** Check off the first to-do (use the checkbox ref from your latest snapshot), then verify.
勾选第一条待办（用最新快照里复选框的引用），然后验证。

```bash
agent-browser snapshot -i
```

```bash
agent-browser check @e7
```

```bash
agent-browser is checked @e7
```

Expected / 预期: `true`

```bash
agent-browser get text ".todo-count"
```

Expected / 预期: `2 items left`

**3.** Click a link by its role and name.
按角色和名称点击链接。

```bash
agent-browser find role link click --name "Completed"
```

**Say / 讲解:**

> The page now shows only completed items. Commands like `find role link --name "Completed"` read almost like English, which makes them easy for both people and AI. And if a name doesn't match, the error lists the names it actually saw, so it's easy to fix.
>
> 页面现在只显示已完成的待办。`find role link --name "Completed"` 这样的命令读起来几乎就是英语，人和 AI 都容易理解。如果名称不匹配，报错会列出它实际看到的名称，很容易修正。

---

## Part 4: Capture the results (2 min) / 记录结果（2 分钟）

**1.** Screenshot and PDF.
截图和导出 PDF。

```bash
agent-browser screenshot todo.png
```

```bash
agent-browser pdf page.pdf
```

**2.** Record a short video. `record start` opens the page fresh, so take a new snapshot before using refs, or use `find`.
录一段短视频。`record start` 会重新打开页面，所以使用引用前要重新快照，或者直接用 `find`。

```bash
agent-browser record start demo.webm https://demo.playwright.dev/todomvc
```

```bash
agent-browser find placeholder "What needs to be done?" fill "Recorded step"
```

```bash
agent-browser press Enter
```

```bash
agent-browser record stop
```

Open `todo.png`, `page.pdf`, and `demo.webm` to show the audience.
打开 `todo.png`、`page.pdf` 和 `demo.webm` 给大家看。

**Say / 讲解:**

> These outputs matter in practice: when an AI tests a web page for you, it can hand back a screenshot or a video as proof of what it did.
>
> 这些产出在实际中很有用：AI 帮你测试网页时，可以交回截图或视频，证明它做了什么。

---

## Part 5 (optional): Let the AI drive (2 min) / 让 AI 来驱动（可选，2 分钟）

> ⚠️ This part was not tested in advance. Rehearse it before the meeting. / 这一部分没有提前测试，会前务必彩排。

**1.** Show the built-in guide that teaches agents how to use the tool. It always matches the installed version.
展示教 agent 使用该工具的内置指南，它始终与当前安装的版本一致。

```bash
agent-browser skills get core
```

**2.** Install the skill into your agent (one time).
把 skill 安装到你的 agent 中（只需一次）。

```bash
npx skills add vercel-labs/agent-browser
```

**3.** Give Claude Code this task / 给 Claude Code 下达这个任务:

> Use agent-browser to open https://demo.playwright.dev/todomvc in headed mode. Add three to-dos: "Buy milk", "Book flights", "Write report". Mark "Book flights" as done, then save a screenshot as ai-result.png.

**Say / 讲解:**

> Watch the commands it runs: they're the same ones I just typed by hand. The AI does the thinking, and agent-browser does the work in the browser.
>
> 看看它执行的命令：和我刚才手动输入的一模一样。AI 负责思考，agent-browser 负责在浏览器里动手。

---

## Part 6: Feature overview and Q&A (2 min) / 功能总览与问答（2 分钟）

**Feature overview / 功能总览** (from the README / 来自 README):

| Group / 分组 | Commands / 命令 | Use / 用途 |
|---|---|---|
| Navigate and read / 导航与读取 | `open`, `snapshot`, `get`, `read` | Open and read pages / 打开、读取网页 |
| Interact / 交互 | `click`, `fill`, `type`, `select`, `upload`, `drag` | Act like a user / 像用户一样操作 |
| Capture / 记录 | `screenshot`, `pdf`, `record`, `diff` | Save and compare results / 保存与对比结果 |
| Network and state / 网络与状态 | `cookies`, `storage`, `network`, `eval` | Inspect page data / 查看页面数据 |
| Debug and performance / 调试与性能 | `console`, `errors`, `trace`, `vitals` | Debug and measure / 调试与测量 |
| Sessions and auth / 会话与认证 | `session`, `state`, `auth`, `profiles` | Reuse logins / 复用登录状态 |
| Agent integration / Agent 集成 | `skills`, `mcp`, `batch`, `plugin` | Connect to AI agents / 对接 AI agent |

Other points / 其他要点:

- **Browsers / 浏览器:** Chrome for Testing by default; can attach to your own Chrome, switch engines (Lightpanda, Obscura), drive Mobile Safari in the iOS Simulator, or use cloud browsers. / 默认 Chrome for Testing；也可连接你自己的 Chrome、切换引擎（Lightpanda、Obscura）、驱动 iOS 模拟器里的 Mobile Safari，或使用云端浏览器。
- **Platforms / 平台:** macOS, Linux, Windows.
- **MCP:** `agent-browser mcp` runs it as an MCP server. / 可作为 MCP server 运行。

**Prepared answers / 常见问题预案:**

**Q: How is this different from Playwright or Selenium? / 和 Playwright、Selenium 有什么区别？**
> Those are libraries for writing test scripts in code. agent-browser is a command-line tool designed for an AI's step-by-step loop: compact snapshots, short refs, and a browser that stays open between commands.
> 它们是用代码写测试脚本的库。agent-browser 是为 AI 的逐步循环设计的命令行工具：精简快照、简短引用，命令之间浏览器保持打开。

**Q: How is it different from "computer use"? / 和 computer use 有什么区别？**
> Computer use usually works from screenshots and controls the real mouse across the whole desktop. agent-browser only works in the browser, reads the page's structure instead of pixels, and has no AI inside.
> computer use 通常基于截图，在整个桌面上控制真实鼠标。agent-browser 只作用于浏览器，读取的是页面结构而不是像素，内部也没有 AI。

**Q: Does it need an API key? Does it cost money? / 需要 API key 吗？收费吗？**
> No. The tool is free and open source, and every command works without AI. You only need an AI subscription if you want an agent to drive it.
> 不需要。工具免费开源，所有命令都不依赖 AI。只有想让 agent 来驱动时，才需要 AI 订阅。

**Q: Can it use sites where I'm logged in? / 能用于需要登录的网站吗？**
> Yes. It can reuse a Chrome profile's login state with `--profile`, and it has session and auth commands. The security docs cover an auth vault, domain allowlists, and action policies.
> 可以。用 `--profile` 可以复用 Chrome 配置文件的登录状态，还有会话和认证相关命令。安全文档介绍了凭据库、域名白名单和操作策略。

---

## Appendix A: Troubleshooting / 故障排查

| Problem / 问题 | Fix / 解决办法 |
|---|---|
| `Unknown ref` | Refs changed. Run `snapshot -i` again and use the new refs, or use `find`. / 引用已变化，重新 `snapshot -i` 用新引用，或改用 `find`。 |
| No browser window appears / 看不到浏览器窗口 | Add `--headed` to the first command. If a browser is already running, run `agent-browser close --all` first. / 第一条命令加 `--headed`；如果浏览器已在运行，先执行 `agent-browser close --all`。 |
| `find` says no element matches / `find` 提示没有匹配 | The name must match exactly. Copy a name from the list in the error message. / 名称必须完全一致，从报错列出的名称里复制。 |
| A click fails on a real website / 在真实网站上点击失败 | A banner or pop-up may be covering it. Close the banner, snapshot again, and retry. / 可能被横幅或弹窗挡住了，先关掉它，重新快照后再试。 |
| Windows PowerShell errors on `@e3` / PowerShell 中 `@e3` 报错 | Quote the ref: `'@e3'`. / 给引用加引号：`'@e3'`。 |

## Appendix B: How big is it? / 项目有多大？

| Part / 部分 | Size / 大小 | Notes / 说明 |
|---|---|---|
| CLI (npm package) / 命令行工具 | ~21 MB | Measured on Linux after install. / 在 Linux 上安装后实测。 |
| Chrome for Testing | ~400 MB (estimate / 估算) | Based on a comparable Chromium build (393 MB). You can skip it by attaching to your own Chrome. / 参照同类 Chromium 版本（393 MB）估算；连接你自己的 Chrome 可省去这部分。 |
| Source code / 源代码 | ~33 MB | Only needed if you build it yourself. Not needed for the demo. / 只有自己编译时才需要，演示用不到。 |
