# 技术文档：Obsidian → Quartz → GitHub Pages 部署

## 架构

```
本地                          GitHub                    访问者
FMJKj001biJi (私人)           obsidian-publish (公开)
     ↓ 手工筛选                    ↓
  publish/ ──联结→ quartz/content ──→ GitHub Actions build ──→ Pages
     ↓                           ↓
  quartz build                public/ (HTML)
  localhost:8080 预览
```

## 关键配置

### quartz.config.yaml
- `fontOrigin: local` — 国内不用 googleFonts，会被墙
- `fontOrigin: system` — **无效值！** 只接受 `googleFonts` 或 `local`
- 字体改用系统字体：`"PingFang SC", "Microsoft YaHei", sans-serif`
- 禁用 `@quartz-community/quartz-fonts` 插件后需在 `quartz/styles/custom.scss` 自定义 `--bodyFont` / `--headerFont`

### explicit-publish 插件
- `enabled: true` 后只有 frontmatter 含 `publish: true` 的笔记出现在网站
- publish 目录下所有文件都应加 `publish: true`

### 内容联结
```cmd
mklink /J "quartz\content" "D:\01 mod\Obsidian仓库\publish"
```
- 联结不需要管理员权限（/J = 目录联结）
- 不影响 Obsidian 正常使用
- Git 能追踪联结内的文件

## 常用命令

```bash
# 本地预览
cd quartz
npx quartz build --serve    # 构建并启动预览（首次慢，增量快）

# 发布
git add -A
git commit -m "更新"
git push                    # 触发 GitHub Actions 自动部署
```

## ⚠️ 踩坑记录

### 1. Windows PowerShell 编码陷阱
**问题**：`Get-Content` / `Set-Content` 在 PowerShell 5.1 默认用系统编码（中文 Windows = GBK/CP936），读写 UTF-8 中文文件时**永久损坏字符**。

**错误示例**：
```powershell
$content = Get-Content file.md -Raw           # × 未指定 -Encoding UTF8
$content = $content -replace "---", "# title"
Set-Content file.md -Value $content -NoNewline # × 未指定 -Encoding UTF8
```

**正确做法**：
```powershell
$content = Get-Content file.md -Raw -Encoding UTF8    # ✓
$content = $content -replace "---", "# title"
[System.IO.File]::WriteAllText("file.md", $content)   # ✓ 始终 UTF-8
```

### 2. Google Fonts 国内被墙
`fontOrigin: googleFonts` 导致中文显示为 `�?`。必须用 `local` 或禁用字体插件 + 自定义 CSS。

### 3. index.html 缺失
如果 content 目录没有 `index.md`，Quartz 不生成首页。网站返回 RSS XML 而非 HTML。

### 4. GitHub Pages 私有仓库限制
免费版 Pages 只能从公开仓库部署。私有仓库需 Pro 或改用 Vercel。
