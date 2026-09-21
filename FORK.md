# Fork 版本管理约定（vel2 内部 fork）

本仓库 fork 自 `deepseek-ai/deepseek-harness`（upstream），origin 为内部 fork
`yangdm2015/deepseek-harness`。Max 开发机（n37-207-009）上运行的 dsh web 从本
fork 的 master 构建部署。

## 规则：每次改动必须加小版本

- fork 版本线独立于 upstream，形式为 `X.Y.Z-fork.N`（semver prerelease）。
- **每次合入任何自定义改动，都必须把 `fork.N` 加 1**（fork.1 → fork.2 → …），
  不允许两个不同的代码状态对外显示同一个版本号。
- 版本号是 dsh 全族统一版本（296 个 package.json 一致），用官方脚本 bump：
  ```sh
  pnpm tsx scripts/release/bump.ts --family dsh 0.1.6-fork.N
  ```
  脚本会改全部 manifest、锁文件并自动生成 release commit。
- 构建产物左下角会显示 `X.Y.Z-fork.N-<commit7>[-dirty]`，看到没有 `fork.` 前缀
  的版本号，说明跑的不是本 fork 的构建，应重新部署。
- upstream 同步到新版本（如 0.1.7）后，fork 版本线重置为 `<新版本>-fork.1`。

## 部署

Max 无法直连 GitHub：在 Mac M4 上打包源码（含 .git）传到 Max，
`DSH_CLIENT_COMMIT_HASH` 由构建环境自动注入。部署形态为 systemd user service
`deepseek-harness.service`。
