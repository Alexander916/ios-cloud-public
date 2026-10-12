# Public cloud development starter

用于 Codex 开发、GitHub Actions 标准 Linux / Windows / macOS 运行器验证的公有仓库。

## 已配置

- `Xcode environment check` 检查 Xcode、Swift 和 iOS SDK，并类型检查一段 iOS SwiftUI 代码。
- 修改 main 分支的 `ci/` 或检查工作流时自动运行，对相同路径的 Pull Request 也运行。
- 可以在 Actions → Xcode environment check → Run workflow 手动运行。
- 工作流使用只读权限，不需要任何 Apple 凭据。
- `AGENTS.md` 提供 Codex 项目说明；`.gitignore` 排除常见签名文件和本地构建文件。
- `Linux and Windows environment check` 提供 Ubuntu 24.04 与 Windows Server 2022 两套任务，验证 Git、Node.js、npm、Python，Linux 额外检查 Docker CLI，Windows 额外检查 .NET SDK。
- 在 Actions 手动运行此工作流时，可选择 `all` / `linux` / `windows`；修改其工作流时也会自动检查两个系统。
- `Codex Xcode agent` 可手动启动 macOS Runner，在 Runner 内运行官方 Codex GitHub Action，并按提示检查和修复 Xcode 构建。

## 选择运行系统

| 工作流 | 系统 | 当前验证内容 |
| --- | --- | --- |
| Xcode environment check | macOS 15 | Xcode、iOS SDK、SwiftUI 类型检查 |
| Linux and Windows environment check：linux | Ubuntu 24.04 | Linux 和常用开发工具 |
| Linux and Windows environment check：windows | Windows Server 2022 | Windows 和常用开发工具 |

每次任务使用临时运行器，结束后环境会销毁。这是执行构建、测试和脚本的环境，不提供长期在线桌面。
添加实际项目后，应补充依赖安装、构建命令、测试命令和产物上传，并更新自动触发的路径范围。

## 在 macOS Runner 中运行 Codex

首次使用前，在仓库的 Settings → Secrets and variables → Actions 中添加 `OPENAI_API_KEY`。不要把密钥写入代码、工作流输入或日志。

打开 Actions → Codex Xcode agent → Run workflow，然后填写 Codex 任务。接入实际 App 后，同时填写 `.xcodeproj` 或 `.xcworkspace` 路径和 Scheme；工作流会先用模拟器 SDK 编译，再让 Codex 读取 `build.log`，最后重新编译验证。

`commit_changes` 默认关闭。先下载工作流产生的诊断 artifact 检查 Codex 的修改，确认后再开启该选项，让工作流把修改提交回本次运行的分支。此工作流只支持手动运行，不对 Pull Request 自动暴露 API 密钥。

也可以在本地 Codex 终端中用 GitHub CLI 控制它：

```powershell
gh workflow run codex-xcode-agent.yml `
  --repo Alexander916/ios-cloud-public `
  --ref main `
  -f prompt='读取 build.log，修复 Xcode 编译错误并保持改动最小' `
  -f project_path='YourApp/YourApp.xcodeproj' `
  -f scheme='YourApp' `
  -f commit_changes=false

gh run list --repo Alexander916/ios-cloud-public --workflow codex-xcode-agent.yml --limit 3
gh run watch RUN_ID --repo Alexander916/ios-cloud-public --exit-status
gh run download RUN_ID --repo Alexander916/ios-cloud-public
```

把 `project_path` 换成 `.xcworkspace` 时使用 `workspace_path`，并填写对应 Scheme。`commit_changes=true` 时，只有构建成功且工作流完成后才会把 Codex 的修改提交回运行所在分支。

## 接入 Codex 云端

1. 在 Codex 选择云端任务，创建或选择云端环境。
2. 连接 GitHub，并授权访问 `Alexander916/ios-cloud-public`。
3. 选择仓库，准备、检查并发布环境。
4. 让 Codex 修改代码并提交 Pull Request；检查会按配置路径触发。

本机 GitHub CLI 的登录不会自动完成 Codex 云端的 GitHub 授权。
Codex 编写代码后，Xcode 检查由 GitHub Actions 的 macOS 运行器执行。
这些工作流提供 GitHub 的多系统执行任务，不会在 Codex 云端环境选择器里自动增加 Windows 或 macOS 选项。

## 后续需要的配置

当前仓库没有完整 App 工程，不会生成可安装的 IPA。

- 开发 App：确定 SwiftUI / Flutter / React Native，添加工程、Bundle ID、构建和测试命令。
- 发布 App：配置 Apple Developer Program、App Store Connect App、签名证书和描述文件。
- 自动上传 TestFlight：配置 App Store Connect API 凭据及上传工作流。
- 真机测试：使用 iPhone 验证界面、权限和设备功能。

发布凭据只存放在 GitHub Secrets；签名和上传流程应与公开 PR 检查分开。
代码和 Actions 日志会公开，勿提交个人资料或打印秘密。

## 费用与服务

公开仓库使用 GitHub 标准 Linux / Windows / macOS 运行器的构建时间免费，大规格运行器另行收费。
此配置不订阅付费服务。Codex 使用额度与 Apple 开发者会员费分别计算。
这是 GitHub Actions 的 Xcode 环境，并非 Apple Xcode Cloud 订阅。
