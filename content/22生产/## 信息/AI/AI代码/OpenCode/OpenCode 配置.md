
- 自动接受权限，显示推�?- GUI图标，自动以管理员打开
- 网络搜索优化，mcp
- 插件：优先用已有成熟方案。不要重复工作�?    - [awesome-opencode/awesome-opencode: A curated list of awesome plugins, themes, agents, projects, and resources for https://opencode.ai](https://github.com/awesome-opencode/awesome-opencode)
    - [Ecosystem | OpenCode](https://opencode.ai/docs/ecosystem/)
- 记忆：先用插件。自制的留挡。全局记忆架构设计。全局部署，项目根目录记忆�?- 数据结构规范V1：说明文档都集中�?doc，STRUCTURE总领所有结构规�?，并将其指引注入系统提示词。数据结构包括可用功能指引。功能又单独留下技术文档。留挡结构规范也通过标准化设计执行。先迭代出第一版用着�?- 数据迁移性：有效可迁移的数据是config和data。原生结构已经比较简明。可以终端与中转站双向同步，写脚本自动和手动触发，中转站定期保留历史版本
- 基本功能学习：学习文档。MCP 、AGENT SKILLS比较重要，得多关注�?- 交互优化：更像chat的体验�?

-----

opencode 文件架构摸清了。三个根目录，职责分明：

## 文件架构总览（Windows 实测�?
| 根目�?| 角色 | 内容 | 迁移�?|
|--------|------|------|--------|
| `~/.config/opencode/` | **CONFIG** | `opencode.jsonc`、`settings.json`、`agents/`、`command/`、`plugins/`、`skills/`、`hooks/`、`gsd-core/`、`docs/`、`memory-template.md`、`node_modules/`、`package.json` | �?**全部可迁�?*（`node_modules` 可删，靠 `package.json` 重装�?|
| `~/.local/share/opencode/` | **DATA** | `opencode.db`（SQLite�?*81MB，所有对话成�?*）、`auth.json`（凭证，敏感）、`opencode.log`、`repos/`（引用仓库缓存）、`tool-output/` | ⚠️ 对话+凭证可迁移，需谨慎（含密钥�?|
| `~/.cache/opencode/` | **CACHE** | `bin/`、`models.json` | �?不迁移，可重�?|

## 关键架构事实（影响迁移设计）

1. **配置�?合并"不是"替换"**：remote �?global �?custom �?project �?`.opencode` �?inline �?managed，逐层覆盖。所以你的个性化集中�?global + project 两层�?
2. **密钥可外�?*：config 支持 `{env:VAR}` �?`{file:path}` 替换�?*API key 不必写进 config**——可放环境变量或独立文件。当前你�?`opencode.jsonc` 已无明文密钥（密钥在 `auth.json`），config 本身可安全迁�?提交�?
3. **对话成果�?SQLite**：`opencode.db` 是单个数据库文件�?1MB），存所有项目的会话/消息�?*不是**按项目散落的文件。迁移这个文�?= 迁移全部对话历史�?
4. **`OPENCODE_CONFIG_DIR` 环境变量**：可指向任意目录当配置源�?*这是迁移的关键杠�?*——新机器把同步来的配置目录设�?`OPENCODE_CONFIG_DIR`，即生效�?
5. **项目级数据跟着项目�?*：每个项目的 `MEMORY.md` + `.memory/` + `AGENTS.md` + `.opencode/` 都在项目根，�?git 提交，天然随项目迁移�?
6. **auth.json 敏感**：含 provider 凭证。迁移它=免重新登录；不迁�?新机�?`/connect` 重认证�?
## 迁移性分层（初步�?
| �?| 内容 | 方式 |
|----|------|------|
| **全局个性化** | `~/.config/opencode/`（去�?`node_modules`、log�?| 打包同步，新�?`OPENCODE_CONFIG_DIR` 指向 + `npm install` 重装依赖 |
| **对话成果** | `opencode.db` | 直接复制（注意关 opencode 再复制，避免 WAL 不一致） |
| **凭证** | `auth.json` | 视安全需求：复制 �?新机�?`/connect` |
| **项目记忆** | 各项�?`MEMORY.md` + `.memory/` | 随项�?git �?|
| **缓存** | `~/.cache/opencode/` | 不迁，新机自�?|

架构清楚了。接下来要继续设�?*集成配置技术文�?*（把迁移流程、目录结构、各文件用途、新机部署步骤写成一份可�?agent/人读的文档）吗？还是先讨论某个具体点（比�?opencode.db 的迁移一致性、或密钥外置方案）？