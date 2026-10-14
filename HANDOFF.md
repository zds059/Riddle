# Riddle 项目交接 Brief

> 用途：把云端 / 手机端会话结论带到 **tom's Mac mini**（Remote Control）继续工作时，把本文件作为开场上下文。  
> 更新：2026-10-14

---

## 1. 一句话现状

`github.com/zds059/Riddle` 目前几乎是空仓库（`README.md` + Python `.gitignore`）。真正要继续的工作不在 Git 里，而在 **Project Agent Store**、本机 Mac 路径，以及已挂载的用户 Skills。

---

## 2. 目标与意图

| 优先级 | 意图 | 状态 |
| --- | --- | --- |
| P0 | 在 Mac 上用 Remote Control 继续访问已有项目分析内容 | 进行中：本机 worker 已在线，但多数会话仍跑在云端 VM |
| P0 | 挂载 / 同步 Project Store `868c977b-5b81-4eca-b021-44b295839de3` | 未完成：云端会话对该 store 返回 403 |
| P1 | 把分析结论与路径写成可入库 brief，供 My Machines 新任务使用 | 本文档 |
| P2 | 完善 Riddle Cloud Agent 环境（需用户在面板 Save） | 草稿 build 曾成功，Save 状态未知 |

---

## 3. 关键 ID 与机器

| 类型 | 值 |
| --- | --- |
| GitHub 仓库 | `https://github.com/zds059/Riddle` |
| Project Store ID | `868c977b-5b81-4eca-b021-44b295839de3` |
| 期望本机 Store 路径 | `~/Library/Application Support/Cursor/AgentStores/cursor_agent_stores/868c977b-5b81-4eca-b021-44b295839de3` |
| Mac 机器 | `tom's Mac mini` |
| Worker 显示目录 | `/Volumes/zhai.apple/others` |
| Worker ID | `21e0f771-6230-54a7-a5c4-349fa94e309c` |
| 备查数据盘 | `/Volumes/zhai.apple/` |
| 云端环境 ID | `ef37570a-c2ef-11f1-bb68-864e54d14197` |
| 已挂载用户 Skills | `/cursor/stores/user/skills/single-cell`、`/cursor/stores/user/skills/st-pipeline` |

---

## 4. Remote Control 结论（必须记住）

1. **开了 Remote Control ≠ 当前云端会话能读本机文件。** 会话仍在云端 VM 时，碰不到 Mac 上的项目。
2. 要用 Remote Control：新建任务，运行位置选 **My Machines → tom's Mac mini**。
3. Worker 需指向目标 **Git 仓库目录**；Git remote 必须与触发仓库一致。
4. Worker 进程保持运行；机器尽量不休眠。
5. **当前云端会话不能中途切到 Mac**——要迁工作就开新的 My Machines 任务，并附上本 brief。

### Mac 侧建议命令

```bash
# 1) 拉取本仓库
cd /path/you/want
git clone https://github.com/zds059/Riddle.git
cd Riddle
git pull

# 2) 在该目录启动 worker（名称可自定）
agent worker start --name "toms-mac-mini" --worker-dir "$(pwd)"

# 3) 若真正业务仓库不在 Riddle，改为业务仓库目录再 start，
#    并保证该目录的 git remote 与你在 Agents 里选的仓库一致。
```

手机继续分析：在 Mac Cursor 打开 Remote Control 后，用同一账号在 iOS / Web Agents 里选 **My Machines** 任务。

---

## 5. 相关会话摘要

### 5.1 项目信息云同步

- 链接：https://cursor.com/agents/bc-9871f7cd-ed55-4ef6-8b53-8d57157f822c
- 用户给出 Project ID，希望同步到云并继续访问内容。
- **结论：** 该 UUID 是 Project Agent Store，不是普通 Cloud Agent 环境；当前 Riddle 云端会话 **无权挂载**（`MintAgentStoreToken` → HTTP 403）。
- 个人 Skills（`single-cell`、`st-pipeline`）在云端可读。
- 正确路径：在 Agents Window 打开该 Project，从 Project 内启 Cloud Agent / coordinator，才能挂载共享上下文。

### 5.2 Mac Store 查找 / 同步 / 读取 / 开启 Remote Control

| 会话 | 链接 | 结果 |
| --- | --- | --- |
| Find project store on Mac | https://cursor.com/agents/bc-a2678101-5db8-5d10-94de-eb79ee025e49 | 落在 Linux 云 VM，未读到 Mac store |
| Mac project store sync | https://cursor.com/agents/bc-760428b4-ae16-533f-b1dd-1b1c6ffc8ebc | 未执行同步；worker 曾在线但任务未落到 Mac |
| Read project store on Mac | https://cursor.com/agents/bc-0f008c7f-431e-5bdc-9d01-aeec992225df | 无 inventory / 无文件内容 |
| Enable Remote Control on Mac | https://cursor.com/agents/bc-e5a08e1d-76a8-55de-a059-54ec2e97496e | 确认需在 Mac GUI 开启；本会话 `usePrivateWorker=false` |

**共同阻塞：** 子任务多次未能真正放到 worker `21e0f771-...`，因此本机路径盘点与 store 同步都未完成。

### 5.3 环境与入门

- **Set up Riddle environment** https://cursor.com/agents/bc-5b23734d-944f-4238-bb62-9a8a6c47d728  
  空仓库环境搭建；草稿 build 成功；用户需在 Cloud Agents 环境面板 **Save**。
- **Running your first Cloud Agent** https://cursor.com/agents/bc-8de696b9-b1bb-4983-ac7d-04d74a093c45  
  Onboarding；另有黑色素瘤分选生物学问答（与仓库代码无关）。
- **Cursor iOS 应用会话** https://cursor.com/agents/bc-d62af184-a337-46be-a986-691db84e238f  
  指引如何用 iOS 收件箱打开同一会话；无代码改动。

### 5.4 当前会话（远程访问讨论）

- 链接：https://cursor.com/agents/bc-01a138a2-2394-7910-9a2e-92af7449b2cc
- 确认：worker 在线，但本会话在云端；迁移优先用 **Git + brief**，大数据再 zip。
- 产出：本 `HANDOFF.md`。

---

## 6. 已确认的技术事实

- 云端工作区实质内容：`README.md`（`# Riddle`）、通用 Python `.gitignore`。
- Project Store `868c977b-...` 在云端 Riddle 会话中 **不可挂载**。
- 用户 Skills 可用：单细胞（`single-cell` / `sc_pipeline`）、空间转录组（`st-pipeline`）。
- 对话记忆 **不会** 随 Remote Control 自动迁移；必须依赖 brief / 仓库文件 / Project Store。

---

## 7. 未决问题（到 Mac 后优先做）

- [ ] 在 **My Machines → tom's Mac mini** 上开新任务，并附上本文件路径。
- [ ] 确认 Project Store 目录是否存在，列出其中 `*.md` / `*.json` / `*.txt`。
- [ ] 检查 `.sync/index.sqlite`、`sync.lock`、同步是否卡住；必要时备份未上传文件后触发同步。
- [ ] 若业务分析实际在别的 Git 仓库：clone 到 Mac，用该目录启动 worker，不要只挂空的 Riddle。
- [ ] 在 Cloud Agents 面板确认 Riddle 环境是否已 Save。
- [ ] 把真正的分析结论（数据路径、样本、已跑 pipeline、关键图表）补进下方「业务分析」模板。

---

## 8. 迁移清单（推荐顺序）

1. **代码 / 可版本化内容 → Git**  
   commit + push → Mac `git pull` → 在仓库目录 `agent worker start`。
2. **分析上下文 → 本 brief**  
   新 My Machines 任务首条消息：`请先阅读仓库根目录 HANDOFF.md，然后继续第 7 节未决项。`
3. **大数据 / 非 Git 文件 → 打包拷贝**  
   zip / 网盘 / 外置盘放到 `/Volumes/zhai.apple/` 或项目旁；**不会**带上聊天记忆。
4. **Project Store**  
   从该 Project 内启动 Agent，或在 Mac 上直接读本地 AgentStores 路径。

---

## 9. 给下一个 Agent 的开场提示（可直接复制）

```text
请阅读仓库根目录 HANDOFF.md。

当前目标：
1) 确认你是否运行在 tom's Mac mini（uname 应为 Darwin）；
2) 定位 Project Store 868c977b-5b81-4eca-b021-44b295839de3；
3) 盘点其中分析文档与同步状态；
4) 把可读的分析结论追加写回 HANDOFF.md「业务分析」一节。

不要假设云端会话已挂载该 store。若你仍在 Linux 云 VM，请停止并说明需要 My Machines 放置。
```

---

## 10. 业务分析模板（待填充）

> 把真正的课题内容写在这里；目前相关会话几乎都在解决「如何访问 / 同步」，尚未把科学结论固化进仓库。

### 10.1 课题一句话

- （例：某某样本的单细胞 / 空间转录组分析）

### 10.2 数据位置

| 数据集 | 路径 / 来源 | 备注 |
| --- | --- | --- |
| | | |

### 10.3 已完成步骤

- [ ] …
- [ ] …

### 10.4 关键结论

1. …
2. …

### 10.5 下一步实验 / 分析

1. …
2. …

### 10.6 重要命令与环境

```bash
# 例
# sc_pipeline ...
# st_pipeline ...
```

### 10.7 图表 / 产物索引

| 文件 | 含义 |
| --- | --- |
| | |

---

## 11. 会话索引（Riddle 相关）

| 名称 | bcId | 备注 |
| --- | --- | --- |
| Yuancheng xiangmu fangwen | `bc-01a138a2-2394-7910-9a2e-92af7449b2cc` | Remote Control / 迁移 / 本 brief |
| 项目信息云同步 | `bc-9871f7cd-ed55-4ef6-8b53-8d57157f822c` | Project Store 403 |
| Cursor iOS 应用会话 | `bc-d62af184-a337-46be-a986-691db84e238f` | 手机打开会话 |
| Enable Remote Control on Mac | `bc-e5a08e1d-76a8-55de-a059-54ec2e97496e` | GUI 开启指引 |
| Read / Sync / Find project store | `bc-0f008c7f-…` / `bc-760428b4-…` / `bc-a2678101-…` | 未落到 Mac |
| Set up Riddle environment | `bc-5b23734d-944f-4238-bb62-9a8a6c47d728` | 环境草稿 |
| Running your first Cloud Agent | `bc-8de696b9-b1bb-4983-ac7d-04d74a093c45` | Onboarding + 生物问答 |
