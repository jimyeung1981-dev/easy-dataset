# Easy Dataset 项目分析

## 1. 项目概述

Easy Dataset 是一个专为大型语言模型（LLM）微调数据集创建而设计的应用程序。它结合了Web技术和桌面应用能力，提供从文档处理到数据集生成的完整工作流程。

## 2. 技术栈

- **前端**: Next.js, React, Material-UI (MUI)
- **后端 & 数据库**: Node.js, Prisma ORM, SQLite
- **桌面应用**: Electron
- **AI 集成**:
  - OpenAI
  - Ollama (本地模型)
  - 智谱AI
  - OpenRouter
- **文件处理**: 支持 PDF, DOCX, Markdown 等多种格式。
- **国际化**: i18next

## 3. 核心功能

- **多源文档导入**: 支持多种文件格式（PDF, DOCX, MD等）的文本提取。
- **智能文本分割**: 提供多种文本分割策略，以适应不同文档结构。
- **多模型支持**: 集成多家AI提供商，方便用户选择不同模型生成数据。
- **结构化数据生成**: 支持生成问答对、多轮对话等多种格式的数据集。
- **跨平台桌面应用**: 通过 Electron 提供在 Windows, macOS, 和 Linux 上一致的本地体验。
- **数据库管理**: 使用 Prisma 和 SQLite 管理项目、文本块和生成的数据集。

## 4. 项目结构

项目采用分层架构，前端、后端和核心逻辑分离清晰。

### 4.1. 目录结构

```
/
├── app/            # Next.js App Router, 页面和API路由
│   ├── api/        # API路由定义
│   └── projects/   # 项目管理页面
├── lib/            # 核心业务逻辑
│   ├── db/         # 数据库操作 (Prisma)
│   ├── llm/        # AI模型集成
│   ├── file/       # 文件处理
│   ├── services/   # 业务逻辑服务
│   └── util/       # 工具函数
├── electron/       # Electron主进程代码
├── prisma/         # Prisma Schema和数据库文件
├── public/         # 静态资源
└── components/     # React组件
```

### 4.2. 核心模块

- **`app/`**: 基于 Next.js 的 App Router，负责页面渲染和 API 路由。
- **`lib/`**: 项目的核心，包含了数据库访问、AI 模型调用、文件处理等关键逻辑。
- **`electron/`**: Electron 的主进程和预加载脚本，负责创建窗口和与系统交互。
- **`prisma/`**: 存放 `schema.prisma` 文件，定义了数据模型和数据库关系。

## 5. 数据模型 (Prisma Schema)

数据模型是整个应用的核心，定义了项目、文件、数据和配置之间的关系。

- **核心流程模型**:
  - `Projects`: 项目的顶层容器。
  - `UploadFiles`: 存储上传的原始文件信息。
  - `Chunks`: 将文件内容分割成的文本块。
  - `Questions`: 基于文本块生成的问题。
  - `Datasets`: 最终生成的问答对数据集。

- **AI 与配置模型**:
  - `LlmProviders`, `LlmModels`, `ModelConfig`: 管理和配置不同的AI模型。
  - `CustomPrompts`: 允许用户为不同任务自定义提示词。

- **任务与扩展模型**:
  - `Task`: 跟踪异步任务（如文件处理、问答生成）的状态。
  - `DatasetConversations`: 支持多轮对话数据集。
  - `Images`, `ImageDatasets`: 支持图像数据集。

## 6. AI 模型集成 (`lib/llm`)

项目的 AI 模型集成设计具有良好的结构和扩展性。

- **统一接口**: 通过 `lib/llm/core/providers/base.js` 定义一个统一的接口，所有模型提供商都遵循这个接口进行实现。
- **多提供商支持**: 在 `lib/llm/core/providers/` 目录下，为不同的 AI 服务（如 OpenAI, Ollama, 智谱AI, OpenRouter）提供了具体的实现。
- **动态调度**: `lib/llm/core/index.js` 作为统一入口，根据用户的配置动态选择和实例化对应的 AI 模型提供商。
- **提示词管理**: `lib/llm/prompts/` 目录专门用于存放和管理不同任务的提示词模板，并支持国际化。

## 7. 文件处理与文本分割 (`lib/file`)

文件处理模块负责从各种格式的文档中提取内容并进行分割。

- **多格式支持**: 在 `lib/file/file-process/` 目录下，为不同文件类型（如 PDF, EPUB, DOCX）提供了专门的内容提取逻辑。
- **智能文本分割**: `lib/file/text-splitter.js` 提供了通用的文本分割功能，可以将长文本切分成适合语言模型处理的小块（Chunks）。
- **Markdown 处理**: 包含专门针对 Markdown 格式的分割工具，以保留其结构信息。

---
项目分析完成。
