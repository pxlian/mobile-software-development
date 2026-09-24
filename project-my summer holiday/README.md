# ongoing_ · 未完

一个记录生活片段的原生微信小程序。用 **Moment** 留下文字、照片或声音，用 **Chapter** 整理一段生活；同一条 Moment 可以由不同的人留下各自的视角，之后再通过日历、回望和 Memory Echo 阅读这些记录。

当前源码版本为 `0.3.0`（见 `app.js`）。首次使用显示空状态；演示数据保存在 `data/demo.js`，不会在启动时伪造用户记录。

## 已实现的体验

- **当下**：记录入口、最近的 Moment、进行中的 Chapter 和基于真实记录日期的 Memory Echo。
- **生活**：按时间浏览可见的 Moment，也可切换查看 Chapter；支持搜索文字、地点和标签。
- **Moment**：文字、多图、语音、日期、地点、心情、标签、草稿、收藏、编辑与删除。Chapter 归属可在记录时选择，也可稍后整理。
- **Chapter**：创建、编辑、成员管理、邀请、完成与归档；详情提供瞬间、日历和回望。现有页面还保留目标数据和入口，用于兼容历史记录。
- **共同记录**：通过复制邀请文字、在「生活 → 加入共同记录」粘贴的方式加入 Chapter 或单条 Moment。不同作者的内容和评论分别保存，并按身份检查编辑权限。
- **回望**：Chapter 的时间线、照片选择与 Canvas 海报；个人档案统计记录日、Moment 和共同记录。

三个主入口是「当下 / 生活 / 我的」。小程序入口、页面和组件分别位于 `app.js`、`pages/` 和 `components/`。

## 本地运行

1. 用微信开发者工具导入本目录 `project-my summer holiday`。
2. 使用 `project.config.json` 中的 AppID，或改为自己的测试号，然后编译。
3. 从首页直接新建 Moment 或 Chapter。本地记录由 `services/store.js` 持久化，照片和声音由 `services/media.js` 保存；单人本地体验无需先部署云函数。

跨设备协作需要微信云开发环境。项目在 `app.js` 中配置了云环境 ID，云函数源码在 `cloudfunctions/collaboration/`；必须在自己的环境中配置数据库和云存储权限、部署云函数，再上传体验版。所需的六个集合、部署步骤和双账号验收流程见 [CLOUD_SETUP.md](./CLOUD_SETUP.md)。**推送 GitHub 源码不会部署云函数或小程序体验版。**

## 代码结构

```text
app.js / app.json / app.wxss       启动、页面注册、全局样式
assets/                            图像与图标
pages/                             当下、生活、Chapter、Moment、回望、搜索、我的
components/                        导航、卡片、内容流与共同视角组件
services/store.js                  本地数据、权限与页面查询
services/collaboration.js          邀请、云同步、离线待同步队列与媒体上传
services/media.js                  本地媒体保存
services/review-poster.js          回望海报
cloudfunctions/collaboration/      云端身份、邀请、权限与数据读写
tests/                             Node.js 检查与微信工具编译脚本
```

页面通过 `services/store.js` 读写业务数据。云端写入由 `services/collaboration.js` 和 `collaboration` 云函数处理；本地修改先保存，再进入待同步队列。真实账号身份由云函数读取微信 OPENID。

## 验证状态

在本目录运行自动检查：

```bash
node --test tests/*.test.js
```

截至 2026-09-25，完整 Node.js 检查为 **10 组通过 6 组、失败 4 组**。失败项为 `archive-behavior`、`design-system`、`responsive-visual` 和 `ui-interactions`；其余包括云端权限模拟、协作同步模拟、核心体验、海报、静态项目和时间体验检查通过。这些结果不能代替微信开发者工具编译和真机验收。

当前仓库没有可核实的最新版云函数部署记录或真实双账号验收结果。跨设备邀请、媒体同步和权限仍需按 [CLOUD_SETUP.md](./CLOUD_SETUP.md) 在微信环境中检查；交接背景见 [HANDOFF.md](./HANDOFF.md)。

## 相关文档

- [CLOUD_SETUP.md](./CLOUD_SETUP.md)：云开发部署与双账号验收。
- [HANDOFF.md](./HANDOFF.md)：本轮实现和待验收事项。
- [PRODUCT_SCOPE_V1.md](./PRODUCT_SCOPE_V1.md)：产品边界与信息架构；其中部分页面描述保留了较早版本的规划，当前页面以源码为准。
- [个人展示内容包-6分钟终版.md](./个人展示内容包-6分钟终版.md)：课程汇报内容与演示脚本。
