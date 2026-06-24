---
name: zhihu-content-publish
description: >
  知乎法律账号运营发布全链路技能。涵盖热点选题→SEO写作→跨Agent审核→
  回答/文章发布→数据记录，含关键词库和SEO策略。
  触发词：发知乎、知乎发布、知乎回答、知乎文章、publish zhihu。
---

# 知乎法律账号运营与发布全链路技能 (zhihu-content-publish)

> 从选题 → SEO写作 → 跨Agent审核 → 发布 → 数据记录，全链路操作指南。
> 任何 agent 读完此文件即可独立执行知乎内容运营。

---

## 一、环境配置引导（⭐ 小龙虾必执行）

> ⚠️ **小龙虾（Qclaw Agent）强制指令**：
> 在执行任何知乎选题/写作/发布任务之前，你**必须**先逐项执行下方「环境自检清单」。
> - 发现缺失项 → **主动引导用户完成配置**，给出具体步骤和命令
> - 全部自检项通过 → 才可进入任务流程
> - 未通过时 → **直接拒绝执行发布操作**，并向用户说明缺什么、怎么补
> - 不要跳过自检直接开始写作或发布，否则会导致流水线断裂

本技能依赖 **Qclaw 三Agent协作** + **本地浏览器自动化环境**，首次使用需配置两类资源。

### A. Qclaw 平台层：三专家 + 三Agent

| Agent 角色 | 必装专家名称 | 在流水线中的职责 |
|-----------|-------------|----------------|
| 选题Agent | **小红书爆款操盘手** | 全平台热搜抓取 → 筛选法律相关选题（懂爆款逻辑、钩子设计、人群心理） |
| 写作发布Agent | **知乎策略师** | 选题决策 → SEO写作 → 送审 → 按审核意见修改 → 发布（懂知乎SEO、平台规则、编辑器操作） |
| 审核Agent | **合同审查专家** | 事实核验 + 法律表述审查（懂法律术语准确性、条款逻辑严谨性） |

**配置步骤**（小龙虾引导用户完成）：
1. 打开 Qclaw，进入 **专家列表 / 专家市场**
2. 搜索并依次安装：`小红书爆款操盘手`、`知乎策略师`、`合同审查专家`
3. 创建三个 Agent，分别绑定对应专家
4. 记录每个 Agent 的 ID，填入本文档「Agent间通信协议」中的 `<Agent_ID>` 占位符
5. 验证：三个 Agent 互相 `sessions_send` 测试消息能正常收发

> ⚠️ **未安装专家会导致流水线断裂**：选题Agent无法抓取热点、写作发布Agent不懂知乎SEO规则、审核Agent无法判断法律表述准确性。

### B. 本地环境层：浏览器自动化工具链

| 工具 | 用途 | 安装/验证方式 |
|------|------|--------------|
| Node.js | xbrowser 运行依赖 | `node --version` 返回版本号（建议 LTS） |
| xbrowser | 浏览器自动化 | 安装后找到 `xb.cjs` 路径，填入 `$XB` 变量 |
| Chrome | 知乎发布浏览器 | 已安装且登录知乎账号 |
| opencli | 知乎适配器 | `opencli zhihu whoami` 返回用户名 |
| crawl4ai（可选） | 网页爬取 | `pip install crawl4ai` |

### C. 环境自检清单（小龙虾逐项执行）

> 小龙虾：开始任务前，按下表逐项检查。每项给出"通过/未通过"判断，未通过则引导用户修复。

| # | 检查项 | 验证命令/方式 | 通过标准 | 缺失时的引导动作 |
|---|--------|--------------|---------|----------------|
| 1 | 三专家已安装 | 询问用户 / 检查专家列表 | 三个专家均已安装 | 引导用户去专家市场搜索安装 |
| 2 | 三Agent已创建 | 询问用户 | 三个 Agent ID 已知 | 引导用户创建并绑定专家，回传 Agent ID |
| 3 | Agent ID 已填入 | 检查本文档通信协议 | `<Agent_ID>` 已替换为实际值 | 向用户索取 ID 并填入 |
| 4 | Node.js | `node --version` | 返回版本号 | 引导用户安装 Node.js LTS |
| 5 | xbrowser | 询问 `xb.cjs` 路径 | 路径存在 | 引导用户安装 xbrowser 并提供路径 |
| 6 | Chrome 登录知乎 | 用 xb 打开 zhihu.com 检查 | 已登录态 | 引导用户在 Chrome 登录知乎 |
| 7 | opencli token | `opencli zhihu whoami` | 返回用户名 | 引导用户配置知乎 token |

### D. 首次使用对话引导脚本（小龙虾参考话术）

检测到首次使用时，小龙虾应主动发起如下引导（可根据实际情况调整措辞）：

```
检测到你首次使用知乎发布技能，我先带你完成环境配置（约5分钟）：

【Qclaw 平台层】
① 请确认你在 Qclaw 专家列表安装了三个专家：
   - 小红书爆款操盘手（给选题Agent用）
   - 知乎策略师（给写作发布Agent用）
   - 合同审查专家（给审核Agent用）
   安装好了告诉我，同时请创建三个 Agent 并分别绑定。

② 请把三个 Agent 的 ID 发给我，我帮你填入通信配置。

【本地环境层】
③ 检查 Node.js……（执行 node --version）
④ 检查 xbrowser……（请告诉我 xb.cjs 的路径）
⑤ 检查知乎登录态……（用浏览器打开 zhihu.com）
⑥ 检查 opencli……（执行 opencli zhihu whoami）

全部通过！现在可以开始使用。你想做什么？
   - 发知乎回答
   - 发知乎文章
   - 看今日选题
```

### E. 环境变量配置

自检通过后，将以下变量改为实际值（小龙虾协助用户填写）：
```powershell
$NODE = "node"
# ⚠️ 改为你的 xbrowser 路径
$XB = '<你的xbrowser路径>/xb.cjs'
```

---

## 二、工具链

| 工具 | 用途 | 命令 |
|------|------|------|
| xbrowser (xb) | 浏览器自动化 | `xb run --browser chrome <action>` |
| opencli zhihu | 知乎适配器 | `opencli zhihu <command>` |
| crawl4ai | 网页爬取 | `crwl <url> -o markdown` |
| sessions_send | 跨Agent通信 | 工具调用 |

---

## 三、账号信息

- 定位：法律领域知识型回答 + SEO引流获客
- **回答≤300字**（短回答高转化），文章800-2000字

> ⚠️ 部署时改为你的账号信息

---

## 四、多Agent协作流程

### 三Agent = 三专家流水线

| Agent 角色 | 绑定专家 | 职责 |
|-----------|---------|------|
| 选题Agent | 小红书爆款操盘手 | 全平台热搜 → 筛选TOP10法律选题 → 推送给写作发布Agent |
| 写作发布Agent | 知乎策略师 | 选1-2条选题 → SEO写作 → 送审 → 修改 → 发布 → 记录 |
| 审核Agent | 合同审查专家 | 事实核验 + 法律表述审查 → 回传审核结论 |

### 协作流程图

```
┌─────────────┐     选题交接      ┌──────────────────┐     送审        ┌───────────┐
│  选题Agent   │ ───────────────→ │  写作发布Agent    │ ──────────────→ │  审核Agent  │
│ (小红书操盘手) │  TOP10选题列表   │  (知乎策略师)     │   草稿+上下文   │ (合同审查专家)│
└─────────────┘                  └──────────────────┘                 └─────┬─────┘
                                        ↑                                   │
                                        │         审核回执                   │
                                        └───────────────────────────────────┘
                                              (通过/小改/打回)
                                        │
                                        ↓
                                 按审核意见修改
                                        │
                                        ↓
                                  用户确认 → 发布
```

### 每日流程

1. **选题抓取**：选题Agent 抓取全平台热搜，筛选法律相关选题
2. **选题交接**：选题Agent 通过 `sessions_send` 发送 TOP10 选题给写作发布Agent（消息格式见「Agent间通信协议」）
3. **选题决策**：写作发布Agent 从 TOP10 中选 1-2 条，标准：法律相关 + 知乎有对应问题 + 蓝海优先
4. **SEO写作**：写作发布Agent 按模板撰写草稿
5. **送审**：写作发布Agent 通过 `sessions_send` 将草稿发给审核Agent
6. **审核回执**：审核Agent 返回 ✅通过 / ⚠️小改 / ❌打回（格式见通信协议）
7. **修改闭环**：
   - ✅通过 → 直接进入发布流程
   - ⚠️小改 → 写作发布Agent自行修改后发布
   - ❌打回 → 修改后重新送审，最多2轮；第3轮仍打回则强制人工介入
8. **发布**：用户确认后，写作发布Agent执行发布
9. **记录**：写作发布Agent 更新 `references/answers_log.md`

### 自主选题（无选题Agent时）

1. **知乎热榜**：`zhihu.com/hot` 截图找法律问题
2. **关键词库**：从 `references/keyword_library.md` 选词搜索
3. **关注律师动态**：找他们答过但回答数少的题

选题标准：关注>100且回答<200，或关注<50且回答<20

> ⚠️ 如使用多Agent协作，需替换下方 Agent ID 和 Session Key 为目标环境的值

---

## 五、Agent间通信协议（⭐ 核心）

### 会话Key约定

> 部署时将 `<选题Agent_ID>` 等替换为实际 Agent ID。

| 通信方向 | Session Key | 用途 |
|---------|-------------|------|
| 选题Agent → 写作发布Agent | `zhihu_topic_<选题Agent_ID>` | 选题交接 |
| 写作发布Agent → 审核Agent | `zhihu_review_<写作发布Agent_ID>` | 送审草稿 |
| 审核Agent → 写作发布Agent | `zhihu_feedback_<审核Agent_ID>` | 审核回执 |

### 消息模板

#### 模板1：选题交接（选题Agent → 写作发布Agent）

```json
{
  "type": "topic_list",
  "date": "2026-06-24",
  "source": "小红书热搜+知乎热榜",
  "topics": [
    {
      "rank": 1,
      "title": "刑事案件请律师有用吗",
      "question_id": "266888677",
      "url": "https://www.zhihu.com/question/266888677",
      "keywords": ["刑事案件请律师", "律师有用吗"],
      "reason": "关注1.2万/回答87，蓝海题，长尾词搜索量大",
      "suggested_hook": "认知差钩 — 先说'90%的人请律师的时间都错了'"
    }
  ]
}
```

#### 模板2：送审草稿（写作发布Agent → 审核Agent）

```json
{
  "type": "review_request",
  "topic_title": "刑事案件请律师有用吗",
  "question_url": "https://www.zhihu.com/question/266888677",
  "content_type": "answer",
  "word_count": 287,
  "keywords_used": ["刑事案件请律师", "取保候审", "律师辩护"],
  "seo_strategy": "首段埋核心词，结尾关键词闭环，认知差钩",
  "draft": "<草稿全文>"
}
```

#### 模板3：审核回执（审核Agent → 写作发布Agent）

```json
{
  "type": "review_result",
  "verdict": "⚠️小改",
  "issues": [
    {
      "severity": "high",
      "category": "法律表述",
      "location": "第2段",
      "problem": "'刑事拘留最多37天'表述不严谨，应区分公安阶段与检察院审查起诉阶段",
      "suggestion": "改为'公安侦查阶段刑事拘留最长37天，含提请检察院审查批准逮捕的7天'"
    },
    {
      "severity": "low",
      "category": "SEO",
      "location": "结尾",
      "problem": "核心关键词在结尾未再现",
      "suggestion": "结尾补充'刑事案件请律师的关键节点'一句"
    }
  ]
}
```

**verdict 取值**：
- `✅通过` — 无需修改，可发布
- `⚠️小改` — 有轻微问题，写作发布Agent自行修改后发布
- `❌打回` — 有严重事实/法律错误，修改后必须重新送审

### 审核闭环规则

| 轮次 | 审核结果 | 处理方式 |
|------|---------|---------|
| 第1轮 | ✅通过 | 直接发布 |
| 第1轮 | ⚠️小改 | 修改后直接发布，无需重审 |
| 第1轮 | ❌打回 | 修改后重新送审（进入第2轮） |
| 第2轮 | ✅/⚠️ | 同上 |
| 第2轮 | ❌打回 | 修改后重新送审（进入第3轮） |
| 第3轮 | 任意 | **强制人工介入**，不再自动循环 |

### sessions_send 调用示例

```
# 选题Agent 发送选题给写作发布Agent
sessions_send(
  session_key = "zhihu_topic_<选题Agent_ID>",
  message = <模板1 JSON>
)

# 写作发布Agent 送审给审核Agent
sessions_send(
  session_key = "zhihu_review_<写作发布Agent_ID>",
  message = <模板2 JSON>
)

# 审核Agent 回传审核结论给写作发布Agent
sessions_send(
  session_key = "zhihu_feedback_<审核Agent_ID>",
  message = <模板3 JSON>
)
```

---

## 六、SEO写作策略

### 核心：知乎是搜索引擎

知乎与百度深度合作，**做知乎SEO = 同时做百度SEO**。

### 四大权重

1. **账号权重**：盐值、领域垂直度
2. **数据权重**：点赞率（点赞/阅读比）、收藏、评论
3. **内容权重**：关键词匹配度（SEO核心）、字数、原创度
4. **时间权重**：新内容加权，持续更新续命

### 关键词策略

| 类型 | 示例 | 竞争 | 转化 | 策略 |
|------|------|------|------|------|
| 大词 | "律师" | 极高 | 低 | 不主攻 |
| 长尾词 | "刑事案件请律师有用吗" | 低 | 高 | **主攻** |
| 场景词 | "被公司辞退了怎么维权" | 最低 | 最高 | **黄金词** |

### 布局规则（密度2%-4%）

1. 首段前50字必含核心关键词
2. 小标题嵌入长尾词变体
3. 段落首句含LSI词
4. 结尾关键词再现闭环
5. 关键词加粗强调

### 回答模板（≤300字）

```
[首段] 核心结论 + 主关键词（50字内）
[正文] 2-3要点，编号分段
  - 法律依据+分析
  - 实操建议
  - 避坑提醒
[结尾] 关键词再现 + 行动建议
```

### 文章模板（800-2000字）

```
[引言] 痛点+数据+核心关键词
一、[小标题含长尾词] 法律依据+案例
二、[小标题含长尾词] 深度分析+建议
三、[小标题含长尾词] 实操指南
四、[Q&A] 3-5个常见问题
[结语] 总结+关键词闭环
```

### 5种下钩子模式

1. 私信咨询 2. 认知差钩 3. 故事钩 4. 收藏钩 5. 争议钩

---

## 七、发布回答

### 回答编辑器：Draft.js，只能keyboard type逐行输入

❌ fill/execCommand/clipboard paste全部无效

```powershell
$NODE = "node"
$XB = '<你的xbrowser路径>/xb.cjs'

# 1. 打开问题页
& $NODE $XB run --browser chrome open 'https://www.zhihu.com/question/<QID>'
& $NODE $XB run --browser chrome wait --load networkidle

# 2. 点击"写回答"
& $NODE $XB run --browser chrome eval "(function(){var b=document.querySelectorAll('button,a');for(var i=0;i<b.length;i++){if(b[i].textContent.indexOf('写回答')>=0){b[i].click();return 'ok'}}return 'fail'})()"

# 3. 聚焦+清空编辑器
& $NODE $XB run --browser chrome eval "(function(){var ed=document.querySelectorAll('[contenteditable]')[0];ed.focus();var s=window.getSelection();s.selectAllChildren(ed);s.deleteFromDocument();return 'ok'})()"

# 4. ⭐ 逐行keyboard type输入（唯一可靠方法）
$content = Get-Content "<file.md>" -Raw -Encoding UTF8
$paragraphs = $content -split "`r?`n`r?`n"
for ($i=0; $i -lt $paragraphs.Count; $i++) {
    $para = $paragraphs[$i].Trim() -replace '^#{1,6}\s*','' -replace '\*\*(.+?)\*\*','$1'
    if ($para.Length -eq 0) { continue }
    foreach ($line in ($para -split "`r?`n")) {
        $line = $line.TrimEnd()
        if ($line.Length -eq 0) { continue }
        & $NODE $XB run --browser chrome keyboard type $line
        & $NODE $XB run --browser chrome press Enter
    }
    & $NODE $XB run --browser chrome press Enter
}

# 5. 验证字数
& $NODE $XB run --browser chrome eval "(function(){return document.querySelectorAll('[contenteditable]')[0].textContent.length})()"

# 6. 发布
& $NODE $XB run --browser chrome eval "(function(){var b=document.querySelectorAll('button');for(var i=0;i<b.length;i++){if(b[i].textContent.trim()==='发布回答'){b[i].click();return 'ok'}}return 'fail'})()"

# 7. 验证URL含/answer/<ID>
& $NODE $XB run --browser chrome eval "window.location.href"
```

### 回答格式操作

| 操作 | 方法 |
|------|------|
| 加粗 | 文字→Shift+Home→Ctrl+B |
| 引用 | 工具栏按钮 |
| 有序列表 | "1. "开头自动识别 |
| 法条 | 加粗+引用 |

---

## 八、发布文章

### 文章编辑器：支持Ctrl+V粘贴

```powershell
# 1. 打开写作页
& $NODE $XB run --browser chrome open 'https://zhuanlan.zhihu.com/write'
& $NODE $XB run --browser chrome wait --load networkidle

# 2. 填标题（React兼容）
& $NODE $XB run --browser chrome eval "var t=document.querySelector('textarea.Input');var s=Object.getOwnPropertyDescriptor(window.HTMLTextAreaElement.prototype,'value').set;s.call(t,'标题');t.dispatchEvent(new Event('input',{bubbles:true}));t.value"

# 3. 剪贴板粘贴正文
Set-Clipboard -Value $content
& $NODE $XB run --browser chrome eval "document.querySelector('div[contenteditable=true]').focus()"
Start-Sleep -Milliseconds 500
& $NODE $XB run --browser chrome press Control+v
Start-Sleep -Seconds 3

# 4. 验证+发布
& $NODE $XB run --browser chrome eval "document.querySelector('div[contenteditable=true]').innerText.length"
& $NODE $XB run --browser chrome eval "(function(){var b=document.querySelectorAll('button');for(var i=0;i<b.length;i++){if(b[i].textContent.trim()==='发布'){b[i].click();return 'ok'}}return 'fail'})()"
# URL应跳转 zhuanlan.zhihu.com/p/<ID>
```

### HTML格式粘贴（保留加粗/引用/列表）

```powershell
$html = "<p><strong>标题</strong></p><blockquote>法条</blockquote><ol><li>步骤</li></ol>"
$pasteJs = "(function(){var h='$html';var t=h.replace(/<[^>]+>/g,'');var dt=new DataTransfer();dt.setData('text/html',h);dt.setData('text/plain',t);var e=new ClipboardEvent('paste',{clipboardData:dt,bubbles:true});var ed=document.querySelector('.public-DraftEditor-content')||document.querySelector('[contenteditable=true]');ed.focus();ed.dispatchEvent(e);return 'ok'})()"
& $NODE $XB run --browser chrome eval $pasteJs
```

### 格式支持

| 格式 | 文章编辑器 | 回答编辑器 |
|------|-----------|-----------|
| 加粗 `<strong>` | ✅ | Ctrl+B |
| 引用 `<blockquote>` | ✅ | 工具栏 |
| 有序列表 `<ol>` | ✅ | "1. "自动 |
| 无序列表 `<ul>` | ✅ | 工具栏 |
| h1-h3 | ❌用`<strong>` | ❌ |
| Markdown | ❌ | ❌ |

---

## 九、发布规则（⭐ 铁律）

- ⛔ **环境自检未全部通过时，禁止执行任何发布操作**（见「环境配置引导」）
- 每次只发一条，用户说"发"才发
- 不批量发布，知乎有限流
- 发完立即汇报
- 每天2-3个高质量回答

---

## 十、常见问题

| 问题 | 解决 |
|------|------|
| type后字数为0 | 先eval聚焦编辑器 |
| Ctrl+V无内容 | eval聚焦→等500ms→再粘贴 |
| 发布按钮无反应 | Draft.js认为空，重新type |
| Markdown符号显示 | 输入前去掉##和** |
| opencli search报错 | 改用xb打开搜索页 |
| ref失效 | 重新snapshot |
| 审核Agent无响应 | 检查 Session Key 是否正确、专家是否已安装 |
| 选题Agent推送为空 | 检查小红书爆款操盘手专家是否已绑定 |
| 小龙虾没引导配置 | 确保读了「环境配置引导」章节并执行自检清单 |

---

## 十一、数据记录

发布后更新 `references/answers_log.md`，记录：问题标题、ID、URL、关键词、策略

---

## 附：部署清单（完整版见「环境配置引导」章节）

1. **Qclaw 专家安装**（⭐ 首次必做）：见「一、环境配置引导」A 节
2. **本地工具链**（Node.js / xbrowser / Chrome / opencli / crawl4ai）：见「一、环境配置引导」B 节
3. **Agent ID 填入**：见「一、环境配置引导」C 节自检清单第3项
4. **环境变量**：`$XB` 路径见「一、环境配置引导」E 节