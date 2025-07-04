<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "a0acf3093691b1cfcc008a8c6648ea26",
  "translation_date": "2025-06-13T06:38:41+00:00",
  "source_file": "03-GettingStarted/02-client/README.md",
  "language_code": "zh"
}
-->
在前面的代码中，我们：

- 导入了库
- 创建了一个客户端实例，并使用 stdio 作为传输方式连接它
- 列出了提示、资源和工具，并调用了它们

这就是一个可以与 MCP 服务器通信的客户端。

接下来，我们将在练习部分详细拆解每段代码，解释其作用。

## 练习：编写客户端

如上所述，让我们花时间详细讲解代码，如果愿意，也可以边听边编写代码。

### -1- 导入库

让我们导入所需的库，我们需要引用客户端和我们选择的传输协议 stdio。stdio 是用于在本地机器上运行的协议。SSE 是另一个传输协议，我们将在后续章节介绍，但目前我们先使用 stdio。

接下来，我们继续实例化。

### -2- 实例化客户端和传输

我们需要创建传输实例和客户端实例：

### -3- 列出服务器功能

现在我们有了一个可以连接的客户端（前提是程序已运行），但它还没有列出服务器的功能，我们接下来来实现这一点：

很好，现在我们已经捕获了所有功能。那么问题是，什么时候使用它们呢？这个客户端比较简单，意味着我们需要在想要使用功能时显式调用它们。下一章我们将创建一个更高级的客户端，它能访问自己的大型语言模型（LLM）。但现在，让我们看看如何调用服务器上的功能：

### -4- 调用功能

要调用功能，我们需要确保指定正确的参数，有时还要指定我们想调用的功能名称。

### -5- 运行客户端

在终端中输入以下命令来运行客户端：

## 任务

在本次任务中，你将运用所学创建一个属于你自己的客户端。

这里有一个服务器可供使用，你需要通过客户端代码调用它，试试看能否为服务器添加更多功能，使其更有趣。

## 解决方案

[解决方案](./solution/README.md)

## 关键要点

本章关于客户端的关键要点如下：

- 客户端既可以用来发现服务器上的功能，也可以调用它们。
- 客户端可以在启动自身的同时启动服务器（如本章所示），也可以连接到已运行的服务器。
- 客户端是测试服务器功能的好方法，除了像上一章所述的 Inspector 之外。

## 附加资源

- [在 MCP 中构建客户端](https://modelcontextprotocol.io/quickstart/client)

## 示例

- [Java 计算器](../samples/java/calculator/README.md)
- [.Net 计算器](../../../../03-GettingStarted/samples/csharp)
- [JavaScript 计算器](../samples/javascript/README.md)
- [TypeScript 计算器](../samples/typescript/README.md)
- [Python 计算器](../../../../03-GettingStarted/samples/python)

## 接下来

- 下一步：[使用 LLM 创建客户端](/03-GettingStarted/03-llm-client/README.md)

**免责声明**：  
本文件由 AI 翻译服务 [Co-op Translator](https://github.com/Azure/co-op-translator) 翻译。尽管我们力求准确，但请注意自动翻译可能包含错误或不准确之处。原始语言的原文应被视为权威来源。对于重要信息，建议使用专业人工翻译。因使用本翻译而产生的任何误解或误释，我们概不负责。