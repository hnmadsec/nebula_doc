# 架构设计

`TH-Nebula`是一个互联网风控分析和检测平台, 可以对包括流量攻击, 账户攻击, 支付攻击在内的多种业务风险场景进行细致的分析, 找出威胁流量, 帮助用户减少损失.

与常见的一些简易安全防护软件不同, `TH-Nebula`本质上应该是一套完整且独立的数据分析平台, 逻辑上, 它需要提供以下几个方面的功能:

- 数据采集与集成平台. 负责对接客户现有系统不同形式存在的各种原始数据, 包括流量, 实时日志, 日志文件等.

- 数据规整化与业务日志提取系统. `TH-Nebula`对原始数据进行清洗和标准转换, 并根据配置抽象出各种标准的业务日志, 方便后续进一步的分析.

- 数据持久化功能.对于进入系统的日志, 进行持久化, 方便后续的离线计算以及攻击溯源操作.

- 海量数据实时计算引擎. 对进入系统的海量数据, 进行大规模实时并行计算, 得到关于用户的实时统计特征

- 海量数据离线批处理计算引擎. 对进入系统的海量数据, 间隔性的进行离线批处理计算, 得到关于用户的固定特征

- 高性能策略引擎. 利用实时计算和离线计算的数据, 对所有用户访问进行策略判别, 识别出风险流量, 方便后续进一步处理

- 风险事件和黑白名单管理功能. 对于系统中识别出的风险事件, 以及与之相关的黑白名单进行管理和查询

- 数据可视化和风险数据自助式分析系统. 方便对原始数据进行`review`, 对风险情况进行溯源

- 数据导出和`API`集成. 用于将黑白名单和风险事件导出, 集成到客户系统; 同时可以进一步将系统内部数据导出.

- 系统配置和管理功能. 复杂的系统需要配合相应的管理工具.

系统粗略的架构设计如下图所示: 

![24.TH-Nebula架构](http://www.z4a.net/images/2018/11/28/24.png)


整个`TH-Nebula`系统, 功能比较完整和复杂, 无法用单个进程或软件的形态来提供这样一整套平台.在物理实现上, 将由多个独立的组件组成, 纯业务模块包括:

- 数据采集和转化模块. 数据采集和规整化由单个物理模块提供.

- 数据实时计算模块和规则引擎. 提供了系统中实时处理的功能, 包括实时计算, 准实时计算, 策略引擎等.为了简化, 目前统一在实时模块.

- 数据离线计算模块. 离线计算处于负责离线数据的统计计算, 数据呈现以及数据持久化等.

- 系统配置和管理模块. 配置和所有的数据管理都有单独的web应用负责.

- `TH-Nebula`前端展现模块. `TH-Nebula`的前端采用`JS`+`API`的模式, 大量的数据展现功能由前端模块来提供支撑.

当然, 系统还用到了许多底层的平台支撑:

- 系统缓存``Redis``.``Redis``提供了缓存数据的支撑, 主要包括消息中间件和监控数据的存储.

- 文件系统. 通过文件数据库, 可以提供海量数据的存储和查询.

- 数据存储`MySQL`. `MySQL`提供了所有具备强持久化需求的数据落地和读取.

- 用户画像辅助`Key-Value`数据库`AeroSpike`.`AeroSpike`是一个`Key-Value`数据库, 为用户画像数据的高性能存取提供了支撑.

- 其他. 包括负载均衡``Nginx``, 离线管理脚本, 进程监控平台, 定制内核模块等多个其他功能.

下图描述了系统的物理模块组成, 以及逻辑模块在其中的划分

![25.TH-Nebula模块组成](http://www.z4a.net/images/2018/11/28/25.png)