# npm 讲解与使用手册

> 本手册面向初学者，系统讲解 npm 的核心概念与日常用法，并以"发布你的第一个包"为实战目标，帮助你从零开始掌握 Node.js 生态中最重要的工具。

## 一、认识 npm

### 1.1 什么是 npm

npm（Node Package Manager，Node 包管理器）是 Node.js 的默认包管理工具。它让你可以轻松地安装、共享和管理 JavaScript 代码包，是目前世界上最大的开源软件注册表之一，拥有数百万个开源包。

npm 由三个部分组成：

- **命令行工具（CLI）**：开发者在终端中使用的 `npm` 命令，用于安装、发布和管理包。
- **包注册表（Registry）**：存放所有公开包的中央数据库，默认地址为 `registry.npmjs.org`。
- **官方网站**：用于搜索包、查看文档和管理账号的网页界面，地址为 `www.npmjs.com`。

### 1.2 npm 与 Node.js 的关系

npm 随 Node.js 一起安装：只要安装了 Node.js，就自动拥有了 npm。两者的版本号相互独立，但通常保持配套更新。在开始学习之前，请先确认本机已正确安装（见第二章）。

## 二、环境搭建与基础配置

### 2.1 安装与验证

前往 Node.js 官网（`nodejs.org`）下载并安装 LTS（长期支持）版本。安装完成后，打开终端执行以下命令验证：

```bash
node -v    # 查看 Node.js 版本，例如 v22.14.0
npm -v     # 查看 npm 版本，例如 10.9.2
```

若能正常输出版本号，说明环境已就绪。

### 2.2 升级 npm

npm 本身也是一个包，可以用它自己来升级自己：

```bash
npm install -g npm
```

### 2.3 配置镜像源（可选）

npm 默认从官方注册表下载包。如果网络环境导致下载缓慢，可以切换到国内镜像加速：

```bash
npm config set registry https://registry.npmmirror.com   # 切换镜像源
npm config get registry                                   # 查看当前使用的源
```

## 三、package.json：项目的"身份证"

### 3.1 初始化项目

每个 npm 项目都有一个 `package.json` 文件，它记录了项目的名称、版本、依赖等核心信息，是项目的"身份证"。创建方式如下：

```bash
mkdir my-project    # 新建项目目录
cd my-project       # 进入目录
npm init            # 交互式填写信息，生成 package.json
npm init -y         # 或者直接接受所有默认值，快速生成
```

### 3.2 核心字段说明

| 字段 | 含义 | 示例 |
| ---- | ---- | ---- |
| name | 包名，发布时须全局唯一 | `my-first-package` |
| version | 版本号，遵循语义化版本规范 | `1.0.0` |
| description | 包的简要描述 | `A greeting utility` |
| main | 包的入口文件 | `index.js` |
| scripts | 自定义脚本命令 | `"test": "node test.js"` |
| dependencies | 生产环境依赖 | `"lodash": "^4.17.21"` |
| devDependencies | 开发环境依赖 | `"jest": "^29.7.0"` |
| license | 开源许可证 | `MIT` |

## 四、安装与管理依赖

### 4.1 安装依赖包

安装依赖是 npm 最常用的功能：

```bash
npm install <包名>              # 安装包，并写入 dependencies
npm install <包名> --save-dev   # 安装为开发依赖（仅开发时需要）
npm install <包名> -g           # 全局安装（命令行工具常用）
npm install                     # 按 package.json 一次性安装全部依赖
```

### 4.2 语义化版本（SemVer）

npm 的版本号遵循"主版本号.次版本号.修订号"的语义化规范。在 `package.json` 中，版本号前可以加符号表示允许的更新范围：

| 写法 | 含义 |
| ---- | ---- |
| `1.0.0` | 只使用这个精确版本 |
| `^1.0.0` | 允许主版本不变的最新版本（1.x.x 范围内最新） |
| `~1.0.0` | 允许次版本不变的最新版本（1.0.x 范围内最新） |
| `*` | 任意版本 |

### 4.3 package-lock.json

执行安装后，项目根目录会自动生成 `package-lock.json` 文件。它精确锁定了每个依赖（包括间接依赖）的实际安装版本，保证团队所有成员和部署环境装出完全一致的依赖树。**请将此文件提交到代码仓库，不要删除。**

## 五、常用命令速查

| 命令 | 作用说明 |
| ---- | ---- |
| `npm init` / `npm init -y` | 初始化 package.json |
| `npm install <包名>` | 安装包并写入生产依赖 |
| `npm install <包名> --save-dev` | 安装为开发依赖 |
| `npm install` | 根据 package.json 安装全部依赖 |
| `npm uninstall <包名>` | 卸载包 |
| `npm update` | 更新依赖到符合范围的最新版本 |
| `npm run <脚本名>` | 执行 package.json 中定义的脚本 |
| `npm list` | 查看当前项目的依赖树 |
| `npm info <包名>` | 查看注册表中某个包的详细信息 |
| `npm login` / `npm logout` | 登录 / 退出 npm 账号 |
| `npm publish` | 发布当前包 |

## 六、使用 npx 执行命令

`npx` 是随 npm 5.2+ 一起发布的命令执行器。它可以直接运行包中的命令而无需全局安装，特别适合临时使用某个工具：

```bash
npx create-react-app my-app   # 无需全局安装即可运行脚手架
npx cowsay hello              # 临时下载、运行、用完即弃
```

## 七、实战：发布你的第一个 npm 包

### 第一步：准备工作

首先，你需要在 npm 官方注册一个账号。

1. **注册账号**：打开浏览器，访问 npm 官网，填写用户名、邮箱和密码完成注册。
2. **验证邮箱**：注册后，npm 会发一封验证邮件到你的邮箱，点击邮件中的链接完成验证。**这一步很重要，未验证的邮箱无法发布包。**

### 第二步：创建你的包

在你的电脑上创建一个新目录，这将是你的项目根目录。

**新建目录并进入：**

```bash
mkdir my-first-package
cd my-first-package
```

**初始化项目：** 运行 `npm init` 命令创建 `package.json` 文件。按提示填写信息，其中**包名（name）至关重要，它必须是全局唯一的**。

**包名被占用怎么办？** 你可以使用作用域（Scope）来解决。作用域就像是你的"命名空间"，格式是 `@你的用户名/包名`。例如，如果你的用户名是 wangr，可以将包名设为 `@wangr/my-first-package`。用这种方式创建包时，发布命令稍有不同（见第四步）。

### 第三步：编写代码

现在，你可以开始写你的代码了。创建入口文件（比如 `index.js`），并实现你的功能：

```javascript
// index.js
function greet(name) {
    return `Hello, ${name}!`;
}

module.exports = greet;
```

### 第四步：登录并发布

这是最激动人心的一步，你的代码即将被全世界看到。

**登录 npm：** 在终端中执行 `npm login` 命令，然后输入你的用户名、密码和邮箱。

```bash
npm login
```

**发布前检查（强烈推荐）：** 在真正发布前，先"演习"一下，看看哪些文件会被打包上传：

```bash
npm publish --dry-run
```

仔细检查列出的文件列表，确保没有包含敏感信息（如 `.env` 文件、密码、私钥等）。

**执行发布：** 一切就绪后，运行发布命令：

```bash
npm publish
```

如果你使用了作用域包（如 `@wangr/my-first-package`），首次发布时需要指定访问权限为公开，因为带作用域的包默认是私有的：

```bash
npm publish --access public
```

**验证发布：** 发布成功后，你就可以在 npm 官网搜索到你的包了，地址通常是 `www.npmjs.com/package/你的包名`。

### 第五步：更新你的包

当你修复了 Bug 或添加了新功能，需要发布新版本。**每次更新都必须升级版本号。**

| 命令 | 升级类型 | 版本变化示例 | 适用场景 |
| ---- | ---- | ---- | ---- |
| `npm version patch` | 修订号 +1 | 1.0.0 → 1.0.1 | 修复 Bug |
| `npm version minor` | 次版本号 +1 | 1.0.0 → 1.1.0 | 新增功能（向下兼容） |
| `npm version major` | 主版本号 +1 | 1.0.0 → 2.0.0 | 重大变更（不向下兼容） |

执行后，`package.json` 中的版本号会自动更新，然后再运行 `npm publish` 即可。

## 八、新手常见"坑"与提示

1. **403 Forbidden 错误**：通常是未登录、包名已被占用，或作用域包忘记使用 `--access public`。先执行 `npm login`，再检查包名和发布参数。
2. **版本号未更新**：每次发布，`package.json` 中的 version 字段都必须比上一个版本高，否则会发布失败。
3. **上传了多余文件**：使用 `package.json` 的 `files` 字段或 `.npmignore` 文件来控制哪些文件被发布，避免将测试文件或本地配置一并上传。例如：

```json
"files": ["index.js", "lib/"]
```

4. **自动化发布**：你可以设置 GitHub Actions，在推送代码到 GitHub 时自动完成版本更新和发布。这对于团队协作和持续集成非常有用。

## 九、常见问题速查

| 问题现象 | 常见原因 | 解决办法 |
| ---- | ---- | ---- |
| 发布时报 403 Forbidden | 未登录 / 包名被占用 / 作用域包未公开 | 先 `npm login`；更换包名或使用作用域；发布时加 `--access public` |
| 提示版本号已存在 | 忘记升级版本号 | 执行 `npm version` 升级后重新发布 |
| 安装依赖速度慢、超时 | 网络原因 | 切换镜像源（见 2.3 节） |
| 发布后搜不到包 | 搜索索引有延迟 | 稍等几分钟，直接访问包页面确认 |
| 安装时提示依赖冲突 | 多个包依赖的版本互不兼容 | 使用 `npm ls <包名>` 定位冲突来源，调整版本范围 |

## 十、写在最后

npm 的学习曲线并不陡峭：日常开发中，`init`、`install`、`run`、`publish` 这几个命令就能覆盖绝大多数场景。建议你现在就动手创建第一个包，把第七章的流程完整走一遍——发布成功的成就感，会是你继续深入 Node.js 生态的最好动力。
