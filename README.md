# IconCreator

IconCreator 是一个面向设计师和前端开发者的 AI 图标工作台。它可以把自然语言描述转换为可用图标：既支持从开源图标库中批量匹配 SVG，也支持生成 3D 风格的原创图标图片。

> 当前项目仍处于持续迭代阶段。公开使用前，请自行配置后端密钥，不要把任何真实密钥提交到仓库。

## 功能概览

- **SVG 图标匹配**：输入一组中文或英文词语，系统会优先在本地图标索引中查找，再借助 OpenAI 兼容接口做语义匹配。
- **统一风格输出**：匹配结果会尽量保持同一图标库、同一视觉风格，减少图标混用带来的割裂感。
- **AI 3D 图标生成**：支持选择提示词风格、主色调、材质、尺寸和比例，一次生成多张候选图。
- **数字孪生提示词实验区**：内置一份可编辑的提示词规则，用于多轮补全需求、整理生成提示词并调用图片生成接口。
- **历史记录与导出**：保留最近的匹配和生成记录，支持复制、下载单个图标，也支持按组打包导出 SVG。
- **多图标库支持**：当前接入 Lucide、Heroicons、Phosphor 和 Tabler 的部分风格。

## 项目结构

```text
IconCreator/
├─ frontend/             # 前端工作台，React + Vite
├─ backend/              # 后端服务，Fastify + Node.js
├─ shared/               # 前后端共享的类型、配置和图标索引
├─ docs/                 # 产品、设计和集成说明
├─ prototype/            # 早期 HTML 原型和视觉参考
├─ scripts/              # 图标索引生成等辅助脚本
└─ README.md
```

## 技术栈

| 模块 | 技术 |
| --- | --- |
| 前端 | React 19、TypeScript、Vite、Tailwind CSS |
| 状态管理 | Zustand |
| 图标渲染 | Iconify |
| 后端 | Node.js、Fastify |
| 数据校验 | Zod |
| 包管理 | pnpm workspace |
| AI 接口 | OpenAI 兼容协议、图片生成接口 |

## 本地运行

请先确认本机已安装 Node.js，并启用 pnpm。

```bash
corepack enable
corepack pnpm install
```

启动前后端联调：

```bash
corepack pnpm dev
```

默认地址：

- 前端：`http://localhost:5173`
- 后端：`http://localhost:8787`
- 健康检查：`http://localhost:8787/api/health`

常用检查命令：

```bash
corepack pnpm check
corepack pnpm build
```

刷新本地图标名称索引：

```bash
corepack pnpm catalog:names
```

## 环境变量

项目提供了示例文件：

- `backend/.env.example`
- `frontend/.env.example`

复制示例文件后再填写自己的配置：

```bash
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env.local
```

后端常用配置：

| 变量 | 说明 |
| --- | --- |
| `LLM_BASE_URL` | OpenAI 兼容接口地址 |
| `LLM_MODEL` | 文本模型名称 |
| `LLM_API_KEY` | 文本模型密钥 |
| `LLM_SYSTEM_PROMPT` | 默认系统提示词 |
| `AI_IMAGE_BASE_URL` | AI 3D 图标生成接口地址 |
| `AI_IMAGE_MODEL` | AI 3D 图标生成模型 |
| `AI_IMAGE_API_KEY` | AI 3D 图标生成密钥 |
| `APIYI_IMAGE_BASE_URL` | 数字孪生图片生成接口地址 |
| `APIYI_IMAGE_MODEL` | 数字孪生图片生成模型 |
| `APIYI_IMAGE_API_KEY` | 数字孪生图片生成密钥 |

前端示例配置只建议用于本地开发。公开部署时，请把真实密钥放在后端或部署平台的安全配置中，不要注入到前端构建产物里。

## 支持的图标库

| 图标库 | 已接入风格 |
| --- | --- |
| Lucide | 线性 |
| Heroicons | 线性、填充 |
| Phosphor | 常规、双色 |
| Tabler | 线性 |

更多 Iconify 集成说明见 [docs/ICONIFY.md](./docs/ICONIFY.md)。

## 部署提示

- 先构建共享包和后端，再启动后端服务。
- 前端生产环境可以独立部署，但 `/api` 请求需要转发到最新的后端服务。
- 生产环境建议通过部署平台管理密钥，不要把 `.env`、`.env.local` 或真实密钥提交到 GitHub。
- 发布前建议执行 `corepack pnpm check` 和 `corepack pnpm build`。

## 公开仓库注意事项

在公开到 GitHub 前，建议确认：

- `.env`、`.env.local`、日志文件和构建缓存不会提交。
- 示例环境变量文件中不包含真实密钥。
- 生成的图片、占位图和第三方素材符合授权要求。
- README、docs 和界面文案中没有内部账号、私有地址或调试信息。

## 文档

- [产品说明](./docs/PRD.md)
- [设计说明](./docs/DESIGN.md)
- [开发进度](./docs/PROGRESS.md)
- [Iconify 集成说明](./docs/ICONIFY.md)

## 许可

当前仓库尚未声明开源许可证。如需公开发布并允许他人使用、修改或分发，请在仓库根目录补充 `LICENSE` 文件。

## 素材说明

`prototype/assets/hero-banner.jpg` 来自 [Lorem Picsum](https://picsum.photos/)，仅作为原型占位图使用。正式发布时建议替换为自有或授权明确的素材。
