# 构建生产级AI应用

到目前为止，我们已经探讨了构建AI应用时所有重要的概念。本课程的这一部分将专注于在真实生产环境中构建AI应用所需的要素和一些最佳实践。

## 1. 护栏 (Guardrails): 保护你的AI系统

我们之前已经解释过，你的AI应用可能会受到使用AI的恶意攻击，例如提示注入或越狱。你的用户可能会暴露你不想泄露的私人信息。值得注意的是，许多第三方API（如OpenAI）为你提供了许多开箱即用的护栏。

### 输入护栏：第一道防线

这些护栏可以防止私人信息泄露给外部API，并阻止可能危及你系统的恶意提示。有许多工具可以自动检测你指定的敏感数据（通常使用AI），你可以阻止这些数据被发送到模型。

**真实案例 - 医疗保健平台：** 一个医疗AI助手需要防止受保护的健康信息（PHI）被发送到外部LLM API：

**实现方式：**
-   构建自定义的命名实体识别（NER）模型来检测患者姓名、社保号码、病历号。
-   使用正则表达式模式来匹配电话号码和地址等结构化数据。
-   实施关键词屏蔽，用于屏蔽药物名称和诊断代码。
-   设置了延迟要求为50毫秒的实时扫描。

**检测规则：**
-   社会安全号码：`\d{3}-\d{2}-\d{4}` 模式匹配。
-   患者ID：针对医院特定的编号格式进行自定义格式识别。
-   医学术语：屏蔽了2,847个特定的药物名称和1,203个诊断代码。
-   个人标识符：与患者数据库交叉引用姓名。

**结果：** 在第一个月内防止了234次PHI泄露，降低了合规风险，使得在保持HIPAA合规的同时能够使用外部AI。

**常见的输入护栏类别：**
-   **个人身份信息（PII）检测：** 姓名、地址、电话号码、电子邮件地址、社保号码。
-   **财务数据：** 信用卡号、账号、路由号码。
-   **公司机密：** 产品名称、财务数据、战略计划。
-   **提示注入：** 试图覆盖系统指令。
-   **恶意内容：** 试图生成有害或非法内容。

### 输出护栏：AI响应的质量控制

这是为了捕捉不符合你标准的输出。比如模型返回不当信息，甚至是仇恨或非法信息。需要注意的是，输出护栏在流式完成模式下可能效果不佳。

**真实案例 - 客户服务AI：** 一家电信公司为其客户服务聊天机器人实施了输出过滤：

**多层过滤：**
1.  **内容安全：** 使用Azure内容安全API检测仇恨言论、暴力、自残内容。
2.  **品牌合规：** 在10,000个适当与不当响应的示例上训练的自定义分类器。
3.  **准确性验证：** RAG引用检查器，确保响应基于知识库。
4.  **语气分析：** 情感分析，确保响应保持专业、有帮助的语气。

**实现细节：**
-   平均延迟80毫秒的实时处理。
-   置信度阈值：屏蔽毒性得分>70%的响应。
-   回退响应：当AI输出被屏蔽时，使用预先编写的安全响应。
-   人工上报：标记多次响应被屏蔽的对话。

**结果：**
-   不当响应减少了94%。
-   客户满意度从3.2分提高到4.1分（满分5分）。
-   防止了潜在的品牌损害事件。
-   估计节省了120万美元的声誉管理成本。

**流式传输挑战：** 输出护栏在流式响应中更难实现，因为你无法在发送完整响应之前对其进行分析。解决方案包括：
-   句子级过滤（在生成每个句子时进行检查）。
-   滑动窗口分析（连续分析最后N个令牌）。
-   保守设置（为流式传输设置更高的屏蔽阈值）。

## 2. 模型路由器和网关：智能流量管理

### 路由器：为正确的任务选择正确的模型

这是AI应用中的一种常见模式。你不是为所有查询都使用一个模型，而是使用不同的模型。在实践中，根据我的经验，大多数AI应用会为不同的任务使用不同的模型。这可以帮助节省成本，因为你可以为更简单的查询使用更便宜的模型。

路由器包含预测用户意图的逻辑，并根据该意图将查询路由到适当的模型（这通常也使用AI）。有一些开箱即用的路由器，或者你可以用一个小型语言模型构建自己的路由器。

**真实案例 - 电子商务平台：** 一家在线零售商为其客户查询构建了一个复杂的路由系统：

**意图分类：**
-   **简单问题**（占查询的43%）：“我的订单状态是什么？” → 路由到快速、廉价的模型（GPT-3.5）。
-   **产品推荐**（占查询的31%）：“给我找一台适合玩游戏的笔记本电脑” → 路由到专门的RAG系统。
-   **复杂支持**（占查询的18%）：“我需要退货，但网站不让我退” → 路由到高级模型（GPT-4）+ 人工上报。
-   **技术问题**（占查询的8%）：“我该如何设置这个路由器？” → 路由到技术文档模型。

**路由器实现：**
-   使用500参数的模型构建了轻量级分类器（每次查询的推理成本：0.0001美元）。
-   训练数据：来自支持工单的50,000个带标签的客户查询。
-   延迟25毫秒的实时预测。
-   置信度阈值：如果分类置信度<85%，则路由到高级模型。

**成本影响：**
-   路由前：平均每次查询0.012美元（所有查询都发送到GPT-4）。
-   路由后：平均每次查询0.004美元（成本降低67%）。
-   每月节省：89,000美元，响应质量相同或更好。
-   由于简单查询的响应速度更快，客户满意度得到提高。

**高级路由策略：**
-   **基于负载的路由：** 根据当前API负载将请求路由到不同的模型。
-   **基于用户级别的路由：** 高级客户获得更好的模型。
-   **上下文感知路由：** 根据对话历史和复杂性进行路由。
-   **A/B测试集成：** 将一定比例的流量路由到实验性模型。

### 网关：统一模型管理

这允许你以安全的方式连接到不同的模型。它是不同模型的一个统一接口，使你的代码易于维护。你通常可以在网关级别应用节流、跟踪使用情况等。

**真实案例 - 金融服务公司：** 一家投资公司构建了一个模型网关来管理其组织内的12个不同的AI模型：

**网关架构：**
-   **单一API端点：** 所有应用程序都连接到一个URL，而不管底层模型是什么。
-   **认证：** 使用带有基于角色的模型访问权限的JWT令牌。
-   **速率限制：** 为不同的用户级别和模型设置不同的限制。
-   **模型抽象：** 相同的请求格式适用于OpenAI、Anthropic、Google和内部模型。

**实现的功能：**
-   **自动故障转移：** 如果主模型宕机，自动路由到备用模型。
-   **使用情况跟踪：** 按部门、用户和模型跟踪成本。
-   **请求日志记录：** 存储所有请求/响应以供审计和训练。
-   **模型版本控制：** 支持同一模型的多个版本，并进行逐步推广。

**业务影响：**
-   **开发人员生产力：** 新模型的集成时间从2周减少到2小时。
-   **成本管理：** 集中计费和跨团队的使用分析。
-   **合规性：** 审计日志和安全控制的单点。
-   **可靠性：** 自动故障转移使正常运行时间达到99.7%（而直接API调用为94%）。

**技术实现：**
```python
# 示例网关请求 - 无论模型如何，格式都相同
POST /gateway/v1/chat/completions
{
    "model": "best-for-finance",  # 网关解析为实际模型
    "messages": [...],
    "user_tier": "premium",       # 影响路由和速率限制
    "department": "trading"       # 用于成本跟踪
}
```

## 3. 缓存：速度和成本优化

缓存在软件开发中并不新鲜。它一直被用来减少延迟和成本（以避免频繁的数据库查询）。在AI领域，有不同的缓存技术。

### 精确缓存：完美匹配

例如，如果用户要求模型总结一个产品，系统会检查缓存中是否存在该确切产品的摘要。这用于避免频繁的向量搜索。这可以使用像Redis这样的内存存储来实现。

**真实案例 - 新闻聚合平台：** 一个新闻网站为文章摘要实现了精确缓存：

**实现方式：**
-   **缓存键：** 文章URL的SHA-256哈希 + 摘要长度参数。
-   **存储：** Redis集群，TTL为72小时（新闻很快会过时）。
-   **缓存大小：** 500GB，存储约200万篇文章摘要。
-   **命中率：** 67%的摘要请求由缓存提供。

**性能影响：**
-   **延迟：** 缓存命中响应时间为15毫秒，而AI生成需要2.3秒。
-   **成本节省：** 每月节省23,000美元的LLM API调用费用。
-   **用户体验：** 热门文章即时加载。
-   **可扩展性：** 在重大新闻事件期间处理了10倍的流量高峰。

**缓存策略：**
-   对热门文章进行主动缓存（在用户请求前缓存）。
-   智能驱逐：移除浏览量<10的文章摘要。
-   地理分布：在多个地区缓存热门文章。

### 语义缓存：相似意图匹配

即使查询不完全相同，但语义上相似，也会使用缓存项。这需要一个向量数据库来存储缓存查询的嵌入，因此更复杂且计算密集。速度和成本可能不值得。

**真实案例 - 法律研究平台：** 一个法律AI助手为案例法研究实现了语义缓存：

**挑战：** 律师经常以不同的方式提出类似的问题：
-   “加州有哪些关于违约的先例？”
-   “给我看加州关于合同违规的案例法”
-   “查找加州法院的违约案例”

**实现方式：**
-   **向量存储：** Pinecone数据库，存储查询嵌入。
-   **相似度阈值：** 如果余弦相似度>0.85，则缓存命中。
-   **嵌入模型：** text-embedding-ada-002，用于一致的表示。
-   **缓存评分：** 按查询热度和新近度加权。

**性能分析：**
-   **缓存命中率：** 34%的查询与语义相似的先前查询匹配。
-   **每次命中成本：** 0.15美元（嵌入查找+向量搜索）。
-   **每次未命中成本：** 3.20美元（使用RAG进行完整的法律研究）。
-   **盈亏平衡点：** 语义缓存在命中率>4.6%时盈利。

**结果：**
-   缓存命中的平均响应时间从8.2秒减少到1.7秒。
-   总体计算成本降低了31%。
-   通过更快的搜索结果改善了用户体验。
-   挑战：当法律先例发生变化时，缓存失效变得复杂。

**何时使用语义缓存：**
-   高成本的AI操作（复杂的RAG、长文档）。
-   重复的用户模式（客户支持、研究）。
-   稳定的知识领域（法律、医疗、技术文档）。

**何时避免语义缓存：**
-   低成本的AI操作（简单的聊天补全）。
-   高度动态的内容（实时数据、突发新闻）。
-   隐私敏感的查询（每个用户都需要新的结果）。

## 4. 可观察性：监控AI性能

同样，可观察性是整个软件工程领域的通用实践。具体到AI，通常是每次请求的输入和输出令牌数、格式失败（如果你期望JSON输出，跟踪模型返回无效JSON的频率）。对于开放式生成，可以考虑简洁性、创造性或积极性等指标——许多这些指标可以使用AI裁判来计算。

### 核心AI指标

**真实案例 - SaaS平台：** 一个带有AI功能的项目管理工具跟踪全面的指标：

**令牌和成本指标：**
-   **每次请求的输入令牌：** 平均342个，95百分位为1,247个。
-   **每次请求的输出令牌：** 平均156个，最大上限为500个。
-   **每次请求的成本：** 平均0.0034美元，环比下降12%。
-   **模型利用率：** GPT-4：23%，GPT-3.5：61%，Claude：16%。

**质量指标：**
-   **格式合规性：** 94.3%的JSON响应是有效的（目标：>95%）。
-   **响应相关性：** AI裁判对响应进行1-5分评分，平均4.2分。
-   **引用准确性：** 87%的RAG响应包含有效的引用。
-   **幻觉率：** 3.1%的响应包含无根据的说法。

### 高级可观察性模式

**用户行为分析：**
-   **对话长度：** 平均3.4轮，15%的用户有>10轮的对话。
-   **提前终止：** 8%的用户在响应中途停止生成（表明质量差）。
-   **重试率：** 12%的用户重新生成响应（质量指标）。
-   **功能采用率：** 67%的活跃用户使用AI功能。

**性能监控：**
-   **端到端延迟：** P50：1.2秒，P95：4.7秒，P99：12.3秒。
-   **模型延迟：** 与RAG检索和护栏处理分开跟踪。
-   **错误率：** 模型API错误（2.1%），超时错误（0.8%），护栏屏蔽（1.4%）。
-   **可用性：** 所有AI功能的正常运行时间为99.2%。

**业务影响指标：**
-   **用户参与度：** AI用户的留存率比非AI用户高34%。
-   **支持工单减少：** AI功能用户的支持工单减少了23%。
-   **功能满意度：** AI功能的NPS评分为67（而整体产品NPS为45）。
-   **收入影响：** AI功能使付费计划的转化率提高了18%。

### RAG特定的可观察性

**真实案例 - 知识管理平台：** 一个带有RAG功能的企业搜索工具：

**检索质量：**
-   **检索延迟：** 向量搜索：45毫秒，语义重排：120毫秒。
-   **块相关性：** 人工标注员对前3个块进行评分，相关性得分为78%。
-   **覆盖率分析：** 23%的查询需要来自多个来源的信息。
-   **索引新鲜度：** 跟踪搜索结果中的数据年龄（平均4.2天）。

**答案质量：**
-   **引用率：** 91%的响应至少包含一个引用。
-   **引用准确性：** 83%的引用实际上支持答案。
-   **完整性：** AI裁判对答案完整性进行评分，平均4.1/5。
-   **信息冲突：** 当来源相互矛盾时进行标记（占查询的7%）。

### 实现工具和平台

**监控堆栈：**
-   **应用指标：** 使用DataDog进行延迟、吞吐量、错误率的监控。
-   **AI特定指标：** 使用Weights & Biases进行模型性能跟踪。
-   **用户分析：** 使用Mixpanel进行用户行为和功能采用率的分析。
-   **成本跟踪：** 自定义仪表板，汇总所有模型提供商的使用情况。

**警报配置：**
-   **即时警报：** 错误率>5%，延迟>10秒，护栏屏蔽>20%。
-   **每日警报：** 成本飙升>30%，质量得分下降>10%。
-   **每周报告：** 使用趋势、成本优化机会、质量改进。

**仪表板设计：**
-   **高管仪表板：** 高级指标、成本趋势、业务影响。
-   **工程仪表板：** 技术指标、错误分析、性能优化。
-   **产品仪表板：** 用户行为、功能采用率、质量趋势。

成功的AI可观察性的关键在于平衡技术指标与业务成果。跟踪对你的特定用例重要的内容，并始终将AI性能与用户满意度和业务结果联系起来。
