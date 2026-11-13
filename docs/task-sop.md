# GPT 需求发现定时任务 SOP 草案（待用户审核，未启用）

版本 v0.5 / 2026-11-13（Issue #1 第四轮复审修订）。此文件是供未来 GPT 自动任务读取的执行规格，**并未创建任何定时任务**。

## 文档权责

本文件是唯一执行规范：**状态定义、升级门槛、评分、去重键、周轮换、写入规则**只能在这里维护。`docs/methods.md` 只描述 M01–M12 的操作、证据类型、可用数据源及限制。发生表述不同时以本 SOP 为准，修订时同步检查方法文档是否仍有相互冲突的陈述。

## 目标
发现适合单人低成本运营、英语 SEO 可获客的精品工具页、Micro SaaS、AI 图片/视频细分工具及 H5 游戏相关需求。优先开发验证时间在 7 天内、总计约 8–12 工时以内、可复用 Next.js/React/Tailwind/Cloudflare 或 Vercel 的机会。先需求验证，不直接开发。

## 启动前的强制阅读
1. 读取仓库 `https://github.com/owenlee669/reqs-geifei` 的 main 分支 `docs/methods.md` 和本文件的**最新版本**。
2. 每次读取 `data/opportunities.jsonl` 的当前去重快照和必要候选状态；周日额外读取 `data/history.jsonl` **本周事件**，只根据事件形成周总结，注明「基于候选状态变更事件，非逐日扫描报告回顾」。均不加载 `reports/` 历史全文。大文件分段读取、保留必要索引并注明覆盖范围；文件不存在视为首次运行，禁止伪称已去重。
3. 文中原始分享是**方法**，不能当作当天新发现、也不能拿旧数据直接作为当前机会。
4. 若 GitHub 无法读取、某网站 403、无付费 API，记录不可用；改用公开可访问的合规数据，不准虚构查到的结果。

## 固定周轮换表

| 星期 | 方法组合 | 工作重点 |
|---|---|---|
| 周一 | M01 + M02 | 产品/付费 App 榜单和新增 sitemap 页面 |
| 周二 | M05 + M06 | 差评、替代品和社区抱怨 |
| 周三 | M03 + M04 | 关键词扩展、公开 SERP 与付费墙 |
| 周四 | M08 + M09 | 新游戏/AI 模型与 GitHub Skills/Issues |
| 周五 | M01 + M05 | 新增产品和竞品差评交叉检查 |
| 周六 | M10 + M07 | 平台子域名 + 有公开定价/真实任务证据的人工服务 |
| 周日 | 去重与周总结 | 只汇总候选池本周状态变化，不读逐日报告；推荐 1 个重点人工验证项目 |

M12 只在**周六 M10 + M07 均已完成且当前合格推荐少于 3 条**时触发，最多产出 1 条候选；否则在汇总注明 `M12 未触发`。不挤占 M10/M07 固定组合。M11 只在仓库 `inputs/gsc-ga4/` 下有用户授权导出文件时作为额外人工数据分析，不替换固定周六组合；无文件则跳过并记 `blocked: missing user exports`。最多两个主要发现渠道；M03、M04 也可作为跨日验证步骤。渠道不可访问时记录 blocked，允许切换并记录原因。**日期基准与轮换星期统一使用 `Asia/Shanghai`（UTC+8）**，日报头部必须注明 `日期基准：Asia/Shanghai（UTC+8）`；若用户未来更改时区，先明确修改本 SOP 及对应任务设置后再生效，不能依赖运行服务器的时区。本次不创建任何日程。

## 8 步执行：发现阶段允许证据缺失

1. 保存具体原始网页、帖子、评论或 Issue 直链、原始发布时间和检查日期；无法访问内容则说明仅看到摘要，不伪称核实。
2. 用一句话记录目标用户、场景、动作和困难；首次只需一条可追溯线索，即可记录为 Signal。
3. 将真实动作提炼成最多 3 个自然英文搜索词，并标注搜索意图和地区；完成后可标 Keyword Candidate，不要求第二份证据。
4. 寻找第二份独立需求证据。Signal/Keyword Candidate 可写 unknown；不同作者和不同原始行为才算独立，转帖与转载不算。
5. 寻找明确商业化证据，包括实际用户付费、可靠的已付费服务、明确预算/购买/切换意愿或有用户使用迹象的收费替代。早期可记 unknown；价格、结账访问和广告并不等于成交。
6. **公开 SERP 实查由 GPT 必做**：保存地区、检查日期、已检查结果数量/类型、真实结果 URL，判断搜索意图及工具供给缺口；未看到完整前十时不得写「前十无工具」。**付费增强盘面**（DR/域龄/竞品流量估计、volume/KD/CPC）只有合法授权来源才填写，缺失均记 `unknown` 交人工核查；**付费增强指标缺失不妨碍公开 SERP 检查通过，也不能单独阻止升级 Validated**。没有任何可访问 SERP 则标未核查，不得升级。
7. 写清 7 天 MVP 的输入、处理、输出、API 依赖、成本、8–12 工时可行性、至少一个需求测试动作，以及版权、商标、合规风险。
8. 按本文件「稳定去重 ID」规范形成 ID；快照只存当前状态、分数、证据与 first_seen/last_seen。发生状态或评分变化才向 `data/history.jsonl` **追加单条事件**，不在快照内嵌历史数组；同一原始证据 URL 去重。

**Validated Opportunity 的硬门槛**：至少两条独立真实需求证据、一条可信商业化证据，以及经确认的 SERP 分析和 MVP 可行性判断。得分高不能绕过门槛；Signal 和 Keyword Candidate 不因缺证据而禁止入库。

## 方法数据依赖与降级路径

- **M01**：benny 的域龄、月访问量及搜索/直接流量占比四阈值仅供人工复核；自动流程只用公开新品、真实产品功能、价格及评论，不以未获付费流量数据为淘汰条件。
- **M04**：自动流程仅访问公开定价、免费额度、App 内购价及可访问的公开收入披露；Stripe 等支付/引荐估算、收入排行榜付费数值不可得则 `unknown` 并交人工，不能凭价格或收银台访问虚构实际成交。
- **M09**：自动流程只使用公开仓库搜索、README、Issues/Discussions 等可访问资料；`filename:SKILL.md`、`path:skills` 等登录后代码搜索属于人工授权补充。若不能访问则标记 `blocked`，不冒充搜索已完成。
- **M02**：快照保存在 `data/sitemaps/<domain>.txt`，历史版先归档 `data/sitemaps/archive/<domain>/<timestamp>.txt` 再覆盖；首次建立基线不算新增，失败不可清空旧快照。候选池仅保留新增 URL 摘要。
- **M08/TikTok**：如需要登录、反爬或授权，则不自动抓取；只处理用户授权提供的可追溯数据。
- **M11**：没有有效用户授权聚合导出时跳过并记 `blocked`，不尝试使用用户的登录态。

## M11 用户授权数据的投递规范

仅从仓库 `inputs/gsc-ga4/` 读取用户明确提供的导出文件，不默认能访问 GSC 或 GA4 登录账户。推荐命名 `gsc_queries_pages_YYYY-MM-DD.csv` 与 `ga4_landing_pages_YYYY-MM-DD.csv`；GSC 至少包含 date、query、page、clicks、impressions、ctr、position（若导出是区间聚合数据，另附起止日期说明）；GA4 至少包含 date、landing_page、sessions、engagement_rate、average_engagement_time，及分析范围。两份数据必须标明属性/站点、时区、时间区间及维度定义。按 **域名+路径** 关联：从 GSC 完整 URL 提取 hostname/pathname，GA4 路径补充站点域名；去协议、`www.`、尾斜杠、查询参数；默认路径小写，但若实际路由区分大小写，应遵从 canonical 防止错配。统一属性/时区，时间范围取两份导出的交集，记录各自最新日期（GSC 数据常延迟，不强行假定恰好两天）。报告匹配、未匹配页面数量及未匹配占比；页面级 GA4 参与指标**不能归因到某一个 GSC 搜索词**，不能将不同粒度直接解释为查询级停留时间。缺一份、无授权、格式不符或数据无法访问时：**跳过 M11 并记 blocked**，不能声称已完成检查。仓库是公开的，严禁上传含个人身份信息、密钥、用户级事件记录的原始数据。

## 人工补数据及数据分工

任务负责获取公开可访问的产品、帖子、评论和 SERP，提炼用户动作、关键词、竞品类型及供给缺口。用户用 Semrush/Ahrefs 等核实无法获取的 volume、KD、CPC；不可获取的值必须显示 unknown 并进入「人工待核」部分。

小红书、抖音、TikTok、闲鱼、招聘 JD，以及需要登录或有反爬限制的 YouTube 评论，不绕过限制自动抓取；只接收用户授权提供的截图、文本、合法导出或链接，并标记人工提供、尚需复核。公开可合法访问的 YouTube 评论可分析。中文平台受众数据不能自动转化为英语市场证据。

## 产出数量与注意力限制

每天最多展示 **3 条达到推荐线**的新发现或显著升级候选；低于推荐线只入候选池/Watchlist，不凑数。按证据可靠度、得分率及覆盖维度排序，超出配额的候选存池待核；**`recommended_today` 只是日报展示布尔值，不改变候选 `status`，被挤出前三不降级 Watchlist**。周日只推荐 1 个值得深挖的项目。允许 0 条。单条 Signal 只归档不推荐。报告分别统计 Signal、Keyword Candidate 与 Validated Opportunity，不把三者都称为已验证成功。
## 稳定去重 ID（唯一规范）

- 按自然英文主需求 query 归一化：Unicode NFKC、trim、转小写、合并连续空格、删除不影响含义的标点；在语义可靠时合并常见词形单复数和同义动词（如 remove background / background removal），必须记录规范化前后的词与映射理由；品牌专有名词不可随意替换。
- `search_intent` 固定枚举：`tool`（操作型）、`howto`（教程）、`alternative`（替代/商业）、`navigation`（导航）、`informational`（资料）；只选主意图，无法判断进 Signal 待核。
- `scenario_type` 固定枚举：`single_use`（单次使用）、`batch`（批量处理）、`professional_workflow`（专业工作流）、`developer_integration`（开发者集成）。记录为属性，**不采用可自由改写的场景描述参与 ID**。
- **ID = SHA-256(`normalized_query + "|" + search_intent + "|" + scenario_type`)** 的小写完整十六进制；使用稳定 UTF-8 字节，不依赖域名/自由文案/运行日期。不同关键词同义归并必须保存 alias，存在明显场景差异时使用固定 scenario_type 保留独立需求；同一需求的两种自由措辞若规范化到同一主词和场景枚举，应生成相同 ID。
- 如果初始信号尚不能确定 query/intent/scenario 枚举，允许存 `provisional:<source URL hash>` 并标待规范化；升级时合并 provisional ID 的证据到正式 ID，历史事件注明 `id_migrated`，防止重复。**不能伪造一个英文词来凑 ID**。

## 状态与评分（分阶段、缺失数据不扣零分）

- `Signal`：一条可追溯原始线索即可入库；只记录原始信号与各维度可观察信息，**不打总分、不进入每日推荐**。
- `Keyword Candidate`：用户动作、自然英文关键词和意图清晰；第二份独立证据、付费信息可为 unknown，不因此拒绝。
- `Validated Opportunity`：必须同时具备 ≥2 条独立需求证据、≥1 条可信商业化证据、可核验的 SERP 和 MVP 可行性检查；高分不可替代这些门槛。
- `Watchlist`：数据待核、缺关键证据或趋势尚不明朗；**数据缺失不构成 Rejected 理由**。
- `Rejected`：证明确为重复无增量、明显不匹配目标、版权/合规否决、开发成本显著超限等；记录确切理由。

| 维度 | 满分 | 有依据才评分 |
|---|---:|---|
| 真实痛点/独立来源 | 25 | 可复核用户行为、频率、证据 |
| 商业化/付费证据 | 20 | 报价并有采用迹象、实际支付、预算或切换意愿 |
| SEO 获客机会 | 20 | 公开 SERP 搜索意图、工具供给缺口、真实竞争格局；不能编造 KD/volume |
| 7 天 MVP 可实现性 | 20 | 功能范围、API 与维护成本有合理依据 |
| 持续性与差异化 | 15 | 长期场景、细分差异、重复使用 |

**计分算法**（仅 Candidate 和 Validated 适用）：
1. 对有足够可信依据的维度记录 `earned/max`；无依据填 `unknown`，整个维度**不进分子也不进分母**，不可按 0 计算。
2. `covered_weight = Σ(已评分维度满分)`；`normalized_score = Σ(已获分) / covered_weight × 100`，注明 **「基于 N/5 维度、覆盖权重 W/100」**。若覆盖权重为 0 则得分 unknown。
3. **每日推荐入围**：`normalized_score >= 60`、至少 **3/5 个维度可评分**，且**「商业化/付费证据」或「SEO 获客机会」至少一维有真实可核查依据可评分**，并有原始证据直链及可执行的下一步；否则仅归档/Watchlist。排序先看证据可靠度，再看得分率与覆盖维度；相同时优先 Validated。推荐入围不意味着自动成为 Validated。**例子**：只有「痛点 + MVP + 持续性」三维且均高分，仍因商业和 SEO 双双 unknown 被拦下；而「痛点 + MVP + SEO」有实际 SERP 证据可按标准参评。
4. `normalized_score < 60` 不进入每日推荐；如低分主要由无法取得的关键数据引起，保留 Watchlist/人工待核而非 Rejected。实查可得信息证明需求很弱时才可给该维度低分，**“缺失”与“否定性证据”必须区别**。
5. 版权侵权、禁止的采集方式、不可负担 API 等单独检查，不能因为评分高跳过。

**例子**：某 Candidate 有需求 18/25、MVP 16/20、持续性 11/15，商业和 SEO 均 unknown：45/60=75 分（3/5 维），**因缺商业与 SEO 两项关键证据，不进入每日推荐，只留 Watchlist/待核**。另一个 Candidate 有需求 18/25、MVP 16/20、SEO 12/20（公开 SERP 可核查），46/65≈70.8 分（3/5 维）可推荐但不自动升级。仅 MVP 20/20 不能因标准化得分 100 就推荐。

## 输出格式（每条）

```markdown
### 候选名称 — 状态（Signal / Keyword Candidate / Validated Opportunity）
- 日期：首次发现 / 本次检查
- 方法：Mxx；去重键：标准化英文需求词+搜索意图+用户场景（域名可为空）
- 用户：谁 / 场景 / 高频操作与困境
- 用户动作：英文一句话
- 关键词：英文词1 / 词2 / 词3（各自意图）
- 证据1：原始直链 + 日期 + 简短原话或行为
- 证据2：独立原始直链 + 日期 + 简短原话或行为（没有则未验证）
- 付费信号：具体替代方案、定价或购买意愿 + 来源（没有则未验证）
- SERP：搜索引擎/地区/日期、真实访问的结果 URL、已核查数量、可能的供给缺口（未查到填 unknown）
- 数据：volume、KD、CPC、trend及来源/日期；无法查写 unknown 并放入人工待核
- 评分：Signal 填“未评分”；其他状态逐维填写 earned/max 或 unknown，并给出 normalized_score、覆盖 N/5 与 W/100；是否达到 ≥60 推荐线
- 7天 MVP：输入 → 处理 → 输出；试验方式；主要开发/运维成本
- 风险：版权/商标/API/数据访问/竞争
- 下一步：一个具体待验证动作
```

运行汇总：固定轮换组合、检查来源、原始信号/去重/各状态/升级/淘汰数量、最多 3 个达到推荐线的候选（可以为 0）、缺失指标人工待核清单，以及访问失败说明。周日报告只基于候选池本周的状态/评分变化，并明确声明不是逐日扫描报告的全文回顾。**无合格机会时如实写 0，不用拼凑日报。**

## 仓库存储与幂等写入规范

| 路径 | 唯一用途 | 写入方式 |
|---|---|---|
| `data/opportunities.jsonl` | 每 ID 一行的**当前快照**；包含 id、normalized_query、search_intent、scenario_type、status、last_score、score_coverage、first_seen、last_seen、evidence_urls、aliases | 同 ID 合并、**整份读取-合并-重写**；**禁止 status_history/score_history 内嵌数组** |
| `data/history.jsonl` | **只追加事件历史**，格式 `{"id":"...","date":"ISO-8601 Asia/Shanghai offset","field":"status|last_score|id_migrated","from":...,"to":...,"evidence_url":"..."}` | 仅真实变更时追加，按事件稳定键（id、date、field、from、to、evidence_url）去重；**不按 ID 覆盖历史** |
| `reports/YYYY-MM-DD.md` | Asia/Shanghai 每日日报（可为 0 条） | 同日重跑先与旧日报合并，再整份重写 |
| `data/checkpoints.json` | 源检查指针：source、last_success_at、last_failure_reason、consecutive_failures、last_snapshot_path | 不保存增量标记；增量来自 sitemap 快照比较 |
| `data/sitemaps/<domain>.txt` | 该域名最新 URL 清单 | 成功生成新快照后再替换 |
| `data/sitemaps/archive/<domain>/<timestamp>.txt` | 旧快照 | 每域名最多保留最近 **12 份**，清理时仅删最旧归档，不动当前快照 |
| `inputs/gsc-ga4/` | 用户手动投递授权的 GSC/GA4 聚合 CSV | **严格只读：自动任务不得创建、修改、删除任何文件**；不存在即 blocked |

**写入顺序与异常**：先读取并计算合并后的候选快照、历史事件、当日报告，再逐项写入 GitHub。每个写操作须获取当前 SHA；409/冲突时重新获取并合并，禁止强制覆盖。GitHub 文件 API 不能原子提交多个文件；中途失败需明确标记 partial、下次以 ID + 事件键去重修复，绝不能谎称全成功。events 只追加逻辑记录，实际 GitHub 更新依旧可能需要整份文件重写，避免“追加 API”幻觉。sitemap 归档清理前确认最新快照成功且归档写入成功。

**同日重跑的 3 条上限**：先合并当天旧/新候选，证据 URL 去重，再按「证据可靠度 → 标准化得分率 → 覆盖维度」重新排序，重新选最多 **3 条合格推荐**；旧榜被挤出的记录保留原 status、只将 `recommended_today=false`，报告底部写「本次重跑替换 X 条」。不得仅追加在前三条后面。日报头必须包含 **`日期基准：Asia/Shanghai（UTC+8）`**。

**失败与人工介入**：checkpoints 的 `consecutive_failures` 成功后归零，失败时 +1；连续失败 **≥5 次**在日报显著标注「建议转人工」，但不凭失败造出增量。无合格候选时也写包含 0 条的报告，不因空报告停止。

**周日周报**：只读 history.jsonl 在 Asia/Shanghai 本周的记录，必要时读取 opportunities 当前快照定位候选；不读取一周 reports 全文；开头注明「基于候选状态变更事件，非逐日运行报告回顾」。如果历史文件首次尚无事件，写暂无变更，不能由当前状态反推出不存在的历史。

## 可直接给自动化调用的提示词（待审核）

> 每轮从 https://github.com/owenlee669/reqs-geifei main 读取最新 docs/task-sop.md 和 docs/methods.md，严格按 SOP 执行；以 Asia/Shanghai 判定日期与周轮换。自动检查公开来源、实查可访问 SERP，不伪造 volume/KD/CPC。按规范化 query+意图+固定场景生成稳定 ID，Signal 不评分；Candidate 得分率≥60、至少3/5维且商业或 SEO 有真实可核查依据时才纳入最多3条的日报；Validated 必须有两条独立需求证据、商业化证据及 SERP/MVP 检查。M12 仅在周六固定组合完成且推荐未满3条时补一条。维护 opportunities 当前快照、history 追加事件、checkpoints 指针与 sitemap 快照；同日重跑合并重新排序后重写日报，保留被挤出者原始状态；周日仅读 history 本周变化生成周报。inputs/gsc-ga4/ 严格只读。无合格机会写0、不可访问数据写 unknown/blocked，连续5次失败建议人工。写库失败报告详情，不假称成功。

## 需用户最终确认（不影响本次文档提交）
实际运行时间和频率；最终推送渠道（默认只写 GitHub 仓库文件）；第三方数据工具授权范围（默认仅公开免费数据）；与现有需求扫描任务的边界。**已确定：零合格机会也写含 0 条日报**。日期基准暂按本次 Issue 指定的 `Asia/Shanghai` 写入 SOP，正式启用前需核对任务时区设置。

**仅文档草案，尚未激活任务。**
