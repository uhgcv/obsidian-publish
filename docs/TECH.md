# 技术文档 v1.0 — Obsidian → Quartz → GitHub Pages

## 架构总览

```
D:\01 mod\Obsidian仓库\
├── FMJKj001biJi/          ← 私人原稿 vault。不推 Git，绝不在此目录做任何自动操作
├── publish/               ← 精选公开笔记。用户手工从 vault 复制到此处
└── quartz/                ← Quartz 引擎 (v5.0.0)
    ├── content/  ──→ publish/（目录联结，mklink /J）
    ├── quartz.config.yaml  ← 主配置
    ├── quartz/styles/custom.scss ← 中文字体
    ├── .github/workflows/deploy.yml ← GitHub Actions 部署
    ├── docs/TECH.md       ← 本文档
    └── public/            ← 构建输出（gitignore，不提交）

    本地预览：npx quartz build --serve → localhost:8080
    线上部署：git push → GitHub Actions → GitHub Pages
    URL：https://uhgcv.github.io/obsidian-publish/
    GitHub repo：uhgcv/obsidian-publish（公开仓库）
```

## 关键配置决策

| 项目 | 选择 | 原因 |
|:---|:---|:---|
| 部署平台 | GitHub Pages | 免费、自动部署 |
| 仓库可见性 | **公开** | 免费版 Pages 只支持公开仓库 |
| 字体 | system（PingFang SC / Microsoft YaHei） | Google Fonts 在国内被墙 |
| 字体插件 | **禁用** `@quartz-community/quartz-fonts` | 避免自动插入 Google Fonts 链接 |
| 中文字体 | 在 `quartz/styles/custom.scss` 手动定义 `--bodyFont` / `--headerFont` | 禁用字体插件后的替代方案 |
| explicit-publish | **禁用** | publish 目录本身就是精选集，全部发布 |
| RSS | **禁用** (`enableRSS: false`) | 避免 index.xml 覆盖 index.html 作为首页 |
| SPA 路由 | 启用 | 左侧目录树导航正常工作 |
| baseUrl | `uhgcv.github.io/obsidian-publish` | GitHub Pages 子路径 |

## 内容联结

```cmd
mklink /J "D:\01 mod\Obsidian仓库\quartz\content" "D:\01 mod\Obsidian仓库\publish"
```

- `/J` = 目录联结，不需要管理员权限
- 联结是 Windows 文件系统层面的：在 quartz/content 看到的文件 = publish 的实际文件
- Git 能追踪联结内的文件
- 删除联结用 `rmdir quartz\content`（不是 del）

## 日常发布流程

```
1. 在 Obsidian (FMJKj001biJi vault) 写/改笔记
2. 手工复制到 D:\01 mod\Obsidian仓库\publish\
3. cd D:\01 mod\Obsidian仓库\quartz
4. npx quartz build           ← 本地验证（可选）
5. git add -A; git commit -m "更新"; git push
6. GitHub Actions 自动构建部署（约 2 分钟）
7. 访问 https://uhgcv.github.io/obsidian-publish/
```

## quartz.config.yaml 当前状态

要点速查（完整文件在项目根目录）：
- `pageTitle: "笔记分享"`
- `locale: zh-CN`
- `fontOrigin: local`
- `ignorePatterns: [.obsidian, .trash, private, templates]`
- 字体插件→false，explicit-publish→false，RSS→false
- URL 结构：左侧 explorer + 右侧 graph + 暗色/亮色/阅读模式 + 搜索

## ⚠️ 注意事项（最高优先级）

### 1. 永远不碰 FMJKj001biJi vault
这个目录是用户的原始笔记库。任何批量操作、脚本、格式修复都只能作用于 `publish/` 目录，绝不允许绕过它直接操作 vault 文件。

### 2. Windows PowerShell 编码陷阱
**这是本轮造成最大损失的根因**。

`Get-Content` / `Set-Content` / `Out-File` 在 PowerShell 5.1 不加 `-Encoding UTF8` 时，默认使用系统编码（中文 Windows = CP936/GBK）。读写含中文的 UTF-8 文件会**永久损坏**字节，不可逆。

禁止做法：
```powershell
Get-Content file.md -Raw | ... | Set-Content file.md
```
安全做法（任选其一）：
```powershell
# 始终指定编码
Get-Content file.md -Raw -Encoding UTF8 | ... | Set-Content file.md -Encoding UTF8
```
或者直接用 .NET：
```powershell
[System.IO.File]::ReadAllText("file.md", [System.Text.Encoding]::UTF8)
[System.IO.File]::WriteAllText("file.md", $content, [System.Text.Encoding]::UTF8)
```
或者用 Node.js/Python 操作，天然 UTF-8。

**识别损坏**：用 Read 工具打开文件，出现 `�` 符号 = 已损坏。不可自动恢复。

### 3. 中文必备配置
- `fontOrigin` 只能是 `"googleFonts"` 或 `"local"`。`"system"` 无效。
- 国内必须用 `"local"` 并禁用字体插件，在 `custom.scss` 定义系统中文字体栈。

### 4. 首页必须要有 index.md
没有 `index.md` → 不生成 `index.html` → 网站显示 RSS 或空白。

### 5. RSS 与首页冲突
`enableRSS: true` 生成的 `index.xml` 可能在根路径优于 `index.html` 被服务，导致首页显示 XML。当前已关闭。

### 6. GitHub Pages 仅支持公开仓库
私人仓库 + 免费账户 = 无法启用 Pages。已用公开仓库 + publish 精选笔记解决隐私问题。

## 试错记录（本轮关键教训）

| # | 问题 | 根因 | 解决 |
|:---|:---|:---|:---|
| 1 | 中文显示 `�?` | Google Fonts CDN 国内被墙 | 禁用字体插件 + custom.scss 系统字体 |
| 2 | 首次部署显示 RSS | 缺少 index.md，无首页 HTML | 创建 index.md |
| 3 | 1186 个 vault 文件编码损坏 | PS 5.1 Set-Content 未指定 UTF-8 | 用户从 OBS 备份恢复 |
| 4 | `fontOrigin: system` 无效 | 该值不存在，Quartz 只认 googleFonts/local | 改为 local |
| 5 | 根路径显示 RSS 而非首页 | `index.xml` 被 GH Pages 优先服务 | 关闭 RSS |
| 6 | 操作 vault 原稿导致批量损坏 | 违反谨慎性原则 | 记录为最高优先级规则 |
