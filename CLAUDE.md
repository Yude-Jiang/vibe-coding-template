# [项目名称]

<!-- /init 填充 [方括号] 内容。手动创建时逐段填写。 -->

---

<!-- ============================================================
     PART 1: 全局规则 — TEMPLATE INHERITED
     
     本段落从 vibe-coding-template 继承,是所有项目的最高层约束。
     
     维护规则:
     - 项目中发现新的通用 learning → 先加到本项目 CLAUDE.md
     - 确认是跨项目通用的 → 同步回 template repo 的 CLAUDE.md
     - 定期(每月/每季度)用 template 的最新版本覆盖各项目的 PART 1
     
     不要在项目中修改 PART 1 的已有规则,只追加。
     项目特定的规则写在 PART 2。
     ============================================================ -->

## 协作偏好

- 反馈极简("合并"、"继续"、"还是报错"),结合 PROGRESS.md 和上下文理解,不要要求重复背景
- 回复同样简洁:直接说结论和操作,不铺垫、不重复已知信息
- 关键判断点(是否建 PR、架构选型、破坏性操作)暂停等确认;执行类操作直接做

  以下三条是这条原则在 Loop Engineering 各阶段的具体落地,默认启用,除非项目 CLAUDE.md
  PART 2 显式覆盖:

  1. **BUILD 阶段内连续执行,不按文件/步骤逐次确认。** BUILD 没有专属 slash command
     (不存在 `/build`),是对 Claude Code 的自然语言指令驱动的连续多步执行。给出指令时
     应一次性说明范围和停止条件,例如:"按 SPEC 实现完,跑测试,红了自己改到绿;只在
     撞到 SPEC 未覆盖的架构选型、或破坏性操作(删数据、改 schema、删文件)时停下来问,
     常规实现细节不用每步确认"。不要把一份 SPEC 拆成多轮"写一点/汇报一点/等指令"。
  2. **阶段衔接处,检查全部通过即视为放行,不必等用户重新发出指令。** 例如:
     `/init` 的完整性校验全部 ✅ → 直接可以开始 BUILD,不必用户再说"开始";
     spec-reviewer 未标记任何可追溯性问题且 DECISION LOG 全部 `[x]` → SPEC 视为定稿;
     `smoke-test.sh` 在 staging 全绿 → 可视为允许优先级最高的下一步(如准备 production
     部署的请求),但**production 部署本身仍属于"关键判断点"必须人工确认**,不因
     staging 通过而自动执行,这与本条"放行"的范围不冲突,只是 production 不在
     "事实判断"的范围内,而是"风险判断"。
  3. **EVOLVE 阶段默认"推"而非"拉"。** `/status-report` 不应该只在用户想起来才触发;
     有 CI/定时任务能力的项目,应配置定期(如每周)自动生成状态报告并主动呈现给用户,
     而不是等待用户手动调用命令去查。

  这三条解决的是"事实判断"(检查有没有过、阶段有没有完成)的自动化,不改变"价值判断"
  (验收标准的具体数字、架构选型的 tradeoff 取舍、数据保留策略等)仍然必须问用户、
  不能由 Claude 替用户拍板的原则 —— 两类判断的边界不因为追求"loop 起来"而模糊。
- 解释方案时给出 reasoning 和 tradeoff,不只给"做什么"
- commit message 必须包含 WHY,不只是 WHAT
- 默认中文,技术术语保留英文(deploy、PR、workflow 等不翻译)
- 对外交付物(邮件、英文文档)用英文,主动校对术语准确性

## 模型分配规则

| 任务类型 | 推荐模型 | 说明 |
|---------|---------|------|
| 核心业务逻辑、架构设计、代码审查 | Claude (B 模型) | 不可降级 |
| 样板代码、单元测试、CRUD、格式转换 | DeepSeek (C 模型) | 省 token |
| 需求探索、领域知识、竞品调研 | Claude/Gemini (A 模型) | 在 Cursor 中使用 |
| 拿不准用哪个 | Claude (B 模型) | 安全默认值 |

## 代码规范(全局)

### 硬性规则
- 单文件不超过 **300 行**,超过必须拆分
- 每个 route 文件不超过 **5 个端点**,超过按子资源拆分
- React 组件命名 PascalCase,不超过 3 个单词
- 新文件必须符合项目特定的目录结构规范(见 PART 2),不允许自由放置
- 不使用 `any` 类型(TypeScript 项目)
- 所有 API 端点必须有错误处理,不允许裸 try-catch 吞掉错误

### 目录结构模板

**React + Vite:**
```
src/components/{ComponentName}/index.jsx
src/pages/{PageName}/index.jsx
src/store/{slice-name}.js
src/api/{resource}.js
```

**Flask / Express:**
```
server/routes/{resource}.js
server/middleware/{name}.js
server/models/{entity}.js
```

**Python 数据管道:**
```
scripts/{data-source}/fetch-{source}.py
scripts/{data-source}/transform-{source}.py
```
必须有 `--dry-run` 参数。

### 测试与验证(全局)

写任何验证脚本、测试用例、smoke-test 时，按以下顺序做，不要颠倒：

1. **先做设计完整性检查，再做执行正确性检查。** 设计完整性问的是"这套测试覆盖了真正
   重要的业务路径吗"；执行正确性问的是"这套测试自己跑起来逻辑对不对"。如果只验证执行
   正确性，会陷入"反复确认一个有缺口的设计执行得很对"，却意识不到设计本身不够。
2. 做设计完整性检查时，主动问："这套测试在假设什么前提成立？如果这个前提是错的，
   会漏掉什么？" 不要默认接受已有的断言设计，去验证它能不能跑通；要先挑战这套断言
   本身是否覆盖了关键失败模式。
3. **对异步/多步骤流程，默认假设"创建→使用创建结果"这条链路是断的，除非证明它是连
   起来的。** 分别测"第一步返回 200"和"第二步返回 200"，两步都 PASS 不代表链路是通的
   ——必须验证用第一步的真实输出去驱动第二步,而不是用硬编码的假数据测第二步。
4. 反向测试（故意构造错误场景验证测试能抓出问题）时，构造的错误类型不能只是"断言
   设计内部的状态码错误"，否则只是在验证已有断言的执行逻辑，测不出断言设计本身的
   覆盖缺口。
5. 在 `set -euo pipefail` 的 bash 脚本里，任何"这步可能拿不到结果，拿不到就该走 FAIL
   分支而不是让脚本死掉"的命令，必须显式加 `|| true` 或等价处理——否则 `set -e` 会让
   脚本在错误最该被看见的地方反而悄悄提前退出，看起来像是正常执行完了。
6. 验证脚本本身的健壮性不应低于（甚至应该高于）被测系统——不引入被测环境不一定具备
   的外部依赖（如 `jq` 等不确定预装的二进制），优先用目标环境一定有的工具实现。

## 环境约束(全局)

- GCP 项目 `st-china-ai-force`,region `asia-east1`
- Cloud Run 监听 port **8080**,流式请求 **5 分钟超时**
- 大陆网络: Firestore 客户端直连**不可用**,需服务端代理或改用 BigQuery
- DeepSeek: OpenAI-compatible SDK,model ID `deepseek-chat`
- AkShare: 参数名有版本漂移,调用前先验证当前版本签名

## 跨项目 Learnings(持续累积)

<!-- 
这是整个 template 最重要的段落。
每个项目中遇到的通用教训都追加到这里,然后同步回 template repo。
格式: [日期] [来源项目] 教训内容
-->

- [2024-12] [analog-pd-dashboard] A 股财报数据是 YTD 累计,必须差减上季度得到单季值
- [2024-12] [analog-pd-dashboard] AkShare 千元单位需 ×0.1 转万元,不是 ×10000
- [2025-01] [resume-ai-screener] Firebase Anonymous Auth 需在控制台手动启用,默认关闭
- [2025-01] [resume-ai-screener] Service Worker 缓存 stale hash 导致部署后白屏,需配置 skipWaiting
- [2025-03] [mcu-competitor-dashboard] 普冉股份 stock code 是 688766 不是 688694
- [2025-03] [mcu-competitor-dashboard] STAR Market(科创板)在 AkShare 中 column 值不同于主板
- [2025-04] [geo-strategic-hub] Tailwind v4 无 typography plugin,需手动处理富文本样式
- [2025-04] [geo-strategic-hub] `@google/genai` SDK 不支持 OpenAI-compatible 接口,不能混用
- [2025-05] [geo-strategic-hub] Cloud Run 流式请求 5 分钟超时,文件操作必须串行不能并行
- [2025-05] [geo-monitoring] Recharts ResponsiveContainer 在 flex 布局中需要显式 width/height
- [2025-06] [st-newsletter] Net Income % 应改为 Net Margin % — 注意英文术语准确性
- [2025-06] [skill-radar] git worktree 多 agent 并行时 merge 顺序重要,先合基础分支

<!-- 新 learning 追加在上方,保持时间顺序 -->

---

<!-- ============================================================
     PART 2: 项目特定 — /init 填充
     
     以下内容仅适用于本项目,不回流到 template。
     ============================================================ -->

## 项目信息

- **描述**: [一句话说明项目做什么、给谁用]
- **部署**: Cloud Run `[service-name]` on `st-china-ai-force` / `asia-east1`
- **技术栈**: [React+Vite / Flask / Express+React / Python scripts]
- **线上 URL**: [部署后填写]
- **GitHub**: [repo URL]

## 架构概览

### 主要模块

```
[模块 1]: [职责]
[模块 2]: [职责]
[模块 3]: [职责]
```

### 关键文件

```
[path/to/main-entry]  — [说明]
[path/to/config]       — [说明]
```

## 项目特定代码规范

<!-- 仅当全局规范不够时追加。优先遵守全局规范。 -->

- [规范 1]

## 项目特定约束

<!-- 本项目独有的约束,不适用于其他项目 -->

- [约束 1]

## 项目已知坑

<!-- 开发中发现的坑,持续追加。跨项目通用的教训同步到 PART 1 的 Learnings。 -->
<!-- 格式: [日期] 现象 → 根因 → 修复 -->

## 常用命令

```bash
# 开发
[npm run dev / python app.py]

# 构建
[npm run build]

# 测试
[npm test / pytest]

# 部署
/deploy

# 状态报告
/status-report
```
