---
id: protractor-migration
title: 从 Protractor 迁移
---

本教程面向正在使用 Protractor 并希望将其框架迁移到 WebdriverIO 的用户。这一教程的创建源于 Angular 团队[宣布](https://github.com/angular/protractor/issues/5502) Protractor 将不再受支持。WebdriverIO 受到了 Protractor 许多设计决策的影响，这也是为什么它可能是最接近迁移的框架。WebdriverIO 团队感谢每一位 Protractor 贡献者的工作，并希望本教程能使向 WebdriverIO 的过渡变得简单直接。

虽然我们希望有一个完全自动化的迁移过程，但现实情况却不同。每个人都有不同的设置，并以不同的方式使用 Protractor。每个步骤都应该被视为指导，而不是一步一步的说明。如果您在迁移过程中遇到问题，请随时[联系我们](https://github.com/webdriverio/codemod/discussions/new)。

## 设置

Protractor 和 WebdriverIO 的 API 实际上非常相似，以至于大多数命令可以通过[代码重构工具](https://github.com/webdriverio/codemod)自动重写。

要安装代码重构工具，请运行：

```sh
npm install jscodeshift @wdio/codemod
```

## 策略

有很多迁移策略。根据您团队的规模、测试文件数量和迁移的紧急程度，您可以尝试一次性转换所有测试或逐个文件转换。鉴于 Protractor 将继续维护到 Angular 版本 15（2022 年底），您仍有足够的时间。您可以同时运行 Protractor 和 WebdriverIO 测试，并开始用 WebdriverIO 编写新测试。根据您的时间预算，您可以先迁移重要的测试用例，然后逐步处理那些甚至可能删除的测试。

## 首先是配置文件

安装了代码重构工具后，我们可以开始转换第一个文件。首先查看[WebdriverIO 的配置选项](configuration)。配置文件可能变得非常复杂，仅转换必要部分可能更有意义，看看一旦需要特定选项的测试被迁移后，其余部分如何添加。

对于第一次迁移，我们仅转换配置文件并运行：

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/protractor ./conf.ts
```

:::info

 您的配置文件名称可能不同，但原则应该相同：首先迁移配置文件。

:::

## 安装 WebdriverIO 依赖

下一步是配置一个最小的 WebdriverIO 设置，随着我们从一个框架迁移到另一个框架，我们将开始构建它。首先通过以下命令安装 WebdriverIO CLI：

```sh
npm install --save-dev @wdio/cli
```

接下来运行配置向导：

```sh
npx wdio config
```

这将引导您回答几个问题。对于这个迁移场景，您：
- 选择默认选择
- 我们建议不要自动生成示例文件
- 为 WebdriverIO 文件选择不同的文件夹
- 选择 Mocha 而非 Jasmine

:::info 为什么选择 Mocha？
即使您之前可能使用 Protractor 和 Jasmine，但 Mocha 提供了更好的重试机制。选择权在您手中！
:::

在完成简短的问卷调查后，向导将安装所有必要的包并将它们存储在您的 `package.json` 中。

## 迁移配置文件

在我们转换了 `conf.ts` 并创建了新的 `wdio.conf.ts` 之后，现在是时候将配置从一个配置迁移到另一个配置了。确保只移植对所有测试运行必不可少的代码。在我们的示例中，我们移植了钩子函数和框架超时。

我们现在将只继续使用 `wdio.conf.ts` 文件，因此不再需要对原始 Protractor 配置进行任何更改。我们可以恢复这些更改，这样两个框架可以并行运行，我们可以一次移植一个文件。

## 迁移测试文件

我们现在准备移植第一个测试文件。为了简单起见，让我们先从没有太多第三方包依赖或其他文件（如 PageObjects）的文件开始。在我们的示例中，要迁移的第一个文件是 `first-test.spec.ts`。首先创建新的 WebdriverIO 配置期望的目录，然后移动文件：

```sh
mv mkdir -p ./test/specs/
mv test-suites/first-test.spec.ts ./test/specs
```

现在让我们转换这个文件：

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/protractor ./test/specs/first-test.spec.ts
```

就是这样！这个文件非常简单，我们不需要任何额外的更改，可以直接尝试通过以下方式运行 WebdriverIO：

```sh
npx wdio run wdio.conf.ts
```

恭喜 🥳 您刚刚迁移了第一个文件！

## 下一步

从这一点开始，您继续转换每个测试和页面对象。代码重构工具可能会对某些文件失败，并显示如下错误：

```
ERR /path/to/project/test/testdata/failing_submit.js Transformation error (Error transforming /test/testdata/failing_submit.js:2)
Error transforming /test/testdata/failing_submit.js:2

> login_form.submit()
  ^

The command "submit" is not supported in WebdriverIO. We advise to use the click command to click on the submit button instead. For more information on this configuration, see https://webdriver.io/docs/api/element/click.
  at /path/to/project/test/testdata/failing_submit.js:132:0
```

对于某些 Protractor 命令，在 WebdriverIO 中没有替代方案。在这种情况下，代码重构工具会给您一些关于如何重构的建议。如果您经常遇到这样的错误消息，请随时[提出问题](https://github.com/webdriverio/codemod/issues/new)并要求添加特定的转换。虽然代码重构工具已经转换了大部分 Protractor API，但仍有很大的改进空间。

## 结论

我们希望本教程能够指导您完成向 WebdriverIO 的迁移过程。社区继续改进代码重构工具，同时在各种组织和各种团队中测试它。如果您有反馈，请不要犹豫，[提出问题](https://github.com/webdriverio/codemod/issues/new)，或者如果您在迁移过程中遇到困难，请[开始讨论](https://github.com/webdriverio/codemod/discussions/new)。