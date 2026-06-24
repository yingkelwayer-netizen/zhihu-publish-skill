# 知乎法律账号运营发布全链路技能

> 从选题 → SEO写作 → 跨Agent审核 → 发布 → 数据记录，三Agent协作的知乎法律内容运营全链路技能。
> 基于 Qclaw 平台，适配 xbrowser 浏览器自动化 + opencli 知乎适配器。
>Qlcaw里要安装以下专家：知乎策略师、小红书爆款操盘手、合同审查专家。

## 🚀 一句话安装

把下面这句话发给你的 知乎策略师（Agent）：

```
帮我安装 zhihu-publish-skill 技能：克隆 https://github.com/yingkelwayer-netizen/zhihu-publish-skill 到本地，读取其中的 SKILL.md，按里面的「环境配置引导」章节逐项帮我完成环境自检和配置，缺什么引导我装什么，全部通过后告诉我可以开始用了。
```

小龙虾会自动：
1. 克隆本仓库
2. 读取 `SKILL.md`
3. 按「环境配置引导」逐项自检
4. 缺什么引导你装什么（专家、Agent、Node.js、xbrowser 等）
5. 全部通过后通知你可以开始使用

---

## 📦 这个技能做什么

- **选题Agent**（绑定「小红书爆款操盘手」专家）：全平台热搜 → 筛选法律选题
- **写作发布Agent**（绑定「知乎策略师」专家）：SEO写作 → 送审 → 修改 → 发布
- **审核Agent**（绑定「合同审查专家」专家）：事实核验 + 法律表述审查

三Agent通过 `sessions_send` 自动协作，审核闭环最多2轮，第3轮强制人工介入。

---

## ✅ 前置条件

| 类型 | 需要什么 | 怎么验证 |
|------|---------|---------|
| Qclaw 专家 | 安装3个专家：小红书爆款操盘手、知乎策略师、合同审查专家 | 专家列表可见 |
| Qclaw Agent | 创建3个Agent分别绑定专家 | 拿到3个 Agent ID |
| Node.js | LTS 版本 | `node --version` |
| xbrowser | 已安装，拿到 `xb.cjs` 路径 | 路径存在 |
| Chrome | 已登录知乎账号 | 打开 zhihu.com 是登录态 |
| opencli | 已配置知乎 token | `opencli zhihu whoami` 返回用户名 |

> 不满足？没关系，小龙虾会引导你逐项完成。直接发上面那句话即可。

### 🛠️ 手动安装方法（命令行）

如果想自己动手装，按以下步骤操作：

**1. 安装 Node.js（LTS 版本）**

Windows（用 winget）：
```powershell
winget install OpenJS.NodeJS.LTS
```

macOS（用 Homebrew）：
```bash
brew install node@lts
```

Linux（Ubuntu/Debian）：
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

验证：`node --version` 应返回类似 `v20.x.x`。

---

**2. 安装 xbrowser**

xbrowser 是本技能依赖的浏览器自动化工具。安装后找到 `xb.cjs` 文件路径，填入 `SKILL.md` 中的 `$XB` 变量。

> 具体安装方式请参考 xbrowser 官方文档。安装完成后用以下命令验证：
```powershell
node <你的xb.cjs路径> run --browser chrome eval "1+1"
```

---

**3. 配置 Chrome 登录知乎**

1. 打开 Chrome 浏览器
2. 访问 `https://www.zhihu.com`
3. 正常登录你的知乎账号
4. 保持登录态不要退出

---

**4. 配置 opencli 知乎 token**

```bash
# 安装 opencli（如尚未安装）
npm install -g opencli

# 配置知乎 token
opencli zhihu login

# 验证
opencli zhihu whoami
```

验证：`opencli zhihu whoami` 应返回你的知乎用户名。

---

**5. 安装 crawl4ai（可选，用于网页爬取）**

```bash
pip install crawl4ai
```

---

## 📁 项目结构

```
zhihu-content-publish/
├── SKILL.md                      # 技能主文件（小龙虾读这个）
├── README.md                     # 你正在看的这个
└── references/
    ├── answers_log.md            # 发布记录日志
    └── keyword_library.md        # 知乎法律长尾关键词库
```

---

## 🎯 使用方式

配置完成后，对小龙虾说：
- `发知乎回答` — 写一条知乎回答并发布
- `发知乎文章` — 写一篇知乎专栏文章并发布
- `看今日选题` — 让选题Agent推送今日法律热点选题

---

## 🔧 部署后需修改的占位符

小龙虾配置时会帮你替换以下占位符：

| 占位符 | 替换为 | 位置 |
|--------|-------|------|
| `<你的xbrowser路径>/xb.cjs` | 实际 xb.cjs 路径 | SKILL.md 多处 |
| `<选题Agent_ID>` | 选题Agent 的实际 ID | 通信协议章节 |
| `<写作发布Agent_ID>` | 写作发布Agent 的实际 ID | 通信协议章节 |
| `<审核Agent_ID>` | 审核Agent 的实际 ID | 通信协议章节 |

---

## 🤝 开源共建

这是一个 **开源项目**，欢迎大家一起共建！无论是补充关键词库、优化发布流程、改进 SEO 策略，还是修复 bug、完善文档，都非常欢迎。

### 参与方式

- **提 Issue**：发现问题或有新想法，[点这里提 Issue](https://github.com/yingkelwayer-netizen/zhihu-content-publish/issues)
- **提 PR**：直接 fork → 修改 → 提交 Pull Request
- **补充关键词**：编辑 `references/keyword_library.md`，添加新的法律长尾关键词
- **分享经验**：在 Issue 区分享你的知乎运营心得，帮助优化策略

### 作者

GitHub 仓库：[yingkelwayer-netizen/zhihu-content-publish](https://github.com/yingkelwayer-netizen/zhihu-content-publish)

如果这个技能对你有帮助，欢迎 ⭐ Star 支持！

---

## 📄 License

MIT
