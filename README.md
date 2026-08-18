# dsh-worktree-panel

DSH Web GUI 的 git worktree 分支管理面板：在官方工作区/会话侧边栏之上，为 git 项目增加
**项目 → 主工作树 / 分支 worktree → 会话** 的维度，同时保留官方列表的全部原有交互。

## 功能

- 项目组内展示**主工作树**（当前分支 + 干净/有改动状态）与各**分支 worktree**
- 在 worktree 内开新会话、删除 worktree、切换主工作树分支
- 底部「＋ 分支 → 创建 worktree」为未建 worktree 的分支一键创建（可选新建分支）
- worktree 落盘位置可配置：默认 **项目内 `.dsh/workspaces/`**，或改为全局目录
- 更改落盘位置时自动检测并**批量迁移**已有 worktree（跳过有未提交改动 / 有活跃会话的）
- 非 git 工作区保持官方原样，不显示任何 worktree UI

## 安装

本插件按 DSH 插件包组织（`dsh` 字段 + `cordis.patch.yml`），通过 DSH 的插件安装/链接流程接入
`web` profile。`cordis.patch.yml` 会禁用官方 `ui-workspace` 行并插入本插件。

## 构建（开发）

`lib/client.js` 是**生成物**：`lib/build.mjs` 会定位官方
`@deepseek-ai/dsh-client-ui-workspace` 的编译产物，patch 出带 worktree 维度的客户端半身。

```bash
npm run build   # 等价于 node lib/build.mjs，重新生成 lib/client.js
npm test        # 运行 client-smoke + render 测试
```

`lib/client.js` 应**提交进仓库**（它是实际发布的客户端产物）；修改客户端逻辑后需重新构建。

> 构建与测试依赖本机的 DSH CLI 环境：`lib/build.mjs` 从 `~/.dsh/profiles/web/` 或
> `~/.npm/_npx/*/` 定位官方 `@deepseek-ai/dsh-client-ui-workspace` 编译产物，
> 两个测试文件也从此处的依赖树解析 `react` / `react-dom`。请先安装 DSH CLI 及其 `web` profile。

## 测试

```bash
npm test
```

- `test/client-smoke.mjs` — 在 Node 里执行 patch 后的 bundle，校验 apply 与 slot 注册
- `test/render-test.mjs` — 用 `react-dom/server` 挂载 WorkspaceBrowser，校验嵌套 worktree 渲染

## 许可证

MIT。`lib/client.js` 衍生自 `@deepseek-ai/dsh-client-ui-workspace`（MIT，Copyright (c) 2026
DeepSeek），详见 `NOTICE` 与 `LICENSE`。
