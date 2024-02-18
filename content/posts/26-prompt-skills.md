---
date: 2024-02-06
tags:
  - Prompt
title: 汇总26个提示词工程技巧
slug: 26-prompt-skills
share: true
canonicalURL: 
keywords: 
description: ""
series:
  - Prompt
lastmod: 
lang: cn
Cover:
  image: /images/post-20240111195047056.webp
author: hellloveyy
---

原论文地址：[arxiv](https://arxiv.org/abs/2312.16171)

26 个原则对比之前的提升效果图：

{{< figure src="/images/26-prompt-skills-20240206161632772.webp" caption="">}}

---


| 类别 | 编号 |
| ---- | ---- |
| 提示的结构和清晰度 | #2 #4 #8 #12 #17 #20 |
| 特异性和信息收集 | #5 #7 #13 #15 #21 #24 #25 #26 |
| 用户互动和参与 | #14 #21 |
| 内容和语言风格 | #1 #6 #9 #10 #11 #16 #18 #22 |
| 复杂任务和编码提示 | #3 #19 #23 |


---


| 编号 | 提示原则（翻译） | 示例 | 原文 |
| ---- | ---- | ---- | ---- |
| 1 | 如果您更喜欢简洁的回答，不需要对 LLM 客气，所以不需要添加诸如“请”、“如果你不介意”、“谢谢”、“我想要”等短语，直接说重点。 | ~~你能否请详细描述一下人类细胞的结构？~~ 描述人类细胞的结构。 | If you prefer more concise answers, no need to be polite with LLM so there is no need to add phrases like “please", "if you don't mind", "thank you", "I would like to", etc., and get straight to the point. |
| 2 | 在提示中整合目标受众。<br><br>例如，受众是该领域的专家。 | 为从未使用过智能手机的老年人概述智能手机是如何工作的。 | Integrate the intended audience in the prompt, e.g., the audience is an expert in the field. |
| 3 | 将复杂任务分解为一系列更简单的提示，在交互式对话中进行。 | P 1: 将以下等式中括号内的负号分配给每个项：2 x + 3 y - (4 x - 5 y) <br><br>P 2: 分别合并'x'和'y'的同类项。 <br><br>P 3: 在合并项后提供简化的表达式。 | Break down complex tasks into a sequence of simpler prompts in an interactive conversation. |
| 4 | 使用肯定的指令词如“做”，同时避免使用否定语言如“不要”。 | 如何保证建筑物在地震中的稳定性？ | Employ affirmative directives such as "do," while steering clear of negative language like "don't". |
| 5 | 当您需要澄清或更深入地了解一个主题、想法或任何信息时，使用以下提示：<br><br>- 用简单的术语解释[具体主题]。<br><br>- 像我是 11 岁一样向我解释。<br><br>- 假如我是[领域]的初学者，向我解释。<br><br>- 假如我是[领域]的专家，向我解释。<br><br>- “使用简单的英语写[文章/文本/段落]，就像你在向 5 岁的孩子解释某事一样”。 | 像我是 11 岁一样解释：加密是如何工作的？ | When you need clarity or a deeper understanding of a topic, idea, or any piece of information, utilize the following prompts:<br>- Explain [insert specific topic] in simple terms.<br>- Explain to me like I'm 11 years old.<br>- Explain to me as if I'm a beginner in [field].<br>- Explain to me as if I'm an expert in [field].<br>- "Write the [essay/text/paragraph] using simple English like you're explaining something to a 5-year-old". |
| 6 | 添加“我将支付 xxx 美元以获得更好的解决方案”。 | 我将支付 300 K 美元以获得更好的解决方案！解释动态规划的概念，并提供一个使用案例。 | Add "I'm going to tip $xxx for a better solution". |
| 7 | 实施基于示例的提示（使用少量示例提示）。 | 示例 1：将以下英语句子翻译成法语：“天空是蓝色的。”（回应：“Le ciel est bleu.”）<br><br>示例 2：将以下英语句子翻译成西班牙语：“我爱书。”（回应：“Amo los libros.”） | Implement example-driven prompting (Use few-shot prompting). |
| 8 | 在格式化提示时，先写“指令”，然后是“示例”或“问题”（如果相关），之后再提出你的内容。用一个或多个换行符分隔指令、示例、问题、上下文和输入数据。 | 指令：将给定单词从英语翻译成法语。问题： “book”的法语单词是什么？ | When formatting your prompt, start with '###Instruction###', followed by either '###Example###' or '###Question###' if relevant. Subsequently, present your content. Use one or more line breaks to separate instructions, examples, questions, context, and input data. |
| 9 | 结合使用以下短语：“你的任务是”和“你必须”。 | 你的任务是向你的朋友解释水循环。你必须使用简单的语言。 | Incorporate the following phrases: "Your task is" and "You MUST". |
| 10 | 结合使用以下短语：“你将受到惩罚”。 | 你的任务是向你的朋友解释水循环。如果你没有使用简单的语言，你将受到惩罚。 | Incorporate the following phrases: "You will be penalized". |
| 11 | 在提示中使用“以自然、类似人类的方式回答问题”的短语。 | 写一段关于健康食品的文字。以自然、类似人类的方式回答问题。 | Use the phrase "Answer a question given in a natural, human-like manner" in your prompts. |
| 12 | 使用引导词，如写“让我们一步步思考”。 | 写一个 Python 代码，循环遍历 10 个数字并将它们全部相加。让我们一步步思考。 | Use Leading words like writing "think step by step". |
| 13 | 在提示中添加以下短语“确保你的回答是无偏见的，避免依赖刻板印象。” | 文化背景如何影响对心理健康的看法？\n 确保你的回答是无偏见的，避免依赖刻板印象。 | Add to your prompt the following phrase "Ensure that your answer is unbiased and avoids relying on stereotypes." |
| 14 | 允许模型通过向你提问来获取精确的细节和需求，直到它有足够的信息来提供所需的输出（例如，“从现在开始，我希望你向我提问，直到...”）。 | 从现在开始，问我问题，直到你有足够的信息来创建一个个性化的健身计划。 | Allow the model to elicit precise details and requirements from you by asking you questions until it has enough information to provide the needed output (for example, "From now on, I would like you to ask me questions to..."). |
| 15 | 要询问一个特定的主题或想法或任何信息，并想测试你的理解，你可以使用以下短语：“教我[任何定理/主题/规则名称]，并在最后包含一个测试，但不要给我答案，然后在我回应时告诉我我的答案是否正确”。 | 教我关于 KVL 定律，并在最后包含一个测试，等我回应后让我知道我的答案是否正确，不要事先提供答案。 | To inquire about a specific topic or idea or any information and you want to test your understanding, you can use the following phrase: "Teach me the [Any theorem / topic / rule name] and include a test at the end, but don't give me the answers and then tell me if I got the answer right when I respond". |
| 16 | 为大型语言模型（LLMs）分配角色。 | 如果你是一位经济专家，你会如何回答这个问题：资本主义和社会主义经济体系之间的主要区别是什么？ | Assign a role to the Large Language Models (LLMs). |
| 17 | 使用分隔符。 | 撰写一篇有说服力的文章，讨论“可再生能源”在减少温室气体排放方面的重要性。 | Use Delimiters. |
| 18 | 在提示中重复特定单词或短语多次。 | 进化作为一个概念，已经塑造了物种的发展。进化的主要驱动因素是什么，它如何影响现代人类？ | Repeat a specific word or phrase multiple times within a prompt. |
| 19 | 将思维链（Cot）与少量示例提示结合起来。 | 示例 1：“将 10 除以 2。首先，取 10 除以 2。结果是 5。”<br><br>示例 2：“将 20 除以 4。首先，取 20 除以 4。结果是 5。”主要问题：“将 30 除以 6。首先，取 30 除以 6。结果是...？ | Combine Chain-of-thought (Cot) with few-Shot prompts. |
| 20 | 使用输出引导词，即在提示的结尾包含期望输出的开头。通过在提示的结尾包含期望响应的开始来利用输出引导词。 | 描述牛顿第一运动定律背后的原理。解释： | Use output primers, which involve concluding your prompt with the beginning of the desired output. Utilize output primers by ending your prompt with the start of the anticipated response. |
| 21 | 要写一篇[文章/文本段落/文章]或任何类型的详细文本：“为我详细写一篇关于[主题]的[文章/文本/段落]，详细添加所有必要的信息”。 | 为我详细写一段关于智能手机演进的段落，详细添加所有必要的信息。 | To write an [essay / text paragraph / article] or any type of text that should be detailed: "Write a detailed [essay / text / paragraph] for me on [topic] in detail by adding all the information necessary". |
| 22 | 要更正/改变特定文本而不改变其风格：“尝试修改用户发送的每个段落。你只应该改进用户的语法和词汇，确保它听起来自然。你应该保持原始写作风格，确保正式的段落保持正式”。 | 尝试修改用户发送的每个文本。你只应该改进用户的语法和词汇，确保它听起来自然。你应该保持原始写作风格，确保正式的段落保持正式。段落：可再生能源对我们星球的未来非常重要。它来自自然... | To correct / change specific text without changing its style: "Try to revise every paragraph sent by users. You should only improve the user’s grammar and vocabulary and make sure it sounds natural. You should maintain the original writing style, ensuring that a formal paragraph remains formal". |
| 23 | 当你有一个可能涉及不同文件的复杂编码提示时：“从现在开始，每当你生成跨越多个文件的代码时，生成一个[编程语言]脚本，可以运行以自动创建指定的文件，或对现有文件进行更改以插入生成的代码。[你的问题]。” | 生成跨越多个文件的代码，并生成一个可以运行以自动创建指定文件的 Python 脚本，用于一个包含两个基本应用的 Django 项目，用于不同功能。 | When you have a complex coding prompt that may be in different files: "From now and on whenever you generate code that spans more than one file, generate a [programming language] script that can be run to automatically create the specified files or make changes to existing files to insert the generated code. [your question]." |
| 24 | 当你想要开始或继续一段文本，使用特定的单词、短语或句子时，使用以下提示：<br><br>- 我为你提供了[歌词/故事/段落/文章...]的开头：[插入歌词/单词/句子]。根据所提供的单词完成它。保持流畅的连贯性。 | 我为你提供了一个幻想故事的开头：“神秘的山脉隐藏着无人知晓的秘密。”根据所提供的单词完成它。保持流畅的连贯性。 | When you want to initiate or continue a text using specific words, phrases, or sentences, utilize the following prompt:<br>- I'm providing you with the beginning [song lyrics / story / paragraph / essay...]: [Insert lyrics / words / sentence]. Finish it based on the words provided. Keep the flow consistent. |
| 25 | 明确说明模型必须遵循的要求，以生成内容，以关键词、规定、提示或指令的形式。 | 为海滩度假创建一个打包清单，包括以下关键词“防晒霜”、“泳衣”和“沙滩毛巾”作为必需品。 | Clearly state the requirements that the model must follow in order to produce content, in the form of keywords, regulations, hints, or instructions. |
| 26 | 要写任何文本，如文章或段落，与提供的样本类似，包括以下指令：<br><br>- “根据提供的段落[ / 标题 / 文本 / 文章 / 答案]使用相同的语言”。 | “温柔的波浪向银色的沙滩诉说着古老的故事，每个故事都是逝去时代的短暂记忆。”根据提供的文本用相同的语言描绘山与风的互动。 | To write any text, such as an essay or paragraph, that is intended to be similar to a provided sample, include the following instructions:<br>- "Use the same language based on the provided paragraph[ / title / text / essay / answer]". |

