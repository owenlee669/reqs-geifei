# 需求挖掘方法合并与原始证据

更新：2026-11-13（Issue #1 第四轮复审，配合 SOP v0.5）。来源：仓库已有的三篇 Web.Cafe 剪存 Markdown（见文末）。它们是**方法分享和作者自述**，不是我们已实测验证的市场机会。归并相同动作，不因平台名称不同就重复执行。

## 统一流程

**信号 → 用户痛点及动作 → 1–3 个自然英文搜索词 → 独立证据 → SERP/关键词验证 → 商业化验证 → 一周 MVP → 去重入库。**

**规则唯一来源**：状态定义、升级门槛、去重键与评分口径统一见 [task-sop.md §状态与评分](./task-sop.md#状态与评分分阶段缺失数据不扣零分)。本文件只定义发现方法、证据类型与操作步骤，不重复制定状态或去重规则。点赞、Star、Trends、sitemap 新页只是线索，不能独立证明付费需求。

**优先级标签的含义**：P0=每轮发现、验证或排序时优先使用的核心方法；P1=按固定周轮换或授权数据条件执行的补充方法；P2=仅满足 SOP 指定触发条件时执行的延展方法。**标签不代表每周运行次数**；固定频率与 M12 触发条件以 [task-sop.md §固定周轮换表](./task-sop.md#固定周轮换表) 为准，不能依据 P0/P1 自行改轮换。

## 12 条归并后的可操作方法

### M01 新产品榜单 / 增长站反查非品牌关键词（P0）
**入口（自动侧）**：Product Hunt 每日新品、Toolify 公开新品/增长榜（不读取需授权的收入数值）、Traffic.cv 公开页面、BetaList、V2EX 分享创造、Indie Hackers 公开产品/收入案例、[Google Play 付费应用榜入口](https://play.google.com/store/apps)、[Apple App Store 榜单](https://apps.apple.com/us/iphone/charts)。**人工侧**：Toolify 收入榜、TrustMRR 或其他需会员/登录的收入与流量指标，未经授权统一记 unknown，不用来筛掉自动发现的新产品。付费榜是支付意图与市场存在的线索，不证明具体收入，需记录地域、榜单日期与类别。  
**步骤**：读取公开榜单及项目主页 URL → 产品/需求证据去重 → 核查用户动作、公开功能与收费模式 → 可获取合法授权时才使用 Similarweb/Semrush/Ahrefs 的流量、落地页与来源地区，否则填 unknown 并交人工 → 排除纯品牌词 → 提取 1–3 个自然英文需求词 → 实查公开 Google SERP/竞争网站。付费 App 同时查看低星评论、具体功能和是否存在网页版需求。  
**原文案例（仅供人工复核，自动任务不得作为硬过滤条件）**：benny 建议 Product Hunt API 获取前一天产品，再检查「域龄 <1 年、月流量 >3,000、自然搜索占比 >20%、直接访问占比 >20%」，称约每 300 个 PH 产品筛出一个。以上是作者个人经验，均需有对应时间和来源数据才能应用。自动扫描默认只用公开可访问的产品发布日期、功能、定价、用户反馈与页面证据入库；未授权的 Similarweb 估计流量与渠道占比一律记 `unknown`，不臆测域龄或流量，也不因缺数据淘汰新站。  
**原始依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)；[网站和页面](https://new.web.cafe/ask/bounty/0jch5yv6g7)。

### M02 头部/竞品 sitemap 增量，反推新词（P0）
**入口**：公开 sitemap.xml / sitemap index、工具目录分类页、竞品新功能页。  
**步骤**：首次 URL 快照保存为 `data/sitemaps/<domain>.txt`（域名转小写且去除不安全路径字符）；再次扫描读取旧快照，先生成新快照并确认成功，再将旧版归档至 `data/sitemaps/archive/<domain>/<YYYY-MM-DDTHHMMSS>.txt`，归档成功后更新当前快照；**每域名归档只保留最近 12 份**，其余清理，清理失败要记录 → 比对规范化 URL 差集（不单凭 lastmod）→ 排除参数、语言/分页/模板重复 → 提取新增 URL 的 title/H1/正文用户动作 → 生成英文关键词并做 SERP 验证。`data/opportunities.jsonl` 仅保存相关新增 URL 摘要、数量和来源，不存整份 sitemap。**首次扫描只有基线，不得宣称新增**；访问失败保留旧快照，标记 blocked，不能覆盖为空。  
**限制**：大站上新页面是“投资布局信号”，不是已证明流量/收入。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)；[验证方法](https://new.web.cafe/ask/bounty/k3ivgzypq1)。

### M03 词根扩展 + 站找词/词找站 + SERP 实查（P0）
**入口**：Similarweb 关键词生成器、Semrush Keyword Magic/Gap、Ahrefs Top Pages/Content Gap、Google 联想/PAA/相关搜索、Keyword Planner、哥飞 KD。  
**步骤**：从产品功能或用户原话提词根 → 扩展长尾与不同语言的概念词根 → 归一化、去重，排除错意图品牌词 → 查搜索量/KD/CPC/趋势（有数据才填写）→ 实查 SERP 的结果类型、独立站、低权重站、搜索意图匹配度 → 形成候选。  
**工具型 SERP 供给缺口**：查询明确要求计算、生成、转换、批量处理或下载，但公开可核验的前列结果主要是文章、问答、视频且缺少实际工具页面，记为 potential-tool-gap；保存搜索地区、日期和实际看到的 URL。只有核验完整前十才能称「前十无工具页」，否则表述为「可见结果未找到」。这是候选信号，不单独构成已验证机会。  
**原文案例**：程序员卡诺称将 119 个词根扩成 1,309，再扩展 9 万余关键词，二轮筛到 800 余条；原文另有「月搜索 3 万、KD<60」但本次剪存段落无法可靠将该阈值对应到确切作者，故不作为可执行数值门槛，待人工核对作者与出处后再引用。这两者都是个人筛法，不能作为默认硬门槛；小站更看重 SERP、低竞争、开发成本。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)；[验证方法](https://new.web.cafe/ask/bounty/k3ivgzypq1)。

### M04 付款/收入/会员付费墙，反推强需求（P0）
**入口（自动/人工分工）**：自动侧只查公开可访问的竞品定价页、免费额度及用量限制、可见 App 内购价格和 Indie Hackers 等公开收入分享原帖；Stripe、Creem、Lemon Squeezy 的收银台/引荐数据、Toolify 收入榜、TrustMRR 或需会员/登录的数值只在合法授权可获取时使用，否则填 `unknown`，列入人工待核，不因缺失而淘汰候选。WPS 等大厂会员功能及竞品付费墙可作为公开功能对比入口。  
**步骤**：发现具体网站 → 确认其细分用户与付费功能 → 记录公开价格/免费次数/限制/升级时机 → 查可核验的付费证据/公开披露 → 抽取可拆分独立功能 → 查英文关键词和竞争。  
**收银台到达率（仅有可核对数据才计算，默认人工）**：到达率 = 支付页访问次数 ÷ 网站访问次数 × 100%。原文示例为 269.26 万 ÷ 3131.87 万 ≈ 8.60%（作者自述，未经独立复核）。只有同时间段、同统计口径的数据才计算，并附分子、分母、日期、数据来源及估计属性；不能统一设「高于 X% 就赚钱」门槛。它只是支付意图代理指标，不等于成功支付率或收入。  
**竞品付费墙所在步骤**：观察上传前、点击生成时、部分结果解锁、用量耗尽、第二次使用、特定高级功能等收费节点；反映竞品尝试变现的位置，不代表已经证明最优。  
**注意**：广告持续投放≠ROI>1；第三方收入是估算；定价页和收银台跳转不等于成交。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)。

### M05 软件差评/替代品，寻找已验证市场中的缺口（P0）
**入口**：G2、Capterra、Trustpilot、Google Play、App Store、Chrome Web Store、Shopify App Store、WordPress 插件、AlternativeTo、Reddit、YouTube 测评区。  
**步骤**：选有真实用户的成熟产品 → 查近期 1–3 星及中评/Cons/功能请求 → 保留具体评论 URL、日期、原话和场景 → 聚类「贵、慢、批量不足、兼容失败、隐私、缺功能」 → 不同作者/不同平台寻找重复证据 → 查有没有已付费替代产品 → 形成单一功能 MVP。  
**原文提示**：安装量较大但评分 3.5–4.3 的 Chrome 扩展作为初筛；不是硬标准。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)；[网站和页面](https://new.web.cafe/ask/bounty/0jch5yv6g7)。

### M06 社区抱怨/求推荐/重复工作（P0）
**入口**：Reddit 垂直版块、HN Ask/Show、GitHub Issues/Discussions、Quora、V2EX、YouTube 教程评论。  
**步骤**：搜索英文句式 `is there a tool that` / `I wish there was` / `alternative to` / `too expensive` / `how do I` / `bulk` → 过滤推广、复制内容 → 抽取「谁在何种场景需要完成什么动作」 → 找实际替代方式/预算/重复频率 → 跨独立来源交叉印证 → 转化为自然英文查询。  
**限制**：抱怨数量和点赞不直接等于付费。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)；[网站和页面](https://new.web.cafe/ask/bounty/0jch5yv6g7)。

### M07 人工付费服务/笨重流程 → 轻量工具（P1）
**入口**：Fiverr、Upwork、Freelancer、闲鱼/淘宝代做服务、招聘 JD、复杂 Excel、20–30 步教程、多个工具手工拼接。  
**步骤（周六固定监控）**：检索可公开访问的 Fiverr、Upwork、Freelancer 的真实服务/任务 → 保存具体任务链接、日期、报价币种与范围、是否已成交的可核实信息（只见报价不等于成交）→ 将重复人工任务拆分输入-处理-输出 → 判断现成 API/开源/轻代码的可行性 → 查英语地区搜索意图 → 做单页/人工交付验证。登录墙、反爬或报价不可见时标记 blocked，不臆测订单。  
**限制**：中国市场订单不能直接证明英语国家购买。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)。

### M08 Google Trends + 新模型/新游戏/短视频玩法（P1）
**入口**：Google Trends 7/30/90 天、Hugging Face Spaces、Replicate、YouTube Shorts、TikTok、SteamDB、游戏热榜。  
**步骤**：发现新名称/动作 → 标注首次出现与地区 → 查趋势变化和社媒原始评论 → 提取 `online`、`browser`、`template`、`how to` 等真实需求组合 → SERP 查供给缺口 → 评估版权、商标与 API 能力。  
**H5 游戏专项**：观察 SteamDB、视频平台、社区与小游戏站的新游热度，从实际用户提问核查 play online / browser / controls / wiki / save editor 等词，并验证英文 SERP 和合法授权路径；不复制未经许可的游戏内容。  
**注意**：Trends 的 0–100 是相对热度，不是搜索量。  
**依据**：[网站和页面](https://new.web.cafe/ask/bounty/0jch5yv6g7)；[验证方法](https://new.web.cafe/ask/bounty/k3ivgzypq1)。

### M09 GitHub 新项目及 SKILL.md 反推可产品化任务（P1）
**入口**：GitHub Trending、新增仓库、Issues、Discussions、低 Star 新 Skill、X 上的 skill 分享。  
**自动步骤**：只依赖无需登录即可访问的 GitHub 公开仓库搜索（创建/更新时间、topic、仓库描述）、Trending、公开 README、Issues 与 Discussions → 识别 Skill 或具体自动化任务的配置难题和用户请求 → 判断网页化/批量化/API 化机会 → 关键词及付费证据核验。**人工补充**：`filename:SKILL.md`、`path:skills` 等代码搜索若要求账号登录或当前执行环境无法调用，标记 `blocked: GitHub code search requires authorization`，由人工授权查询或提交结果；不能声称自动检索过代码，也不得臆测未读取文件的内容。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)。

### M10 平台子域名和导航站出站流量（P1）
**入口**：`vercel.app`、`pages.dev`、`github.io` 子域名；导航站出站链接；Similarweb 落地页。  
**步骤**：找近期起量的公开子站 → 检查是否为真实项目而非预览/演示 → 合并同项目不同部署 → 找其自然搜索页、非品牌词 → SERP/竞争/商用验证。  
**依据**：[验证方法](https://new.web.cafe/ask/bounty/k3ivgzypq1)。

### M11 自有 GSC 查询 × GA4 页面指标（P1）
**入口与数据投递**：只能读取用户授权放置于仓库 `inputs/gsc-ga4/` 的两类 CSV：`gsc_queries_pages_YYYY-MM-DD.csv`（至少 date、query、page、clicks、impressions、ctr、position）与 `ga4_landing_pages_YYYY-MM-DD.csv`（至少 date、landing_page、sessions、engagement_rate、average_engagement_time）；记录对应站点、属性、时区、起止日期和统计粒度。没有导出文件、缺字段或未经授权：**跳过 M11，记 `blocked: missing/invalid user exports`**。该仓库是公开的，只放聚合报表，严禁个人数据或密钥。  
**步骤与规范化**：导出近 28 天查询-落地页数据 → 分别转换为统一的页面关联键（从完整 URL 取 hostname+pathname；路径字段补齐对应站点域名；去协议、去 `www.`、去尾斜杠、去查询参数，常规路径转小写；如果目标站存在大小写敏感路由，先按实际 canonical 处理，不强行合并）→ 统一站点属性和时区，取两份报表**实际可比的日期交集**并记录数据新鲜度（GSC 常有约 2 天延迟，不能机械固定两天）→ 按页面汇总后关联，报告匹配页数、未匹配页数与占比 → 结合高曝光低 CTR、排名约 8–20、页面参与度差距分析，决定改页面或增工具。**GA4 页面级参与度不能归因到某一条 GSC 搜索词**；维度和聚合粒度不一致时只给页面关联观察，禁止宣称该 query 造成低停留。  
**原文建议**：「点击>10 且停留<30秒」仅是作者的示例阈值；低参与不必然等于需求。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)。

### M12 成功需求横向细分/延展（P2）
**步骤**：找到明确已有需求的单点 → 从人群、输入、输出格式、语言、工作流程和专业场景衍生变体 → 每个新变体独立核验词、SERP 与付费线索 → 只做用户动作清晰的最小功能。  
**原文案例**：作者从 AI headshot、hair 方向扩展身体局部相关生成需求；属于思路示例，不代表所有衍生关键词可行。  
**依据**：[私藏方法](https://new.web.cafe/ask/bounty/fd0wrgx7fh)。

## 中文平台与人工补数据

小红书、抖音、TikTok、闲鱼、招聘 JD 平台，以及受登录/反爬限制的 YouTube 评论：任务不绕过登录、验证码和访问限制；只接收用户授权提供的截图、文字、链接或导出数据，再做聚类并标记「用户提供，未独立核实」。公开可正常访问的 YouTube 评论仍可以查询。

- 小红书：搜索下拉词、相关笔记数和头部赞藏、评论区「求工具」；截屏记录时间。
- 抖音：同主题多条视频播放、评论「求链接」「怎么做」，不能将播放当销量。
- TikTok：同抖音口径，仅接收用户授权提交的截图、合法导出或可访问链接；标记「用户提供、未独立核实」，不得绕登录与反爬。
- 闲鱼：同类商品数与「想要」数、已显示订单/报价；不能直接外推英文市场。
- 招聘 JD：重复岗位和工作职责，记录公司、职位、发布日期及链接。
- YouTube：验证中文需求翻译成自然英文后的教程/视频评论，优先直链，否则记录人工提供材料。

人工线索先记 Signal，仍需在英文市场验证。

## 证据类型与独立性判断

- Product Hunt/Toolify/BetaList/付费 App 榜属于**发现渠道**，同一产品被多榜单采录仍是同一信号，不构成独立用户证据。
- Semrush/Similarweb/Ahrefs 和 Google SERP 主要是**验证工具**，不是又一个独立用户抱怨。
- Reddit、G2、评论、Issue 是**原始需求证据**；同一用户跨平台复制不算多源。
- Stripe/定价/付费墙是**商业化信号**，不能拿来冒充交易记录。
- sitemap 与趋势属于**领先信号**，需要后续交叉验证。
- 仅凭一个截图、一个案例或文章作者说「赚到钱」，不得升级为已验证机会。
- 目标优先适合独立开发者：英文 SEO、单页工具、小 SaaS、H5 游戏相关小需求、低成本一周 MVP。
- 不通过平台授权的付费 API/私有接口逆向采集；遵守平台服务条款。

## 原始资料索引（完整文件均原样保留）

1. `你有哪些私藏的挖掘需求的好方法 - 悬赏专区 _ Web.Cafe/你有哪些私藏的挖掘需求的好方法 - 悬赏专区 _ Web.Cafe.md` — [原始网页](https://new.web.cafe/ask/bounty/fd0wrgx7fh)
2. `你都在哪些网站挖掘需求？主要看这个网站的哪些地方？ - 悬赏专区 _ Web.Cafe/你都在哪些网站挖掘需求？主要看这个网站的哪些地方？ - 悬赏专区 _ Web.Cafe.md` — [原始网页](https://new.web.cafe/ask/bounty/0jch5yv6g7)
3. `被实际验证过的找需求（关键词）的方法到底有哪些 - 悬赏专区 _ Web.Cafe/被实际验证过的找需求（关键词）的方法到底有哪些 - 悬赏专区 _ Web.Cafe.md` — [原始网页](https://new.web.cafe/ask/bounty/k3ivgzypq1)

这三篇文章中“验证过”指作者/参与者自述，本文并未独立验证文中收入、流量或转化数字。
