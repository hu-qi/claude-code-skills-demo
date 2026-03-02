---
name: write-response
description: 撰写建议函的智能助手。当用户需要撰写建议函、意见书、建议书时触发此技能。通过 API 获取专业的撰写指南，帮助用户高效完成正式建议文档的起草。触发词包括：建议函、意见书、建议书、撰写建议、写建议、起草建议、提出建议、拟写建议函。
---

# 建议函撰写助手

此技能帮助用户撰写专业的建议函、意见书等正式建议文档。

## 触发条件

当用户提到以下关键词时触发：
- 建议函、意见书、建议书
- 撰写建议、写建议、起草建议
- 提出建议、拟写建议函
- 需要写一份建议

## 工作流程

### 1. 获取撰写指南

首先执行 Python 脚本从远程 API 获取详细的撰写指南：

```python
import requests

def fetch_writing_guide():
    """从远程 API 获取建议函撰写指南

    可用的免费 API 选项：
    1. GitHub Raw 文件（推荐）：
       - 将提示词保存为 GitHub 仓库或 Gist 中的文件
       - 使用 raw.githubusercontent.com 的 URL
       - 示例：https://raw.githubusercontent.com/用户名/仓库名/main/guide.md

    2. JSONPlaceholder（测试用）：
       - https://jsonplaceholder.typicode.com/posts/1
       - 返回 JSON，需要解析 .body 或 .content 字段

    3. 自建 API：
       - 使用 Cloudflare Workers / Vercel Serverless Functions
       - 免费额度足够个人使用
    """
    # 配置你的 API URL（支持 GitHub Raw、Gist 或其他免费 API）
    api_url = "https://raw.githubusercontent.com/你的用户名/你的仓库/main/suggestion-letter-guide.md"

    try:
        response = requests.get(api_url, timeout=10)
        response.raise_for_status()
        return response.text
    except requests.RequestException as e:
        return f"获取指南失败: {e}"

# 执行获取
guide = fetch_writing_guide()
print(guide)
```

**注意**：如果 API 获取失败，使用内置的备用指南（见 `fallback-guide.md`）。

### 2. 展示指南供用户确认

获取到撰写指南后，向用户展示指南内容摘要：

```
📋 已获取建议函撰写指南，主要包含以下内容：

[展示指南的核心要点和结构]

请确认是否按照此指南开始撰写？如需调整或有特殊要求，请告知。
```

### 3. 收集用户需求信息

用户确认后，询问以下关键信息：

1. **建议对象**：建议函是写给谁的？（个人/部门/机构）
2. **建议主题**：主要建议内容是什么？
3. **背景情况**：为什么要提出这个建议？
4. **期望结果**：希望对方采取什么行动？
5. **语气风格**：正式公文/商务沟通/友好建议

### 4. 撰写建议函

根据获取的指南和用户提供的信息，撰写建议函：

- 遵循指南中的结构和格式要求
- 使用恰当的正式语言
- 确保逻辑清晰、论据充分
- 包含明确的建议事项和期望

### 5. 审阅与修改

完成初稿后：
- 展示完整文档供用户审阅
- 根据用户反馈进行修改
- 确保文档符合用户期望

## 输出格式

建议函应包含以下部分：

1. **标题**：清晰表明建议主题
2. **称呼**：对建议对象的恰当称呼
3. **背景说明**：阐述提出建议的原因和依据
4. **建议内容**：分条列出具体建议
5. **预期效果**：说明建议实施后的预期效果
6. **结语**：礼貌的结束语
7. **落款**：撰写人/单位及日期

## 文件引用

- `fallback-guide.md` - API 获取失败时的备用撰写指南
- `templates/` - 各类建议函模板目录

## 注意事项

- 保持客观、理性的语气
- 建议要有可行性和可操作性
- 注意措辞的得体性和礼貌性
- 根据建议对象调整正式程度