# 盘点那些前端项目上的规范工具

czpcalm [高级前端进阶](javascript:void(0);) *2023-04-25 09:10* *发表于浙江*

规范化是前端工程化的一个重要部分。现在，有许多工具能够辅助我们实行代码的规范化,比如你一定知道的 ESLint 和 Prettier。

今天，来聊聊这些工具的工作原理和基本使用，了解它们是如何发挥作用的，以及如何更好地利用这些工具去规范项目的代码。

本文主要聊聊这些工具的作用和基本使用方式，不会有细致的使用步骤和教程，这些内容我希望你能从官方指引中查看。

## 1. ESlint - 检查你的 JavaScript 代码

让我们先从知名度最高的 **ESLint**[1] 开始。

### 1.1. ESLint 及其作用

Lint 是一类专门用于检查代码的工具软件， 也称 linter。ESLint ，即 JavaScript （ECMAScript）代码的检查工具。

正如官网的介绍 —— “Find and fix problems in your JavaScript code”，ESLint 能够辅助查找出你的 JavaScript 代码中的问题，包括：

- 代码风格问题（styles）。比如，运算符两边的空格、语句末尾的分号。
- 不好的写法。比如，使用 `==` 进行比较而不是 `===`。
- 可能存在逻辑问题的代码模式。比如，定义了一个变量，但没有使用到它。

此外，ESLint 还能够帮你自动修复一些简单的问题。

我们将在下一小结学习如何使用 ESLint 检查我们的 JavaScript 代码，并修复其中的一些问题。

### 1.2. ESLint 快速上手

为了在项目中使用 ESLint，需要先安装它。

```
# 初始化一个 npm 项目
mkdir eslint-test
cd eslint-test
npm init -y
# 安装 eslint
npm init @eslint/config
复制代码
```

回答一系列问题后，你可以看目录中的配置文件 `.eslintrc.js`，这个配置文件告诉 ESLint 如何去解析项目，这个项目采用了哪些规范和规则。

ESLint 是一个高度配置化的工具。尤其需要留意 `extends` 和 `rules` 字段，它们定义了在项目中采用哪些规则。一段代码有没有问题，取决于项目中应用了哪些规则。比如：`{semi: ["error", "always"]}` 要求必须在语句末尾添加分号，相反，`semi: ["error", "never"]` 禁止任何不必要的分号。

具体的配置教程可以参考**官方配置文档**[2]，不是这里三两句能说完的。

在这里，我使用的是 **airbnb-base**[3] 规范。

```
module.exports{
  // ...
  extends: 'airbnb-base'
  // ...
}
复制代码
```

写一段简单的 JavaScript 代码用于测试：

```
// file - add.js
function add(x,y) {
  return Number(x)+Number(y);
}

// ❌ 这是一个错误的调用
add("2"+'3')
复制代码
```

ESLint 是一个命令行工具，这个命令调用之前安装的 ESLint 去检查 `add.js` 代码：

```
npx eslint add.js
复制代码
```

输出结果：

```
eslint-test/add.js
  1:15  error  A space is required after ','                comma-spacing
  2:19  error  Operator '+' must be spaced                  space-infix-ops
  5:5   error  Strings must use singlequote                 quotes
  5:8   error  Operator '+' must be spaced                  space-infix-ops
  5:8   error  Unexpected string concatenation of literals  no-useless-concat

✖ 5 problems (5 errors, 0 warnings)
  4 errors and 0 warnings potentially fixable with the `--fix` option.
复制代码
```

可以看到，ESLint 发现了代码中的 5 个 error 等级的问题，并且提示其中的 4 个问题是**可修复**的。根据提示，执行：

```
npx eslint add.js --fix
复制代码
```

可以看到，现在提示只有一个问题：

```
eslint-test/add.js
  5:9  error  Unexpected string concatenation of literals  no-useless-concat

✖ 1 problem (1 error, 0 warnings)
复制代码
```

重新查看 `add.js` 文件，可以看到 4 个格式化问题已经被自动修复了。检查第 5 行 `add('2' + '3');`, 发现正确写法应该是 `add('2', '3');`。

很好，所有的问题都解决了，ESLint 可帮了大忙 👍 。

尽管在实际中，我们很少直接调用 `eslint` 命令，更多是配合编辑器和一些工作流工具使用（后面小结内容）。不过，记住 ESLint 原本是一个命令行工具，对我们理解它的工作原理很有帮助。

### 1.3. 配合编辑器使用的 ESLint

上一节中，我们是在命令行下使用 ESLint，并从命令行的输出中看到代码中的问题。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/H8M5QJDxMHoFicNib23I0fA3EtTyDUaoWHlL7L4SldtTxXgUmgwr2K1U97MdTZEkQrrOpcOXWBfMc7rexjvyOowA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)image.png

许多人是从编辑器里接触 ESLint 的。的确，当 ESLint 与编辑器配合工作时，它的威力才真正显现出来。

以 VSCode 为例，在 VSCode 上使用 ESLint 需要安装 **ESLint 插件**[4]。启用插件后，可以在编辑代码的同时看到哪些代码有问题，及时发现，及时修复。使用鼠标 hover 红线，或者在下方的 PROBLEMS 面板中都可以看到具体的错误提示。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/H8M5QJDxMHoFicNib23I0fA3EtTyDUaoWHAh9rib1SHxiaVUxgicYohm5jeFZ0RypLovJicPM6CnOUkgNrLlEHXch9FA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)Awesome！不过，问题来了：

1. 插件做了什么，这种功能怎么实现的？
2. 装了插件还需要在项目里安装 ESLint 吗？
3. 不同的项目中使用的 ESLint 版本和配置的规则不同，会发生冲突吗？

**回答：**

1. 插件原理：插件我们敲码的时候，在后台自动执行 `eslint` 命令分析代码，并根据结果实时回显到编辑器中。
2. 需要每个项目内安装 ESLint。看插件的文档：

> The extension uses the ESLint library installed in the opened workspace folder. If the folder doesn't provide one the extension looks for a global install version.
>
> 插件使用当前项目目录中安装的 ESLint 库。如果目录中没有安装，则尝试使用全局安装的。

说明，插件并不包含 ESLint 核心库，而是尝试读取本地或全局安装的 ESLint，并使用查找读取项目内的 `eslintrc.*` 配置文件。

> 💡 如果插件无法正确读取项目中的 ESLint 程序和配置，会导致插件启动失败。这个问题常常是因为 ESLint 没有安装在打开目录的根部。可以通过修改**插件的配置**[5]解决。

1. 因为插件始终使用工作目录中的 ESLint 程序和配置，当使用本地安装时，每个项目都是独立的，不会冲突。

### 1.4. 启用编辑器的保存自动修复功能

编辑器还有一个强大的功能，可以在保存时，自动修复那些支持自动修复的问题，不用执行额外的 `eslint \--fix files` 命令， **强烈推荐开启**。

在 VSCode，你可能需要添加设置来启用自动修复功能：

```
{
  // ...
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
复制代码
```

WebStorm 用户可以参考**这里**[6]设置。

启用后，就几乎不用手动执行 `eslint` 命令了。

## 2. Prettier

另一个广为人知的工具是 **Prettier**[7]，它是一个代码格式化工具。

### 2.1. 使用 Prettier

当你了解了 ESLint 的使用后，自然你也能理解 Prettier 的使用。

Prettier 同样支持配置，不过相对简单，同样可以通过命令行执行，同样可以有编辑器插件可以和编辑器配合，具体可以参考官方文档。

### 2.2. Prettier vs ESLint

你可能好奇 Prettier 和 ESLint 有什么区别？

首先，虽然它们都会对代码 AST（语法树）进行检查，但 Prettier 只会进行语法分析，只能检查并归正代码的**格式问题**，而 ESLint 还会进一步对代码进行语义分析，能发现**格式问题和代码模式问题**。比如，用 let 声明了一个变量，但是这个变量在后面并没有被重新赋值，因为没有格式问题，Prettier 会通过，而 ESLint 则能发现这里应该使用 const 声明更好。

既然 ESLint 也能做格式化工具，那为什么还需要 Prettier？因为 ESLint 只能检查 JavaScript 代码以及 TypeScript、JSX 等衍生代码（需配置解析器），无法检查项目中的 CSS、HTML 等代码。Prettier 则天然支持对大多数项目文件的格式化，包括 JSX、Vue、TypeScript、CSS、HTML、JSON、Markdown、YAML 等。

### 2.3. 同时使用 Prettier 和 ESLint

从上面可以看出，在 JavaScript 及其衍生语言的格式化上，ESLint 和 Prettier 是有重合的。所以，在实际运用中，我们需要保证这些文件只会采用其中一种进行格式化，避免不必要的格式化。更遭的情况是启用了两个，而且两个工具的风格配置互相冲突。我就曾在项目中遇到这种情况，ESLint 格式化之后，Prettier 也执行格式化，结果 ESLint 检查还是不通过。

我推荐在 JavaScript 中也使用 Prettier。因为 ESLint 需要我们把风格配置成 error 等级，才会支持自动修复。但是，这样，一旦有格式问题，编辑器就会标红，很烦人，强迫症受不了，而 Prettier 不会有。下图是一段只有风格问题的代码在分别启用这两种工具时的编辑器显示。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/H8M5QJDxMHoFicNib23I0fA3EtTyDUaoWHlM0lXPs4k4xn70vl9B2w7mZicAicJcfh28VwMxU3ZU1l5t377WLibxJPg/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1)image.png

要同时使用二者，就需要关闭 ESLint 中可能和 Prettier 冲突的规则，而这就是 **eslint-config-prettier**[8] 所做的事。所以，想同时使用两者，你需要在 ESLint 中使用该配置，具体的配置方式参考使用文档即可。

如果你只想在 JavaScript 中使用 ESLint，可以在 `.prettierignore` 中忽略所以的 JavaScript 文件即可。

## 3. Stylelint

顾名思义，**Stylelint**[9] 就是样式代码的 linter。

> A mighty CSS linter that helps you avoid errors and enforce conventions.

它的功能和使用，都和 ESLint 类似，不过作用目标不同而已。

和 Prettier 的区别在于，它和 ESLint 一样，是一个 linter，会进行语义分析，能发现一些模式问题。在 Stylelint 15 之前，如果同时使用 Stylelint 和 Prettier，也需要使用 **stylelint-config-prettier**[10] 避免在样式文件上的规则冲突。Stylelint 15 废弃了所有风格规则，不会和 Prettier 冲突。

## 4. 工作流工具

将 linter/formatter 与一些工作流工具配合，能够实现团队规范的自动化。

规范化原则是：**越早发现不规范的代码，改正的成本越低。**

### 4.1. 编码时：编辑器支持

从编码开始，就推荐你启用编辑器的代码检查功能，这可能是需要插件或者设置来实现。同时，建议开启保存时自动格式化，这能持续保证你的代码是符合规范的。

### 4.2. 提交时：Git hooks + lint-staged

Git pre-commit hook 可以让我们在提交之前执行一些命令，利用这点，可以在提交前对代码执行代码的 lint 检查和格式化，自动修复一些可以修复的问题，如果有不可自动修复的问题，取消本次提交，从而，避免不规范的代码被提交到代码仓库。

默认的 Git hook 不容易设置，社区中流行使用 **husky**[11] 进行配置。

每次提交时的检查应该是针对当前 commit 内修改的内容，而不是全部文件，也就是只检查暂存区内的文件。**🚫💩lint-staged**[12]可以实现这种效果。

具体的配置方式请参阅这两个工具的文档。

### 4.3. 交付时：CI 集成

pre-commit 检查可以通过 `git commit \--no-verify` 跳过，导致一些不规范的代码被 push 到代码仓库。

可以通过 Gitlab-CI 或 Github Action 这类 CI 工具集成代码检查，在代码被 push 到远程仓库，或者 merge request 发起时，执行特定的任务对代码发起检查。

值得一提的是，CI 集成可以是整个代码仓库统一的，这样，可以实现公司层面的所有项目统一标准，而不只是基于团队和项目。

## 5. 其它工具

### 5.1. commitlint：规范提交消息

**commitlint \- Lint commit messages**[13] 是规范提交消息（commit message）的一个工具，可以避免有些小伙伴就喜欢 `git commit \-m "u"`，要求一个提交消息符合**Conventional Commits**[14]规范，即以下形式：

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
复制代码
```

commitlint 本身是一个命令行工具，用于判断一个消息是否符合规范。

```
# Lint from stdin
echo 'foo: bar' | commitlint
⧗   input: foo: bar
✖   type must be one of [build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test] [type-enum]

✖   found 1 problems, 0 warnings
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint
复制代码
```

为了实现在 commit 时检查消息，需要和 git commit-msg hook 配合。配合上面提到的 husky：

```
npx husky add .husky/commit-msg  'npx --no -- commitlint --edit ${1}'
复制代码
```

### 5.2. branch-name-lint: 规范分支名

如果希望规范团队项目中的分支名称，可以使用**branch-name-lint**[15]。

一般地，项目只需要规范成员 push 到远程仓库的分支名称符合规范即可，不限制本地分支。按这个思路，可以利用 git pre-push hook：

```
npx husky add .husky/pre-push  'npx --no -- branch-name-lint <your-configuration.json>'
复制代码
```

### 5.3. EditorConfig: 统一编辑器配置

**EditorConfig**[16]是一个跨编辑器/IDE 的编辑器配置工具。与 linter 和 formatter 不同，它的作用对象不是代码，而是去约束编辑器的配置，比如缩进风格是 tab 还是空格，缩进多少个字符。同时也说明，它是作用于文件级别的，对代码语法是无感的，对所有文件都生效。

许多编辑器都支持 EditorConfig，有的可能需要安装插件，不需要项目安装什么依赖包，只需要在根目录下创建一个 `.editorconfig` 文件。

EditorConfig 适用于团队成员中使用不同编辑器的场景。当所有编辑器都使用项目根目录下的 `.editorconfig` 文件作为配置时，能一定程度上保持代码文件的统一性。

## 6. 总结

前端工程化的常见代码规范工具有：

- ESLint: 一个用于 JavaScript、JSX 等语言的可配置的代码检查工具。
- Stylelint: 一个 CSS/Less/Sass 等样式代码的 linter。
- Prettier: 支持多种语言的代码格式化工具。
- Husky: 一个流行的用于配置 git hooks 的工具。
- lint-staged: 对提交到暂存区的文件进行检查的工具。
- EditorConfig: 统一不同编辑器配置的工具。
- commitlint: 检查提交消息是否符合规范。
- branch-name-lint: 检查代码分支是否符合规范。

在使用中，要善于利用编辑器、git hooks、CI 工具来自动化执行代码检查和格式化。

最后，谨记，工具虽好，但**不要一把梭**，需要根据团队情况和项目情况选择必要的几个即可。实践中，ESLint + Prettier + Husky + lint-staged 是大部分人的选择。如果过分使用，会引入一些不必要的工作内容和理解成本。



关于本文

# 作者：czpcalm

https://juejin.cn/post/7218915344131850295