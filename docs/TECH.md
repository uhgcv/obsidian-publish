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

### 5. 谨慎性原则（最高优先级）

**永远不要擅自操作超出用户明确指令范围的文件或目录。**

- 分仓库架构的本意是保险：publish 是公开发布区，FMJKj001biJi 是原稿重地。任何时候不该绕过 publish 直接碰原稿
- 只碰用户指定的文件/目录，绝不多动一个
- PowerShell 的 Get-Content / Set-Content 在中文环境下会永久损坏文件编码。涉及中文文件的任何操作，必须显式 `-Encoding UTF8` 或使用 .NET API

### 6. 中文编码损坏的原理与修复

**现象**：Obsidian 里中文正常，但 Quartz 构建后显示 `�?` 乱码。

**根因**：PowerShell 5.1 的 `Get-Content`/`Set-Content` 不指定 `-Encoding UTF8` 时，默认用系统编码（中文 Windows = CP936/GBK）。UTF-8 中文文件的某些字节序列在 CP936 中无效，被替换为 `?`（0x3F）。这个损坏是**永久的、不可逆的**——原始字节丢了就是丢了。

**损坏的具体机制**：以"酸"字（UTF-8: `E9 85 B8`）为例：
- CP936 解释器将 `E9 85` 识别为一个 2 字节的 CP936 字符
- 剩余的 `B8` 是 CP936 前导字节，期待一个后续字节但没有
- `B8` 及后续字节被替换为 `?`（0x3F）
- 原 3 字节 → 只剩 2 字节有效 + 1 个 `?`

**为什么重启后就好了**：
- 修复后的笔记是用 Obsidian（UTF-8 原生）重新编辑保存的，没有经过 PowerShell
- 干净的文件直接复制到 publish 目录，全程 UTF-8 无损
- 之前怎么修都不好，是因为我不断在 PowerShell 里重复"复制 + Set-Content"，**每次操作都在重新损坏**

**教训**：
- 涉及中文文件的操作，绝不通过 PowerShell 的 `Get-Content`/`Set-Content`/`Out-File`（不加 `-Encoding UTF8`）
- 批量操作用 `.NET API` 或 Node.js/Python，显式指定 UTF-8
- 识别损坏：文件中出现 `?`（单字节 0x3F）且上下文应是中文 → 已损坏
- 恢复：从原始源重新复制，绝不通过 PowerShell 中转
