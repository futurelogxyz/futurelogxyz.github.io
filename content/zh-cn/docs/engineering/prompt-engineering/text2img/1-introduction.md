---
title: 1. 简介
weight: 1
prev: /docs/getting-started
next: /docs/guide/organize-files
sidebar:
  open: false
---



## 1 什么是 Text2Img Prompt Engineering?
借用 [OpenArt Prompt Book](https://openart.ai/promptbook) 中的定义：
> Text2Img Prompt engineering is the process of structuring words that can be interpreted and understood by a text-to-image model. Think of it as the language you need to speak in order to tell an AI model what to draw.

> Prompt engineering is a great way to stretch the limitations of Text2IM models. Good prompts can make images go from good to great.


## 2. Text2Img Prompt Engineering 有什么不同?
Text-to-Image 模型的 Prompt 也是一段文本，描述了你想要生成的图片的内容。比如，你可以输入 `a dog on the grass ground`，然后 Text-to-Image 模型就会生成一张狗在草地上的图片。

{{< figure src="https://bytememo-1251943639.file.myqcloud.com/img/20230713/image_D2xkxEWe_1689237854709_raw.jpg" caption="" ratio="1x1" class="rounded" >}}

但需要注意的是，适用于 Stable Diffusion 等 Text-to-Image 模型（如 DALLE，Midjourney 等）的 Prompt 与 LLM 模型的 Prompt 存在较大的不同。 

LLM 模型的 Prompt 通常是包含完整的语法结构的语句，而 Text-to-Image 模型的 Prompt 通常是由名词或形容词短语构成，重点在于描述清楚什么东西以什么样的形式或风格等出现在图片上，而不需要用一个完整的句子，Text-to-Image 模型通常也无法理解解析句子结构。
并且，Text-to-Image 模型的 Prompt 中的词汇对于大部分人来说都是较为陌生的，你可能得是一个摄影师，一个画家，甚至得选修过艺术史课程才能理解各个词汇所代表的含义。

因此，我们将通过介绍 Text-to-Image 模型 Prompt 的构成、各个构成部分相关词汇、部分生成控制参数 以及 相关工具 等，便于我们能快速方便地上手使用 Text-to-Image 模型，体验 AI 的异想世界。


{{< callout type="info" >}}
我们介绍的参数和示例仅在 Stable Diffusion 模型上测试通过，但大部分技术适用于所有 Text-to-Image 模型，暂没有列出或说明不同模型的差异。
{{< /callout >}}