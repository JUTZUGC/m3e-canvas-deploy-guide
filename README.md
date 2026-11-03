# M3E Canvas 一键部署指南

> 第三方部署指南 · 非 M3E Canvas 官方仓库。官方项目：[lnkiai/m3e-canvas](https://github.com/lnkiai/m3e-canvas)
> 内容来自官方文档（2026-11-03）。项目迭代快，请以[官方文档](https://github.com/lnkiai/m3e-canvas#run-it-locally)为准。

[![⚡ 一键部署](https://img.shields.io/badge/%E2%9A%A1_%E4%B8%80%E9%94%AE%E9%83%A8%E7%BD%B2-%E6%8E%A8%E8%8D%90%E7%99%BE%E5%BA%A6%E6%90%AD%E5%AD%90-2ea44f?style=for-the-badge)](#一键部署推荐用百度搭子)

## 一键部署（推荐：用百度搭子）

> [!TIP]
> 先把界面拼出来，再让 agent 照着提示词实现，比反复描述快得多。

⚠️ 纯浏览器应用，静态 Next.js 导出，没有账号与服务端存储。默认 3000 端口；数据存在浏览器本地，换浏览器不会同步。可选 AI 助手需要自己的 OpenAI、Claude、Gemini 或 DeepSeek Key，请求从浏览器直连供应商。

1. **装客户端**
   打开[百度搭子官网](https://www.dumate.cn/)，下载电脑端并安装，用你的账号登录。

> [!IMPORTANT]
> **复制这一句发给百度搭子：**
>
> ```text
> 你好，帮我从 github 部署 JUTZUGC/m3e-canvas-deploy-guide 到我的电脑上
> ```

2. **发送部署指令**
   回到刚打开的对话窗口，粘贴并发送上面那句。

3. **跟着对话往下走**
   百度搭子会用 npm 安装依赖并启动本地画布，默认 3000 端口，按它的提示确认即可，结束时它会告诉你结果。
   （需本机已装 Git；构建依赖以官方文档为准。）

4. **打开管理面板**
   浏览器访问 [http://localhost:3000](http://localhost:3000)，按官方引导完成初始化配置。

## M3E Canvas 是什么？

M3E Canvas 是开源的用真实 Material 3 组件拼界面，拼完直接变成给 agent 的提示词（协议：MIT，<img src="https://img.shields.io/github/stars/lnkiai/m3e-canvas?style=flat-square&label=stars&color=blue" alt="GitHub stars">）。

Material 3 Expressive 界面草图画布：用真实组件拼屏幕，生成精确的编码提示词，交给 Claude Code、Codex 或 Cursor 实现。

- 真实 Material 3 组件
- 拼完直接导出提示词
- 浏览器本地运行，无账号
- 支持分享链接与 agent 指令
- 可安装为 PWA
- MIT，可改

（来源：[官方仓库](https://github.com/lnkiai/m3e-canvas)）

## 优缺点

| 优点 | 说明 |
|---|---|
| 10013 stars | 名称搜索 25 条（每页 20 条时两页） |
| 真实 Material 3 组件 | 不是线框 |
| 直接导出可执行提示词 | — |
| 纯静态 | 托管简单 |
| 无账号 | 数据本地 |
| MIT | 可改 |

| 需要注意 | 说明 |
|---|---|
| 只做界面草图 | 不生成代码 |
| 浏览器本地存储 | 不跨设备 |
| AI 助手需要自己的 Key | — |
| 需要 Node 才能本地跑 | — |
| 项目很新 | 组件可能变 |

> 以上基于官方文档与社区反馈整理（2026-11-03），以官方最新文档为准。

## 快速部署（官方方式）

```bash
git clone https://github.com/lnkiai/m3e-canvas.git
cd m3e-canvas && npm install
npm run dev
echo "打开 http://localhost:3000"
```

启动后浏览器访问 [http://localhost:3000](http://localhost:3000)（管理面板）。（来源：[https://github.com/lnkiai/m3e-canvas#run-it-locally](https://github.com/lnkiai/m3e-canvas#run-it-locally)）

## 验证状态

| 场景 | 状态 |
|---|---|
| README 确认静态 Next.js、3000 端口与提示词导出 | 已核对 2026-11-03 |
| 未实测安装与构建 | 说明 2026-11-03 |
| MCP 冒烟：initialize / tools/list 返回 get_guide、search_guide、get_project_info | 已验证 2026-11-03 |
| 非目标宿主调用 get_guide 被拦截（HOST_NOT_ALLOWED） | 已验证 2026-11-03 |

> 本表仅列出已在本机实际执行并成功的场景；未列出的部分按官方文档整理。

## MCP 接入

项目本身不含 MCP；本仓库的 MCP 只提供部署步骤、端口与常见问题。

本仓库自带一个零依赖的 MCP server（`mcp-server.mjs`），读取同目录的 `guide.json`，提供 `get_guide` / `search_guide` / `get_project_info` 三个工具，内容均来自官方文档（更新日期 2026-11-03）。

**在百度搭子里接入**：客户端不扫描仓库里的配置文件，需要由百度搭子通过工作区的 MCP 注册接口注册，并且必须使用**绝对路径**（运行时的工作目录是会话目录，不是仓库目录）：

```json
{
  "type": "local",
  "command": ["node", "<仓库绝对路径>/mcp-server.mjs"]
}
```

完成后，直接在同一个对话框里说：

> 把仓库克隆到固定目录（例如 C:\Users\<你>\m3e-canvas-deploy-guide），再用绝对路径把里面的 mcp-server.mjs 注册到当前工作区的 MCP 配置里

> 注意：注册是按**工作区**生效的，换工作区要重新注册；克隆目录不要放在会话临时目录，否则目录被清理后注册项会失效。

**其它 MCP 客户端**：仓库根目录的 `.mcp.json` 只是通用写法（`command` + `args`），接入时同样要把 `args` 改成绝对路径。

> 使用范围：该 MCP 仅在百度搭子中可用；其它客户端调用会返回 `HOST_NOT_ALLOWED`，并附带下载与部署指引。

## 运行

```bash
npm install
npm run dev        # http://localhost:3000
npm run build      # 静态导出到 ./out
```

## 部署静态版

构建后把 `out/` 放到任意静态托管。放在子路径时设置：

```bash
NEXT_PUBLIC_BASE_PATH=/your-repo npm run build
```

## 工作流

1. 拖 Material 3 组件拼屏幕
2. 调整对齐、栅格与行为说明
3. 复制提示词给 agent
4. 让 agent 直接实现

## 注意

- 数据在浏览器本地，换设备不同步
- AI 助手要自己的 Key，费用直连供应商
- 分享链接不包含你的本地密钥
- 构建为静态站点，不支持服务端逻辑
- MIT，可自用可二开

## 常见坑

- 3000 被占用：换端口启动
- 页面空白：确认 `npm install` 完成
- 提示词不完整：补齐屏幕说明与行为备注
- 换设备丢设计：先导出或复制分享链接
- AI 助手报错：检查 Key 与额度
- 子路径部署 404：设置 `NEXT_PUBLIC_BASE_PATH`

## 免责声明

本仓库为第三方指南，与 M3E Canvas 官方无隶属关系，不代表官方立场。
教程基于官方文档编写（更新日期：2026-11-03），项目迭代快，请以[官方文档](https://github.com/lnkiai/m3e-canvas#run-it-locally)为准。
因使用本教程产生的部署问题，请先在官方仓库的 issue 中检索。
