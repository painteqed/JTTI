# Azure OpenAI 服务详解

你当前正在访问 Microsoft Azure Global Edition 技术文档网站。如果需要访问由世纪互联运营的 Microsoft Azure 中国技术文档网站，请访问 [https://docs.azure.cn](https://docs.azure.cn)。

## 本文内容

Azure OpenAI 服务提供了对 OpenAI 强大语言模型的 REST API 访问，包括 o1、o1-mini、GPT-4o、GPT-4o mini、支持视觉的 GPT-4 Turbo、GPT-4、GPT-3.5-Turbo 和嵌入模型系列。这些模型可以轻松适应特定任务，如内容生成、汇总、图像理解、语义搜索和自然语言到代码的转换。用户可以通过 REST API、Python SDK 或在 [Azure AI Foundry](https://ai.azure.com) 中访问该服务。

| 可用的模型 | 描述 |
| ---------- | ---- |
| o1 和 o1-mini | 有限访问权限 - [请求访问权限](https://aka.ms/OAI/o1access) |
| GPT-4o 和 GPT-4o mini | 最新的高级语言模型 |
| GPT-4 系列（包括 GPT-4 Turbo with Vision） | 支持视觉的模型 |
| GPT-35-Turbo 系列 | 高效的语言模型 |
| Embeddings 系列 | 用于语义搜索的嵌入模型 |

| 微调 | 描述 |
| ---- | ---- |
| GPT-4o-mini（预览版） | 适用于特定任务的微调模型 |
| GPT-4（预览版） | 最新模型的微调版本 |
| GPT-3.5-Turbo (0613) | 高效且灵活的模型 |

| 价格 | 描述 |
| ---- | ---- |
| [查看定价](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/) | 详细的价格信息 |
| [特殊的定价信息](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/concepts/gpt-with-vision#special-pricing-information) | 针对视觉模型的定价 |

## 负责任 AI

Microsoft 致力于以“以人为本”的原则推动 AI 的进步。生成性模型（如 Azure OpenAI 中提供的模型）具有显著的潜在优势，但如果不经过精心设计和采用全方位的缓解措施，这些模型可能会生成错误甚至有害的内容。Microsoft 已进行了大量投资来帮助防范滥用和意外损害，包括融入 Microsoft 的[负责任 AI 使用原则](https://www.microsoft.com/ai/responsible-ai?activetab=pivot1:primaryr6)，采用服务使用[行为准则](https://learn.microsoft.com/zh-cn/legal/cognitive-services/openai/code-of-conduct?context=/azure/ai-services/openai/context/context)，生成[内容筛选器](https://learn.microsoft.com/zh-cn/azure/ai-services/content-safety/overview)以支持客户，以及提供客户在使用 Azure OpenAI 时应考虑的负责任 AI [信息和指导](https://learn.microsoft.com/zh-cn/legal/cognitive-services/openai/transparency-note?context=%2Fazure%2Fai-services%2Fopenai%2Fcontext%2Fcontext&amp;tabs=image)。

## 开始使用 Azure OpenAI 服务

若要开始使用 Azure OpenAI 服务，需要在 Azure 订阅中创建 Azure OpenAI 服务资源。请首先查看以下指南：[创建和部署 Azure OpenAI 服务资源](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/how-to/create-resource)。

> 可以通过 Azure 门户、Azure CLI 或 Azure PowerShell 创建资源。拥有 Azure OpenAI 服务资源时，可以部署 GPT-4o 等模型。部署模型后，可以：
>
> - 试用 Azure AI Foundry 门户操场，探索模型的功能。
> - 使用 REST API 或 SDK 开始对服务进行 API 调用。

例如，你可以在操场或通过代码试用[实时音频](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/realtime-audio-quickstart)和[助手](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/assistants-quickstart)功能。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/WildCard)

## 比较 Azure OpenAI 和 OpenAI

Azure OpenAI 服务通过 OpenAI GPT-4、GPT-3、Codex、DALL-E、Whisper 和文本转语音模型为客户提供高级语言 AI，并能够实现 Azure 的安全性和企业承诺。Azure OpenAI 与 OpenAI 共同开发 API，确保兼容性的同时能够实现二者之间的平稳过渡。使用 Azure OpenAI，客户可在运行与 OpenAI 相同的模型时获得 Microsoft Azure 的安全功能。Azure OpenAI 提供专用网络、区域可用性和负责任 AI 内容筛选功能。

## 关键概念

### 提示和补全

补全终结点是 API 服务的核心组件。此 API 提供对模型的文本输入、文本输出接口的访问。用户只需提供一个包含英文文本命令的输入提示，模型就会生成文本补全内容。

> 提示：`""" count to 5 in a for loop """`
>
> 补全：`for i in range(1, 6): print(i)`

### 令牌

#### 文本标记

Azure OpenAI 通过将文本分解为标记来处理文本。标记可以是单词，也可以是字符块。例如，单词“hamburger”将分解为标记“ham”、“bur”和“ger”，而“pear”之类的常见短单词只是一个单个标记。许多标记以空格开头，例如“hello”和“bye”。

在给定请求中处理的标记总数取决于输入、输出和请求参数的长度。处理的标记数量也会影响模型的响应延迟和吞吐量。

#### 图像标记

Azure OpenAI 的图像处理功能结合了 GPT-4o、GPT-4o mini 和带视觉模型的 GPT-4 Turbo，通过图像词汇切分来确定图像输入所消耗的总标记数。消耗的标记数基于两个主要因素进行计算：图像的细节级别（低或高）和图像的维度。

- **低分辨率模式**
  - 低细节级别允许 API 在不需要高分辨率图像分析的场景中返回更快速的响应。
  - GPT-4o 和具有视觉的 GPT-4 Turbo：每张图像消耗固定的 85 个标记，不论图像大小。
  - GPT-4o mini：每张图像消耗固定的 2833 个标记，不论图像大小。

- **高分辨率模式**
  - 如果是高细节，则允许 API 更详细地分析图像。图像标记数根据图像的尺寸进行计算。

### 资源

Azure OpenAI 是 Azure 上的一个新产品。可以在 Azure 订阅中像使用任何其他 Azure 产品一样通过[创建资源](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/how-to/create-resource)或服务实例开始使用 Azure OpenAI。

### 部署

创建 Azure OpenAI 资源后，必须先部署模型，然后才能开始发出 API 调用和生成文本。可以使用部署 API 来完成此操作。这些 API 允许指定要使用的模型。

### 提示设计

OpenAI 的 GPT-3、GPT-3.5 和 GPT-4 模型都是基于提示的。使用基于提示的模型时，用户通过输入文本提示与模型交互，该文本提示通过文本补全进行响应。补全是模型的输入文本的延续。

虽然这些模型很强大，但它们的行为对提示也很敏感。这使得[提示工程](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/concepts/prompt-engineering)成为一项重要的开发技能。

### 模型

该服务为用户提供对多种不同模型的访问。每种模型提供不同的功能和价位。

- DALL-E 模型（有一些为预览版，请参阅[模型](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/concepts/models#dall-e)）基于用户提供的文本提示生成图像。
- Whisper 模型可用于将语音转录并翻译成文本。
- 文本转语音模型（目前为预览版）可用于将文本合成为语音。