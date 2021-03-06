## 2021

### 2021-02

**红谷滩智慧政务**

- 02.19 完成官网百度收录及首页加载优化，智慧政务-拆分主页为多个子页面
- 02.20 新增视频求助页面，优化菜单栏功能，对接数据栏接口
- 02.22 开发视频求助页面白板功能
- 02.23 联调 web 端与小程序的白板功能
- 02.24 对接手写签名接口，优化白板布局
- 02.25 智慧政务添加小图标，页面排版
- 02.26 调试智慧政务项目，解决出现的问题

### 2021-03

- 03.01 红谷滩智慧政务项目-优化首次视频加载不出来问题；Insight-修改 Host 引入方式
- 03.02 查看智慧产业相关文档
- 03.03 查找智慧农贸系统，设计智慧农贸原型大体结构
- 03.04 讨论数据大屏保存接口数据结构，设计智慧农贸原型首页
- 03.05 设计智慧农贸原型整体框架，探讨智慧政务优化建议，探索 js 录制音频功能
- 03.08 智慧政务-探索 js 录制视频功能，对接 Insight 大屏保存接口
- 03.09 Insight-调试大屏保存和获取接口，智慧政务-调整页面整体结构
- 03.10 智慧政务-开发视频录制模块
- 03.11 智慧政务-开发审核详情和求助记录等
- 03.12 智慧政务-根据 UI 设计稿开发新需求功能
- 03.13 智慧政务-对接 web 端更新和新增的接口
- 03.15 智慧政务-项目联调测试，优化界面及功能细节
- 03.16 智慧政务-优化项目细节，查找安卓端小程序未能正常播放视频留言问题
- 03.17 智慧政务-录制视频留言上传功能开发，根据会议建议新增界面功能
- 03.18 智慧农贸原型-设计食品安全模块原型；智慧政务-根据测试所提的一些建议优化初始功能
- 03.19 智慧农贸原型-设计菜品管理、溯源管理和招聘管理等模块原型
- 03.22 智慧政务-新增加无感审批，重新调整系统路由
- 03.23 智慧政务-开发无感审批功能及对接相关接口
- 03.24 智慧政务-修复出现的一些小问题，智慧农贸原型-设计商户管理、供应商管理模块
- 03.25 智慧农贸原型-设计商品管理、检测管理模块，在线播放 PPT 项目-探索获取通过 iframe 引入的第三方网站 dom
- 03.26 智慧农贸原型-设计客流管理、消防管理和新闻资讯模块
- 03.29 重大项目原型-查阅重大项目建设方案，修改重大项目结构
- 03.30 重大项目原型-设计首页、新闻模块、政策法规、下载中心和项目调度等模块
- 03.31 重大项目原型-设计项目详情和项目监控等模块

### 一季度考评

- 如期完成红谷滩智慧政务演示系统开发(Web 端)的开发，主要工作有：1.探讨和挖掘项目需求；2.项目架构搭建、优化及后续更新部署；3.视频通话技术选型及相关难点攻克，并建立起 web 端和小程序的视频通话；4.开发包括远程视频办理、邮寄资料审核、电子资料审核、视频求助办理、在线指导、审批类办理、无感审批和消息聊天等相关功能。5.声网的视频通话需要通过 https 协议访问才能使用，而我们的服务器 80 端口已经被官网占用，在不影响官网的使用也不能将智慧政务的项目合并到官网项目的情况下，我想出了官网路由后配置智慧政务的路由，于是修改 nginx 的相关配置，将智慧政务部署在其他的端口，并在官网某个路由下通过转发代理到智慧政务所在的端口，智慧政务相应的跟路由也要做指定的更改。

- 完善华链智慧 Insight 数据大屏项目，主要工作有：1.大屏保存的数据量巨大而且结构复杂，我使用 Vuex 巧妙的设计了大屏数据缓存方式，前端不仅能一键读取到指定图表的所有数据，在保存和获取大屏数据时，两者的转换也极为简便；2.对接数据代理模块接口；3.对接大屏保存接口(静态类型数据)，实现从获取大屏列表到编辑画板的大屏回显和编辑保存再到大屏预览这一整个流程的打通；4.修复开发过程出现的诸多问题。

- 完成了华链智慧官网首屏加载优化、百度搜索收录和版本更新部署。

- 华链物联网云平台功能更新，1.实现了节点树的左键点击和右键点击具有相同的效果，项目所使用的树型控件左键点击相比右键点击缺少了点击点的位置信息，因而无法确定点击后弹框的具体位置，为了解决该问题，我在树型控件的自定义方法中添加了自定义点击事件，通过该事件能获取到点击点的相关位置信息，从而实现了节点树的左键点击和右键点击具有相同的效果；2.开发鼠标悬浮在节点上时在右侧的 2.5D 图中标示相关位置，鼠标悬浮功能在树型控件同样是没有定义的，为了实现该效果我同样是在自定义方法中添加了鼠标悬浮事件。

- 红谷滩智慧产业系统原型设计，主要工作：1.完成智慧农贸系统的原型设计及评审，包括信息展示模块、商户管理系统和管理员管理系统；2.重大项目管理系统的新闻、政策法规和项目详情的原型设计；3.在绘制原型时最大的困难就是需求不清晰，智慧农贸系统的需求文档只有简短几页，每个模块简单的一两句话概括，通话逐字逐句研读以及联想各功能点之间的关联，最终才绘制出功能齐全，逻辑合理的智慧农贸系统原型；

- 滴谱智慧共享办公二期开发，除了一张未给的平面图外，其他的已开发完成；左键点击效果华链物联网云平台的实现方式一致。

### 2021-04

- 04.01 智慧政务-梳理各科室的业务办理流程，绘制成思维导图，根据新改动的接口对 web 端做了相应的修改
- 04.02 智慧政务-设计权限管理原型，参加一季度工作总结汇报
- 04.06 智慧政务-完善权限管理原型，搭建权限管理模块
- 04.07 智慧政务-开发登录模块，开发路由改变时变换权限菜单选中项功能
- 04.08 智慧政务-开发权限管理的列表功能，将列表与分页组合封装成通用模块
- 04.09 智慧政务-开发权限管理的新增和编辑功能
- 04.12 智慧政务-对页面结构做了全面的调整，将菜单栏分为一级菜单和二级菜单
- 04.13 智慧政务-对接登录和重新对接其他接口
- 04.14 智慧政务-开发数据字典功能，评审页面设计及开发计划
- 04.15 智慧政务-调整页面主题结构，开发及对接数据字典接口
- 04.16 智慧政务-调试 websocket 通信连接及心跳包检测，调试数据字典树状结构
- 04.19 智慧政务-对接数据字典树状结构编辑和删除，开发菜单管理功能，
- 04.20 智慧政务-对接菜单管理新增、编辑和删除接口;智慧农贸-和曾总讨论原型修改方案
- 04.21 智慧农贸-新增消防设施管理，消防预案，信息发布，采购信息等模块的原型设计
- 04.21 智慧农贸-新增消防设施管理，消防预案，信息发布，采购信息等模块的原型设计
- 04.22 智慧农贸-新增菜品采集模块原型设计，智慧政务-对接角色管理功能
- 04.23 机场大屏-设计大屏原型，搭建项目框架
- 04.25 机场大屏-开发机场大屏，查找并解决云平台综合管理系统的场景点击未能出现控制菜单的问题
- 04.26 智慧政务-对接和调试业务管理接口
- 04.27 智慧政务-开发首页功能，智慧农贸-评审农贸原型
- 04.28 智慧农贸-修改智慧农贸原型
- 04.29 智慧政务-开发数据分析页面功能
- 04.30 智慧政务-开发个人信息页面功能及部分接口对接

### 2021-05

- 05.06 智慧政务-完善个人信息页面功能，智慧农贸-修改系统管理原型设计
- 05.07 重大项目-设计项目申报管理模块原型
- 05.08 智慧政务-把获取业务列表及消息对话轮询修改为 websocket 方式
- 05.10 智慧政务-对接用户管理接口，根据用户角色动态生成菜单列表并限制恶意访问没有权限的菜单
- 05.11 智慧政务-数据字典新增图片上传功能，对接数据分析接口
- 05.12 智慧政务-修改预约时间段选择方式、菜单展开方式及联调部分业务
- 05.13 智慧政务-调整全局公共样式，开发多事联办业务
- 05.14 智慧政务-对接首页业务列表、业务归档等接口
- 05.17 机场项目-上午去沟通交流项目需求，智慧政务-调试业务归档，完善 table 表单分页功能
- 05.18 智慧政务-对接和调试智能审批、业务归档和多事联办等接口，机场项目-制定开发计划
- 05.19 机场项目-研究项目文档，分配文档编写任务，到机场核对需求
- 05.20 智慧政务-修改业务管理和根据用户角色获取菜单的问题，和小程序联调
- 05.21 智慧政务-和小程序联调，修改所出现问题，参加会议汇报项目进展
- 05.24 智慧政务-解决线上环境接口请求，websocket 连接和访问白板跨域的问题
- 05.25 智慧政务-开发首页快捷办理业务入口功能
- 05.26 智慧政务-和小程序联调，修改业务的可关联多部门、修改视频通话业务办结功能
- 05.27 智慧政务-审核出生一件事及设置状态为办理中的业务访问权限，调试数字签名和消息聊天功能
- 05.28 智慧政务-修复调试中出现的一些问题；云平台-修复云平台树节点点击及综合管理系统中灯光控制延迟的问题
- 05.29 智慧政务-修改出生一件事的审核流程；机场项目-制作智能化项目解决方案 PPT
- 05.31 智慧政务-修复测试发现的一些问题；机场项目-整理项目所需的文件和需求，创建 web 端框架和总览大屏

### 2021-05

- 06.01 机场项目-开发总览页大屏，查找体温监控系统的解决方案
- 06.02 机场项目-开发总览页大屏的整体布局，调试不同分辨率下页面的适配情况
- 06.03 机场项目-审查机场项目投标文档，调整总览页面图表样式
- 06.04 机场项目-研究和开发全球物流集散飞线图
- 06.07 机场项目-开会讨论开发中遇到的一些困难并做了会议纪要，根据机场提供的通航城市添加了飞线图航线
- 06.08 机场项目-开发智能管控系统，实现分区切换平面图，对接天气和登录接口
- 06.09 机场项目-调整全球飞线图，实现城市显示和国界划分，放大和缩小等功能，研究 web 端播放.flv 格式的监控视频
- 06.10 机场项目-开发登录页面，新增全球飞线图缩放功能，更换一些基本图标
- 06.11 机场项目-对接总览大屏图表接口，开发智能管控分区功能
- 06.15 机场项目-对接智能管控接口，修复切换平面图存在残留的问题
- 06.16 机场项目-调整飞线图的城市显示方式和封装flv视频格式播放方法
