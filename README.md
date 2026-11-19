# 文献阅读

个人 Codex 技能，版本 **1.0.0**。将文献检索、Zotero 导入、真实全文精读和 Obsidian 整理连接成可重复使用的工作流。

## 使用

在 Codex 中调用“文献阅读”，或输入 `$literature-reading`，然后说明主题和处理范围。例如：

> 使用文献阅读，帮我找近三年关于轴承故障诊断中泛化评估的 5 篇论文，导入 Zotero 的“泛化评估”分类，并精读到科研文献库。

> 使用文献阅读，同步并精读 Zotero 中的新论文，保留已有笔记，更新首页和目录。

> 使用文献阅读，只依据文献库比较这些论文的数据划分方式，给出原文页码，并保存到问答。

如果只要候选清单，说“先列清单，等我选择再导入”。

## 仓库内容

- `literature-reading/SKILL.md`：技能入口、任务范围、操作边界。
- `literature-reading/agents/openai.yaml`：中文显示名称和调用提示。
- `literature-reading/references/search-import.md`：检索、去重、Zotero 导入与全文获取。
- `literature-reading/references/local-library.md`：既有文献库、证据记录、问答和校验。

本仓库保存技能规则，**不包含论文 PDF、个人笔记、文献库运行数据或账号配置**。技能复用已有的本地 Zotero 与科研文献库；同步脚本不负责调用模型，精读和问答在发出指令后由 Codex 执行。

## 本地依赖与恢复

默认文献库为 `~/Documents/obsidian/科研文献库`。技术脚本、模板和状态保存在该库的 `_系统` 中，日常目录保持为“文献、知识卡片、问答”。迁移机器时，文献库与 Zotero 需单独迁移；本仓库本身不会重建这两者。

本机安装遵循 SkillHub 优先规则，从这份本地原创技能打包，经 SHA-256 核对，显式安装到 `~/.codex/skills`。从备份恢复时可要求 Codex：“从这个 GitHub 仓库安装 literature-reading 技能，先汇总来源、版本与影响。”也可手动将 `literature-reading` 文件夹放入 Codex 技能目录。

安装后在下一轮对话调用；若列表尚未更新，可重新打开 Codex。AI 精读的笔记仍待本人复核，正式引用请核对原文版本和页码。

## 来源与版本

由用户授权在本地创建，基于已经验证的 Zotero + Obsidian + Codex 个人工作流。未复制论文正文、账号凭据或第三方插件源码；当前版本为 1.0.0。
