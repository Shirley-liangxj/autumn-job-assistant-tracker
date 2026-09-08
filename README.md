# 🚀 秋招求职与简历助手 · 增强版

> 在 [ljkss/autumn-job-assistant-tracker](https://github.com/ljkss/autumn-job-assistant-tracker) 之上迭代而来的浏览器扩展，**新增「多版本简历管理」核心模块**，解决"一库只能存一份简历"在差异化投递场景下的硬伤。

## ✨ 核心特性

| 模块 | 能力 |
|---|---|
| 📝 **简历侧边栏** | Shadow DOM 注入，与任意招聘网站 CSS 100% 隔离；`Ctrl+Shift+F` 一键唤起；点击字段自动填入并触发原生事件 |
| 📌 **岗位一键收录** | JSON-LD / OpenGraph / 正文正则三层兜底识别，秒级提取公司-岗位-城市 |
| 📊 **投递管理中枢** | 阶段漏斗、状态统计、搜索/排序、日程待办，IndexedDB 历史快照回滚 |
| 🖼️ **离线 OCR** | Tesseract 中文识别引擎，截图即建档（微信/邮件/网申成功页） |
| 📚 **🆕 多版本简历** | 同时维护"国央企版 / 管培生版 / 私企版"等多份简历，切换时一键同步到所有已开网申页 |

## 🆕 本版本相比原版增强

### 📚 多版本简历管理（Multi-Version Resume）

#### 背景问题
秋招时同一求职者常需维护多份差异化简历：

- **投央国企**：突出政治面貌、学生干部、稳定性、体制内相关经历
- **投私企民企**：突出项目成果、数据量化、技术深度
- **投管培生**：突出综合素养、领导力、跨部门协作

原版 `chrome.storage.local` 只有一个 `resume.v1` 槽位，导入新 JSON **直接覆盖旧版**且无切换能力。在多版本同时维护的真实场景下极易误填，**尤其把含体制内经历的版本错投到私企这种高风险场景**。

#### 解决方案
- 新增 `resumeVersions.v1` 存储位，按版本名存多份简历
- 顶部下拉框一键切换"当前生效版本"
- 切换时把选中版本**低侵入写入 `resume.v1`**，content.js 零改动
- 所有已开网申页通过 `storage.onChanged` 自动同步

#### 能力清单
- 版本下拉切换
- 新建版本（基于当前或导入新 JSON）
- 删除当前版本（保留至少 1 份）
- 保存/导入时自动回写到当前版本
- 向下兼容旧版单简历备份

## 🛠️ 安装与使用

### 环境
- 浏览器：**Edge 88+**（推荐）或 Chrome 88+
- 数据 100% 存储在本机 `chrome.storage.local`，**无任何外发**

### 步骤
1. 打开 `edge://extensions/`（或 `chrome://extensions/`）
2. 右上角开启**开发者模式**
3. 点击**加载已解压的扩展程序**
4. 选择本项目根目录即可

### 多版本简历用法
1. 首次安装 → 打开扩展图标 → 「简历资料库」→ 顶部「📚 简历版本」→ 新建"国央企版"，导入对应 JSON
2. 继续新建"管培生版""私企版"，分别导入对应 JSON
3. 投递前在下拉框选对应版本 → 打开网申页 → 侧栏自动加载该版本内容
4. ⚠️ 切换版本不会清空已填内容，但自动填充会按新版本走，请逐条核对

## 📁 项目结构

```
autumn-job-assistant-tracker/
├── manifest.json          # 扩展清单 V3
├── background.js          # 后台 Service Worker
├── content.js             # 注入页面的侧边栏主逻辑
├── dashboard.html         # 管理中枢 UI
├── dashboard.js           # 管理中枢逻辑（含多版本模块）
├── tesseract/             # 离线 OCR 引擎
├── icons/                 # 扩展图标
└── README.md              # 本文件
```

## 🙋 致谢

- 原作者：[ljkss/autumn-job-assistant-tracker](https://github.com/ljkss/autumn-job-assistant-tracker)，保留原项目全部权利
- 增强版作者：[Shirley-liangxj](https://github.com/Shirley-liangxj)
- 增强提交：`feat(resume): add multi-version resume management`
- 本仓库基于原项目 fork，LICENSE 状态详见原仓库
