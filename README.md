# dsh-worktree-panel

[![npm version](https://img.shields.io/npm/v/dsh-worktree-panel)](https://www.npmjs.com/package/dsh-worktree-panel)
[![license](https://img.shields.io/npm/l/dsh-worktree-panel)](./LICENSE)

DSH Web GUI 的 git worktree 分支管理面板：在官方工作区/会话侧边栏之上，为 git 项目增加 **项目 → 主工作树 / 分支 worktree → 会话** 的维度，同时保留官方列表的全部原有交互。

A git worktree / branch management panel for the DSH Web GUI. On top of the official workspace/session sidebar, it adds a **project → main working tree / branch worktree → session** dimension while preserving every interaction of the official list.

---

## 安装 / Installation

### 从 npm 安装（推荐）/ From npm (recommended)

```sh
dsh plugin --profile web add dsh-worktree-panel
```

安装指定版本 / Install a specific version:

```sh
dsh plugin --profile web add dsh-worktree-panel@0.1.0
```

装完**重启 `dsh web`** 生效。/ Restart `dsh web` afterwards for it to take effect.

### 从源码链接（开发）/ Link from source (development)

```sh
dsh plugin --profile web add link:/绝对路径/到/dsh-worktree-panel
# 例如 / e.g.
dsh plugin --profile web add link:$HOME/Desktop/HeathHe/myself/dsh-worktree-panel
```

### 升级 / Upgrade

```sh
dsh plugin --profile web add dsh-worktree-panel@<新版本>
# 然后重启 dsh web / then restart dsh web
```

---

> 本插件按 DSH 插件包组织（`dsh` 字段 + `cordis.patch.yml`）。`cordis.patch.yml` 会禁用官方 `ui-workspace` 行并插入本插件，因此安装后工作区/会话侧边栏由本插件接管，官方原交互全部保留。
>
> This plugin is organized as a DSH plugin package (`dsh` field + `cordis.patch.yml`). `cordis.patch.yml` disables the official `ui-workspace` row and injects this plugin, so after installation the workspace/session sidebar is taken over by this plugin while every official interaction is preserved.

## 功能 / Features

- 项目组内展示**主工作树**（当前分支 + 干净/有改动状态）与各**分支 worktree**
- 在 worktree 内开新会话、删除 worktree、切换主工作树分支
- 底部「＋ 分支 → 创建 worktree」为未建 worktree 的分支一键创建（可选新建分支）
- worktree 落盘位置可配置：默认 **项目内 `.dsh/workspaces/`**，或改为全局目录
- 更改落盘位置时自动检测并**批量迁移**已有 worktree（跳过有未提交改动 / 有活跃会话的）
- 非 git 工作区保持官方原样，不显示任何 worktree UI

- Show the **main working tree** (current branch + clean/dirty status) and each **branch worktree** within a project group
- Open a new session inside a worktree, delete a worktree, and switch the main working tree's branch
- The bottom "＋ branch → create worktree" button one-click creates a worktree for any branch without one (optionally creating a new branch)
- Configurable worktree location: default to **`.dsh/workspaces/` inside the project**, or switch to a global directory
- Changing the location auto-detects and **bulk-migrates** existing worktrees (skipping those with uncommitted changes / active sessions)
- Non-git workspaces stay exactly as the official UI — no worktree UI is shown

## 构建（开发）/ Build (Development)

`lib/client.js` 是**生成物**：`lib/build.mjs` 会定位官方 `@deepseek-ai/dsh-client-ui-workspace` 的编译产物，patch 出带 worktree 维度的客户端半身。

`lib/client.js` is a **build artifact**: `lib/build.mjs` locates the official `@deepseek-ai/dsh-client-ui-workspace` bundle and patches it into a client half with the worktree dimension.

```bash
npm run build   # 等价于 node lib/build.mjs，重新生成 lib/client.js
                # Equivalent to node lib/build.mjs; regenerates lib/client.js
npm test        # 运行 client-smoke + render 测试
                # Runs the client-smoke + render tests
```

`lib/client.js` 应**提交进仓库**（它是实际发布的客户端产物）；修改客户端逻辑后需重新构建。

`lib/client.js` should be **committed to the repository** (it is the actual published client artifact); rebuild it after modifying client logic.

> 构建与测试依赖本机的 DSH CLI 环境：`lib/build.mjs` 从 `~/.dsh/profiles/web/` 或 `~/.npm/_npx/*/` 定位官方 `@deepseek-ai/dsh-client-ui-workspace` 编译产物，两个测试文件也从此处的依赖树解析 `react` / `react-dom`。请先安装 DSH CLI 及其 `web` profile。
>
> Building and testing depend on a local DSH CLI environment: `lib/build.mjs` resolves the official `@deepseek-ai/dsh-client-ui-workspace` bundle from `~/.dsh/profiles/web/` or `~/.npm/_npx/*/`, and both test files resolve `react` / `react-dom` from that dependency tree. Install the DSH CLI and its `web` profile first.

## 测试 / Tests

```bash
npm test
```

- `test/client-smoke.mjs` — 在 Node 里执行 patch 后的 bundle，校验 apply 与 slot 注册 / Executes the patched bundle in Node to verify `apply` and slot registration
- `test/render-test.mjs` — 用 `react-dom/server` 挂载 WorkspaceBrowser，校验嵌套 worktree 渲染 / Mounts WorkspaceBrowser with `react-dom/server` to verify nested worktree rendering

## 许可证 / License

MIT。`lib/client.js` 衍生自 `@deepseek-ai/dsh-client-ui-workspace`（MIT，Copyright (c) 2026 DeepSeek），详见 `NOTICE` 与 `LICENSE`。

MIT. `lib/client.js` is derived from `@deepseek-ai/dsh-client-ui-workspace` (MIT, Copyright (c) 2026 DeepSeek); see `NOTICE` and `LICENSE` for details.
