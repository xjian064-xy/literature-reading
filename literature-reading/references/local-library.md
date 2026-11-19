# 本地文献库操作

## 单一事实来源

仓库根目录固定为 `~/Documents/obsidian/科研文献库`。先读取：

1. `AGENTS.md` 和 `首页.md`：维护规则、当前导航。
2. `_系统/脚本/runtime.json`：已配置的 Python。
3. `_系统/资料/manifest.json`：真实条目、主笔记、附件、版本哈希与状态。
4. 要精读时再读 `_系统/模板/论文精读模板.md`；需要命令细节时再读 `_系统/说明/维护说明.md`。

路径缺失时先定位现有库，不凭空新建第二个仓库。现有同步实现为 `_系统/脚本/research_library.py`，脚本将仓库根目录作为 ROOT。不要退回旧的编号目录、`raw` 或 `scripts` 布局。当前库的 AGENTS 与模板比本参考中描述的历史布局更具体；如已更新，使用当前实际文件。

## 命令

工作目录为仓库根目录；下面的 `python` 代表 runtime.json 的 Python，不是未经检查的系统解释器。初始已验证的运行时是：

`~/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3`

```bash
python _系统/脚本/research_library.py status
python _系统/脚本/research_library.py sync --all
python _系统/脚本/research_library.py sync --collection 分类key
python _系统/脚本/research_library.py sync --item 条目key
python _系统/脚本/research_library.py search "数据泄漏 轴承"
python _系统/脚本/research_library.py finalize 条目key
python _系统/脚本/research_library.py index
python _系统/脚本/research_library.py check
```

首次生成单篇笔记时支持 `sync --item 条目key --note-title 简短中文标题`。根据当前 `--help` 核实命令。指定分类时先用 `status` 查精确 key；仅该分类，不隐式包含子分类。用户只处理新导入的几篇时用其实际条目 key，避免顺手精读全库。

`_系统/更新文献库.command` 可执行资料同步和校验，**不自动调用模型**。`search` 是空格分隔、要求全部命中的关键词定位，不是语义检索或自动问答。中文检索使用少量核心词；命中后仍需读上下文和原文。

## 精读和证据

从 manifest 找实际附件、本地文件和逐页提取；不能把摘要当全文。原文在 Zotero，提取内容和快照在 `_系统/资料`。关键图、表、公式和抽取错误处调用可用 PDF 技能渲染核对；缺字或扫描页需说明 OCR 缺口。实际阅读的预印本页码不能写成期刊版页码。

模板固定十个部分，由 `finalize` 核验，不要为简化排版擅自改掉必需章节：

研究背景与问题；创新点与边界；数据与研究对象；方法与流程；实验设置与评价；核心发现；局限性；与当前课题的关系；可引用内容与证据；后续问题与检索标签。保留“我的补充”。

更新旧笔记前备份到 `_系统/资料/笔记历史`，保留时间与来源指纹。文件名短，`zotero_key`、`attachment_key` 与完整标题在属性里；依 key 复用主笔记，不把多语言附件当作多篇文献。

证据位于 `_系统/资料/证据/条目key.json`。参照当前已通过验收的样例；核心字段是：

```json
{
  "item_key": "实际条目key",
  "attachment_key": "实际附件key",
  "pdf_sha256": "当前原始PDF的实际SHA256",
  "claims": [
    {
      "id": "E01",
      "claim": "用中文准确转述的原文主张",
      "pdf_page": 1,
      "excerpt": "从该PDF物理页逐字核对的原文短摘录"
    }
  ]
}
```

以上只说明字段，不是可直接提交的真实证据。当前校验至少需要三条关键主张，每条摘录去空白后不少于 20 字符，并在**指定页**的提取文本里匹配；不能为了凑条件编造摘录。实际来源稀少或不适合该精读模板时说明限制，不伪造通过状态。

正文使用 `[E01]` 引用；证据表给出 `zotero://open-pdf/library/items/附件key?page=物理页码`，以及 manifest 指向的本地提取页。属性 `topics`、`methods`、`datasets` 使用列表；数据集只列论文实际使用的，迁移建议放正文。

完成笔记后运行 `finalize 条目key`，再 `index` 与 `check`。结构、摘录和来源哈希校验通过不等于学术内容完全正确、本人已读或实验已复现。不得手改 manifest 状态绕过验收。

## 状态、问答和收尾

| 内部状态 | 用户可见含义 |
|---|---|
| `pending` | 待精读 |
| `ai_reviewed` | AI 已精读，待本人复核 |
| `needs_review` | 原文或主要书目信息变化，待重新核查 |
| `blocked` | 缺少可读全文或处理条件 |

同步会保留已有正文；来源变化时保留旧分析并标记复核。用户新写的个人批注不能覆盖。来源快照和全文提取按内容哈希留存，不改写历史材料，也不静默删除 Zotero 中消失的论文笔记。

问答先用 `search` 找笔记，再核对关联证据。保存到 `问答` 时记录问题、回答、日期、来源和页码，标清推论与资料不足；概念积累放 `知识卡片`。对比表可用研究对象、数据集、划分单位、方法、指标、结果和限制，缺失项写未报告，不跨不一致协议强行排名。现有问答不是新增独立证据。

`index` 更新文献目录、主题/方法/数据集索引、处理日志与首页自动区；首页标记之外保留用户内容。完成后检查活动链接、状态和输出位置，报告新增/跳过/缺失，而不向用户堆放技术日志。默认不更改当前 Obsidian 外观，不恢复侧栏已收起的技术文件。
