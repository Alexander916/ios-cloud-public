# iOS cloud public starter

用于 Windows + Codex 开发、GitHub Actions 标准 macOS 运行器验证的公有仓库。

## 已配置

- `Xcode environment check` 检查 Xcode、Swift 和 iOS SDK，并类型检查一段 iOS SwiftUI 代码。
- 修改 main 分支的 `ci/` 或检查工作流时自动运行，对相同路径的 Pull Request 也运行。
- 可以在 Actions → Xcode environment check → Run workflow 手动运行。
- 工作流使用只读权限，不需要任何 Apple 凭据。
- `AGENTS.md` 提供 Codex 项目说明；`.gitignore` 排除常见签名文件和本地构建文件。

## 接入 Codex 云端

1. 在 Codex 选择云端任务，创建或选择云端环境。
2. 连接 GitHub，并授权访问 `Alexander916/ios-cloud-public`。
3. 选择仓库，准备、检查并发布环境。
4. 让 Codex 修改代码并提交 Pull Request；检查会按配置路径触发。

本机 GitHub CLI 的登录不会自动完成 Codex 云端的 GitHub 授权。
Codex 编写代码后，Xcode 检查由 GitHub Actions 的 macOS 运行器执行。

## 后续需要的配置

当前仓库没有完整 App 工程，不会生成可安装的 IPA。

- 开发 App：确定 SwiftUI / Flutter / React Native，添加工程、Bundle ID、构建和测试命令。
- 发布 App：配置 Apple Developer Program、App Store Connect App、签名证书和描述文件。
- 自动上传 TestFlight：配置 App Store Connect API 凭据及上传工作流。
- 真机测试：使用 iPhone 验证界面、权限和设备功能。

发布凭据只存放在 GitHub Secrets；签名和上传流程应与公开 PR 检查分开。
代码和 Actions 日志会公开，勿提交个人资料或打印秘密。

## 费用与服务

公开仓库使用 GitHub 标准 macOS 运行器的构建时间免费，大规格运行器另行收费。
此配置不订阅付费服务。Codex 使用额度与 Apple 开发者会员费分别计算。
这是 GitHub Actions 的 Xcode 环境，并非 Apple Xcode Cloud 订阅。
