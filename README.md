# 选题雷达｜Hermes AI 内容选题 Agent

> 从账号定位与历史内容出发，检索和核验公开信息，生成最多 3 个候选选题与 1 个推荐选题，把最终决定留给创作者。

选题雷达是一个可安装的 [Hermes Profile Distribution](https://hermes-agent.nousresearch.com/docs/user-guide/profile-distributions)。它把岗位目标、工作流程、输出规范、质量门槛和安全边界封装成一个独立 Agent；使用者在自己的电脑上安装，并接入自己的模型、搜索服务、资料目录和飞书机器人。

## 它能做什么

- 读取使用者授权提供的账号定位、关键词与历史选题；
- 围绕定位检索公开网页，记录来源、时间与关键事实；
- 筛除低相关、低可信和重复素材；
- 生成最多 3 个方向不同、可解释、可追溯的候选选题；
- 在证据充分时给出 1 个推荐选题，并说明推荐理由与风险；
- 素材不足或来源冲突时明确报告，不为了凑数给出确定结论。

它不会替使用者确认最终题目、自动发布内容，也不会附带原作者的模型额度、搜索额度、飞书权限、个人资料或历史会话。

## 快速安装

### 1. 准备环境

- Hermes Agent `>= 0.20.1`；
- Git；
- 一个由你自己配置的模型服务；
- 如需联网检索，再准备你自己的搜索服务或 Hermes Web Search。

没有安装 Hermes？请先阅读 [Hermes 官方安装指南](https://hermes-agent.nousresearch.com/docs/getting-started/installation)。

### 2. 一条命令安装

```bash
hermes profile install github.com/YFpsy/topic-radar-github --alias
```

安装器会先显示 Profile 清单并请求确认。`--alias` 会创建同名命令，之后可以直接使用 `topic-radar-agent`。

### 3. 填写你的私有配置

安装完成后，把 Profile 根目录下的 `TOPIC_RADAR_CONFIG.example.md` 复制为：

```text
$HERMES_HOME/local/topic-radar.local.md
```

这里的 `$HERMES_HOME` 指**这个已安装 Agent 的 Profile 目录**。在新文件中填写自己的账号定位、资料范围、历史选题位置和输出偏好。`local/` 属于使用者本地空间，更新 Distribution 时会保留，也不应提交到公开仓库。

### 4. 配置模型与搜索并启动

```bash
topic-radar-agent setup
topic-radar-agent tools
hermes profile show topic-radar-agent
hermes profile info topic-radar-agent
topic-radar-agent chat
```

首次运行可以发送：

```text
请根据我的本地配置，为今天完成一次正式选题：先读取定位与历史选题，再检索并核验公开来源，输出最多 3 个候选选题和 1 个推荐选题。不要发布，等我最终确认。
```

完整的 Windows、macOS、Linux 配置方法、飞书接入、更新、卸载和排错说明见 [安装与部署指南](docs/08-安装与部署.md)。

## 工作流程

```text
读取账号定位与历史内容
        ↓
制定检索方向，搜索公开信息
        ↓
核验原始来源、日期和关键事实
        ↓
筛选素材，与历史主题语义去重
        ↓
生成最多 3 个候选题和 1 个推荐题
        ↓
记录证据与风险，等待创作者确认
```

## 安装后你得到什么

| 内容 | 是否包含 | 说明 |
| --- | --- | --- |
| Agent 岗位、流程与行为边界 | 是 | 由根目录 `SOUL.md` 定义 |
| Hermes Distribution 清单 | 是 | 由 `distribution.yaml` 定义 |
| 私有配置模板 | 是 | 只有字段示例，没有真实资料 |
| 模型或搜索 API Key | 否 | 使用者自行配置 |
| 飞书机器人和权限 | 否 | 可选，由使用者自行创建 |
| 原作者笔记、素材和运行记录 | 否 | 不随仓库分发 |
| 自动定时任务 | 否 | 当前版本需使用者自行触发或配置 |

当前 Distribution 安装的是**选题 Agent 的身份与工作流**，不是一套开箱即用的外部账户。能否完成联网检索，取决于使用者是否正确配置模型、搜索工具和可读取的本地资料。

`SOUL.md` 和提示词只能约束 Agent 的行为，**不能代替真正的访问控制**。文件读写权限、飞书使用者白名单、搜索次数、模型预算和运行时长上限，都应在操作系统、Hermes Gateway 或对应服务后台设置。

## 项目组成

| 组件 | 职责 |
| --- | --- |
| Hermes | 加载 Profile，执行研究、核验、去重与选题流程 |
| 搜索服务 | 检索公开网页；可使用 Hermes 支持的服务或自己的提供商 |
| 飞书（可选） | 发起任务、接收进度与查看结果 |
| Codex（维护端） | 辅助设计、检查和迭代 Agent 项目，不是安装后的运行依赖 |

## 仓库结构

```text
.
├─ distribution.yaml                  # Hermes Distribution 清单
├─ SOUL.md                            # 实际安装的 Agent 身份与工作流
├─ TOPIC_RADAR_CONFIG.example.md      # 私有运行配置模板
├─ README.md
├─ .gitignore
├─ config/
│  └─ config.example.md               # 配置字段说明
└─ docs/
   ├─ 01-岗位卡.md
   ├─ 02-工作流卡片.md
   ├─ 03-流程图.md
   ├─ 04-Hermes-Profile.template.md   # 阅读参考，不是安装入口
   ├─ 05-Agent使用说明书.md
   ├─ 06-脱敏输出示例.md
   ├─ 07-测试与验收.md
   └─ 08-安装与部署.md
```

## 更新与卸载

```bash
# 获取仓库中的新版本；本地私有数据与 local/ 不会被覆盖
hermes profile update topic-radar-agent

# 删除整个本地 Profile；执行前请备份自己的配置和运行数据
hermes profile delete topic-radar-agent
```

## 隐私与安全

- 不要把 API Key、App Secret、访问令牌或验证码写进 Markdown 或提交到 Git；
- 只向外部搜索服务发送完成检索所需的非敏感关键词，不发送原始笔记、草稿、评论、个人信息或密钥；
- 使用云端模型时，模型提供商可能接收完成任务所需的运行上下文；部署者应先确认提供商政策并限制授权资料范围；
- 为飞书机器人设置最小权限和使用者白名单；
- 在 Gateway 或服务后台设置搜索、模型与重试预算；不要把成本控制只写在提示词里；
- `SOUL.md` 是行为规则，不是技术访问控制；文件权限、触发白名单和费用上限必须在系统层真正限制；
- 运行前先阅读 `SOUL.md`，确认 Agent 的行为规则符合你的预期；
- 发布自己的衍生仓库前，同时检查当前文件和 Git 历史。

## 文档

- [岗位卡](docs/01-岗位卡.md)
- [工作流卡片](docs/02-工作流卡片.md)
- [流程图](docs/03-流程图.md)
- [使用说明书](docs/05-Agent使用说明书.md)
- [脱敏输出示例](docs/06-脱敏输出示例.md)
- [测试与验收](docs/07-测试与验收.md)
- [安装与部署](docs/08-安装与部署.md)

Hermes Profile Distribution 的安装、更新和数据保留行为，以 [Hermes 官方说明](https://hermes-agent.nousresearch.com/docs/user-guide/profile-distributions) 为准。
