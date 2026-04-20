---
name: wechat-oa
description: 微信公众号运营技能。自动获取 access_token、上传图片、创建草稿、发布文章到微信公众号草稿箱。触发词：发公众号、发微信文章、发布到公众号、微信公众号、微信文章草稿、微信图文。
---

# 微信公众号草稿发布技能

将文章一键发布到微信公众号草稿箱，支持封面图上传、自动获取 access_token、草稿管理。

## 工作流程

1. 读取 config.json 获取凭证
2. 获取 access_token（自动缓存，过期前刷新）
3. 上传封面图获取 thumb_media_id
4. 将文章内容转为 HTML 并创建草稿
5. 返回草稿预览链接

## 核心脚本

| 脚本 | 用途 |
|------|------|
| `scripts/get_token.py` | 获取/缓存 access_token |
| `scripts/upload_thumb.py` | 上传封面图 |
| `scripts/create_draft.py` | 创建草稿并发布到草稿箱（主脚本）|
| `scripts/list_drafts.py` | 列出草稿箱内容 |
| `scripts/make_cover.ps1` | 生成封面图（Windows PowerShell）|

## 使用方法

### 基础发布

```bash
python scripts/create_draft.py --title "文章标题" --content "正文HTML" --author "作者" --digest "摘要"
```

### 带封面图发布

```bash
python scripts/create_draft.py --title "文章标题" --content "正文HTML" --author "作者" --digest "摘要" --thumb-image "C:/path/to/cover.jpg"
```

### 查看草稿列表

```bash
python scripts/list_drafts.py
```

### 响应示例

```
access_token 获取成功：62_xxx字符...
封面图上传成功，media_id：xxx
草稿创建成功！
标题：XXX
media_id：xxx
请前往公众号后台确认草稿内容。
```

## 配置说明

配置文件：`config.json`（仓库中为占位符模板）

```json
{
  "appid": "wx开头的AppID",
  "appsecret": "32位AppSecret",
  "account_name": "你的公众号名称",
  "cache_dir": ".cache"
}
```

> ⚠️ config.json 已加入 .gitignore，不要将含真实凭证的文件提交到 GitHub。

access_token 自动缓存到 `.cache/access_token.json`，有效期约2小时，脚本会自动刷新。

## 权限要求

| 功能 | 是否需要微信认证 |
|------|----------------|
| 获取 access_token | ❌ 不需要 |
| 创建草稿 | ✅ 需要 |
| 上传永久图片素材 | ✅ 需要 |

详细 API 说明见 `references/wechat_api.md`。

## AI 助手触发词

- 发公众号 / 发微信文章
- 发布到公众号
- 创建草稿 / 推送到草稿箱
