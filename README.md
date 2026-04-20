# 📱 微信公众号草稿发布技能

> 将文章一键发布到微信公众号草稿箱，支持封面图上传、自动获取 access_token、草稿管理。
> 适用于 OpenClaw AI 助手。

---

## ✨ 功能特性

- 🔑 **自动 access_token**：本地缓存，过期前自动刷新
- 🖼️ **上传封面图**：自动上传到微信永久素材库
- 📝 **创建图文草稿**：推送到公众号草稿箱
- 🔍 **查看草稿列表**：随时浏览和删除
- ⚙️ **零配置运行**：凭证写入 config.json 后一键使用

---

## 🚀 快速开始

### 1. 安装依赖

```bash
pip install requests
```

### 2. 配置凭证

复制 `config.json` 并填入你的公众号信息：

```bash
copy config.json config_local.json
# 编辑 config_local.json，填入 appid 和 appsecret
```

config.json 内容：

```json
{
  "appid": "wx开头的AppID",
  "appsecret": "32位AppSecret",
  "account_name": "你的公众号名称",
  "cache_dir": ".cache"
}
```

> ⚠️ 请勿将包含真实凭证的 `config.json` 提交到 GitHub！已配置 `.gitignore`。

### 3. 发布文章

```bash
# 纯文本模式（自动生成封面）
python scripts/create_draft.py ^
  --title "文章标题" ^
  --author "作者名" ^
  --digest "文章摘要（不超过54字）" ^
  --content "正文内容，支持<strong>HTML</strong>格式"

# 带封面图模式
python scripts/create_draft.py ^
  --title "文章标题" ^
  --author "作者名" ^
  --digest "摘要" ^
  --content "正文内容" ^
  --thumb-image "cover.jpg"
```

### 4. 查看草稿列表

```bash
python scripts/list_drafts.py
```

### 5. 生成封面图（可选）

```bash
# 需要先安装 Pillow：pip install Pillow
python scripts/make_cover.py --title "文章标题" --subtitle "副标题" --output cover.jpg
```

---

## 📁 文件结构

```
wechat-oa/
├── skill.json            # SkillHub 元数据（安装识别用）
├── SKILL.md              # AI助手技能说明
├── README.md             # 本文件
├── config.json           # 凭证模板（占位符，勿含真实值）
├── .gitignore            # 忽略 config.json / .cache
├── references/
│   └── wechat_api.md     # 微信API详解
└── scripts/
    ├── get_token.py      # 获取 / 缓存 access_token
    ├── upload_thumb.py   # 上传封面图
    ├── create_draft.py   # 创建草稿（主脚本）
    ├── list_drafts.py    # 列出草稿
    └── make_cover.py     # 生成封面图（需 pip install Pillow）
```

---

## 🔐 权限要求

以下能力需在 **微信公众平台 → 设置与开发 → 基本配置** 中确认：

| 功能 | 是否需要微信认证 |
|------|----------------|
| 获取 access_token | ❌ 不需要 |
| 创建草稿 | ✅ 需要 |
| 上传永久图片素材 | ✅ 需要 |
| 查看草稿列表 | ✅ 需要 |

> 未经认证的账号可以先完成开发调试，但无法调用草稿接口。认证后可完整使用。

---

## 🛡️ 安全提示

- **不要**将含真实凭证的 `config.json` 提交到 GitHub
- `.cache/` 目录（access_token 缓存）会自动生成，已加入 `.gitignore`
- 建议定期轮换 AppSecret

---

## 📦 在 OpenClaw AI 助手中安装

### 方法一：SkillHub 自动安装（推荐）

```bash
# 安装最新版本
openclaw skill install wechat-oa-skill

# 或指定版本
openclaw skill install wechat-oa-skill --version 1.0.0
```

### 方法二：手动安装

1. 下载或 clone 本仓库
2. 将整个 `wechat-oa` 文件夹放入 `~/.openclaw/skills/` 或 `~/.qclaw/skills/`
3. 编辑 `config.json` 填入凭证
4. 运行测试脚本验证

---

## 📄 API 参考

详细接口说明见 `references/wechat_api.md`，包括：

- access_token 获取与缓存
- 永久素材上传
- 草稿创建 / 查询 / 删除
- 正文图片处理方案

---

## 📝 使用示例

### Python 调用示例

```python
import subprocess

result = subprocess.run([
    "python", "scripts/create_draft.py",
    "--title", "AI 助手使用指南",
    "--author", "陈叶PPT",
    "--digest", "本文介绍如何使用 AI 助手一键发布公众号文章",
    "--content", "<p>这是正文内容，支持<strong>加粗</strong>和图片。</p>",
    "--thumb-image", "cover.jpg"
], capture_output=True, text=True)

print(result.stdout)
```

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

## 📄 License

MIT License
