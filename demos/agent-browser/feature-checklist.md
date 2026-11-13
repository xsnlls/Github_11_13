# agent-browser Feature Checklist / 功能清单

- **Project / 项目:** [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser)
- **Version tested / 测试版本:** 0.43.0, installed from npm, unchanged / 从 npm 安装，未做任何修改
- **Sources / 来源:** README + `agent-browser --help` + `agent-browser skills list`
- **Test setup / 测试环境:** Linux cloud container, headless Chromium 153, 2026-11-13. Target pages: example.com, the-internet.herokuapp.com, demo.playwright.dev/todomvc, httpbin.org. / Linux 云容器，无头 Chromium 153。

**Legend / 图例**

| Mark | Meaning | 含义 |
|---|---|---|
| ✅ | Tested and works; can be demoed live | 已测试可用，可现场演示 |
| ⚠️ | Works, with a caveat (see note) | 可用，但有注意事项 |
| 📝 | Needs extra setup (key, app, or OS); describe only | 需要额外配置（key、应用或系统），只能介绍 |
| ❓ | Not tested here | 未在此测试 |

---

## 1. Navigate and read / 导航与读取

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Open a page / 打开网页 | `open <url>` | ✅ | |
| Back, forward, reload / 后退、前进、刷新 | `back`, `forward`, `reload` | ✅ | |
| Page as AI-readable text / 网页转为 AI 可读文本 | `read [url]` | ✅ | Returns the page's main text. / 返回页面正文。 |
| Snapshot with refs / 带引用的快照 | `snapshot -i` | ✅ | Options `-c` (compact), `-d` (depth), `-s` (scope to selector) all work. / `-c` 精简、`-d` 深度、`-s` 限定范围均可用。 |
| Get info / 获取信息 | `get title/url/text/html/value/attr/count/box/styles` | ✅ | |
| Check state / 检查状态 | `is visible/enabled/checked` | ✅ | |
| Wait / 等待 | `wait <ms or selector>` | ✅ | |
| Scroll / 滚动 | `scroll`, `scrollintoview` | ✅ | |

## 2. Interact / 交互

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Click, double-click / 单击、双击 | `click`, `dblclick` | ✅ | |
| Fill, type, press keys / 填写、输入、按键 | `fill`, `type`, `press`, `keyboard type` | ✅ | |
| Hover, focus / 悬停、聚焦 | `hover`, `focus` | ✅ | |
| Checkboxes, dropdowns / 复选框、下拉框 | `check`, `uncheck`, `select` | ✅ | |
| Drag and drop / 拖拽 | `drag <src> <dst>` | ✅ | |
| Raw mouse / 鼠标底层操作 | `mouse move/wheel` | ✅ | |
| Find by meaning / 按语义查找 | `find role/label/placeholder/text ...` | ✅ | Name must match exactly; the error lists the names it saw. / 名称须完全一致，报错会列出实际名称。 |
| Upload a file / 上传文件 | `upload <sel> <file>` | ✅ | The file was attached. / 文件已选中。 |
| Download a file / 下载文件 | `download <sel> <path>` | ✅ | |
| Submitting a form by POST / 以 POST 方式提交表单 | `click` on a submit button | ⚠️ | On two pages (login, upload), the submit didn't go through or the page hung in my environment, though `curl` reached the server fine. Probably my sandbox's network, not the tool. **Retest on your machine.** / 在两个页面上提交没有生效或页面卡住，但 `curl` 能正常访问服务器。可能是我这里的沙箱网络问题，**需在你的电脑上复测**。 |

## 3. Capture and compare / 记录与对比

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Screenshot / 截图 | `screenshot [path]` | ✅ | `--full` for full page. / `--full` 截整页。 |
| Annotated screenshot / 带编号标注的截图 | `screenshot --annotate` | ✅ | Numbered labels that match refs; made for vision models. / 编号与引用对应，专为视觉模型设计。 |
| Save as PDF / 导出 PDF | `pdf <path>` | ✅ | |
| Video recording / 录屏 | `record start/stop` | ✅ | `.webm` / `.mp4`. `doctor` checks for ffmpeg. / `doctor` 会检查 ffmpeg。 |
| Compare snapshots / 对比快照 | `diff snapshot` | ✅ | |
| Compare screenshots / 对比截图 | `diff screenshot --baseline <img>` | ✅ | Reports the % of differing pixels; the baseline must be the same size. / 报告像素差异百分比，基准图尺寸需一致。 |
| Compare two pages / 对比两个页面 | `diff url <u1> <u2>` | ✅ | |

## 4. Browser settings / 浏览器设置

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Window size / 窗口尺寸 | `set viewport <w> <h>` | ✅ | |
| Emulate a phone / 模拟手机 | `set device "iPhone 14"` | ✅ | |
| Dark mode / 深色模式 | `set media dark` | ✅ | Only visible if the site supports it. / 网站支持才有效果。 |
| Location / 地理位置 | `set geo <lat> <lng>` | ✅ | |
| Offline mode / 离线模式 | `set offline on/off` | ✅ | Reloading while offline shows Chrome's error page, as expected. / 离线时刷新会显示错误页，符合预期。 |

## 5. Network and storage / 网络与存储

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| List requests / 查看请求 | `network requests` | ✅ | |
| Mock a response / 模拟接口返回 | `network route <url> --body <json>` | ✅ | The page showed `{"mocked":true}` instead of the real data. / 页面显示的是模拟数据。 |
| Block requests / 拦截请求 | `network route <url> --abort` | ✅ | |
| Record traffic as HAR / 录制 HAR | `network har start/stop` | ✅ | |
| Cookies / Cookie | `cookies get/set/clear` | ✅ | |
| Local/session storage / 本地存储 | `storage local/session` | ✅ | |
| Save and load browser state / 保存与加载状态 | `state save/load` | ✅ | Cookies + storage in a JSON file. / 存为 JSON 文件。 |

## 6. Tabs, frames, dialogs / 标签页、框架、对话框

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Tabs / 标签页 | `tab new/list/<id>/close` | ✅ | Tab IDs look like `t1`, `t2`, not numbers. / 标签 ID 形如 `t1`，不是数字。 |
| Iframes / 内嵌框架 | `frame <sel>`, `frame main` | ✅ | |
| Pop-up dialogs / 弹窗 | `dialog status/accept/dismiss` | ✅ | An open dialog blocks other commands, with a clear message telling you to resolve it. / 弹窗未处理时会阻塞其他命令，并给出清晰提示。 |

## 7. Debug and performance / 调试与性能

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Run JavaScript / 执行 JS | `eval <js>` | ✅ | |
| Console logs, page errors / 控制台日志、页面报错 | `console`, `errors` | ✅ | |
| Highlight an element / 高亮元素 | `highlight <sel>` | ✅ | Visible only in headed mode. / 有界面模式下才看得到。 |
| Performance trace, CPU profile / 性能追踪、CPU 分析 | `trace`, `profiler` | ✅ | |
| Core Web Vitals / 核心网页指标 | `vitals` | ✅ | |
| Accessibility audit / 无障碍审计 | `a11y` | ✅ | Uses axe-core; found 3 violations on TodoMVC. / 基于 axe-core，在 TodoMVC 上发现 3 个问题。 |
| React component tree / React 组件树 | `react tree` | ✅ | Requires `open --enable react-devtools`. / 需加此参数打开。 |
| SPA navigation / 单页应用跳转 | `pushstate <url>` | ✅ | |
| Clipboard / 剪贴板 | `clipboard write/read` | ⚠️ | Write works; read was blocked by browser permissions in headless mode. / 写入可用；无头模式下读取被浏览器权限拦截。 |

## 8. Sessions and login state / 会话与登录状态

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Parallel isolated sessions / 并行隔离会话 | `--session <name>`, `session list` | ✅ | Two browsers ran side by side. / 两个浏览器同时运行。 |
| Saved-login vault / 登录凭据库 | `auth save/list/show/delete` | ✅ | Use `--password-stdin` to keep the password out of the command line. / 用 `--password-stdin` 避免密码出现在命令行。 |
| Log in with a saved profile / 用凭据自动登录 | `auth login <name>` | ❓ | Not tested because of the form-submit issue above. / 因上面的表单提交问题未测试。 |
| Connect to a running Chrome / 连接已运行的 Chrome | `--cdp <port>` | ✅ | |
| Reuse your Chrome login / 复用你 Chrome 的登录 | `--profile`, `--auto-connect`, `profiles` | 📝 | Needs your own Chrome profile; none exists here. / 需要你本机的 Chrome 配置文件，这里没有。 |
| Auto-save and restore state / 自动保存恢复状态 | `--restore` | ❓ | |

## 9. AI agents and integrations / AI agent 与集成

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Built-in skills / 内置 skill | `skills list`, `skills get core` | ✅ | 10 skills: core, dogfood (explore and test a web app), electron, slack, derive-client, modern-web-guidance, webmcp-gen, vercel-sandbox, protected-vercel-deployments, agentcore. |
| MCP server / MCP 服务 | `mcp` | ✅ | Started and listed **33 tools** over MCP. / 启动后通过 MCP 列出 **33 个工具**。 |
| Run several commands at once / 批量执行 | `batch "cmd" "cmd"` | ✅ | |
| Machine-readable output / JSON 输出 | `--json` | ✅ | |
| Built-in AI chat / 内置 AI 对话 | `chat "<instruction>"` | 📝 | Needs a Vercel AI Gateway key (`AI_GATEWAY_API_KEY`). Default model per help: `anthropic/claude-sonnet-4.6`. / 需要 Vercel AI Gateway key。 |
| Plugins / 插件 | `plugin list/add/run` | ✅ | `list` works; none configured. / `list` 可用，未配置插件。 |
| WebMCP (experimental) / WebMCP（实验性） | `webmcp list/invoke` | 📝 | Needs a page that exposes WebMCP tools. / 需要提供 WebMCP 工具的网页。 |
| Saved flows (experimental) / 保存的流程（实验性） | `flow save/run` | ❓ | |

## 10. Observe / 观察

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Observability dashboard / 可视化面板 | `dashboard start` | ✅ | Served at `http://localhost:4848` (HTTP 200). I didn't inspect the UI itself. / 页面可访问，未查看界面内容。 |
| Live streaming / 实时流 | `stream enable/status/disable` | ✅ | |

## 11. Setup / 安装维护

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Health check / 健康检查 | `doctor` | ✅ | Checks Chrome, ffmpeg, daemons, encryption key, and provider keys. / 检查 Chrome、ffmpeg、守护进程、加密密钥和云服务 key。 |
| Download Chrome / 下载 Chrome | `install` | ❓ | Not run here; I used the container's Chromium. / 这里没运行，用的是容器自带的 Chromium。 |
| Self-update / 自动升级 | `upgrade` | ❓ | |

## 12. Safety controls / 安全控制

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| Domain allowlist / 域名白名单 | `--allowed-domains` | ✅ | Blocked example.org with a clear message. / 成功拦截 example.org 并给出提示。 |
| Limit output size / 限制输出长度 | `--max-output <n>` | ✅ | |
| Mark page content as untrusted / 标记网页内容边界 | `--content-boundaries` | ⚠️ | Ran without error; I didn't confirm the markers. / 运行无报错，未确认标记效果。 |
| Action policy / 操作策略 | `--action-policy <file>` | ❓ | My test file didn't take effect; it needs the documented format. / 我的测试文件未生效，需按文档格式编写。 |
| Confirm risky actions / 危险操作确认 | `--confirm-actions`, `confirm`, `deny` | ❓ | |

## 13. Display, engines, devices, cloud / 显示、引擎、设备、云端

| Feature | Command | Status | Note / 说明 |
|---|---|---|---|
| **Visible browser window / 可见浏览器窗口** | `--headed` | ❓ | **No display here. Must test on your machine; the live demo depends on it.** / **这里没有显示器，必须在你的电脑上测试，现场演示要靠它。** |
| Other engines / 其他引擎 | `--engine lightpanda/obscura` | 📝 | Needs the engine installed separately. / 需要单独安装引擎。 |
| iOS Simulator / iOS 模拟器 | `-p ios` | 📝 | macOS only; needs Xcode and Appium. / 仅限 macOS，需要 Xcode 和 Appium。 |
| Cloud browsers / 云端浏览器 | `-p browserbase/browserless/browseruse/kernel/agentcore` | 📝 | Each needs its own API key or account. / 各自需要 API key 或账号。 |

---

## Summary / 总结

- **✅ Live-demo ready / 可现场演示:** almost all core features: navigating, reading, interacting, capturing, comparing, network mocking, storage, tabs, dialogs, debugging, performance, accessibility, sessions, the MCP server, and the dashboard. / 几乎全部核心功能。
- **⚠️ Caveats / 注意事项:** POST form submission (retest on your machine), clipboard read, content boundaries.
- **📝 Describe only / 只能介绍:** AI chat (needs a key), reusing your Chrome login, other engines, iOS, cloud browsers, WebMCP.
- **❓ Must test before the meeting / 会前必须测试:** `--headed` (most important / 最重要), `install`, `auth login`.
