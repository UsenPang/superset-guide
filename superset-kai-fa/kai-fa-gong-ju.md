# 开发工具

开发工具使用vscode，可以支持远程开发。如果使用远程开发需要安装Remote Development 扩展包。supserset的前端使用的是react + redux +typescript组合，可以安装插件`ES7+ React/Redux/React-Native snippets` 。后端使用python，需安装对应的python插件。

superset开发也有句法检查要求，对应前后的代码python、typescript分别需安装pylint、eslint插件。

### 安装及使用Remote Development 扩展包

* 打开 VSCode。
* 点击左侧活动栏中的扩展图标（或按 `Ctrl+Shift+X`）。
* 在搜索栏中输入 `Remote Development`。
* 找到 `Remote Development` 扩展包，并点击 `Install`

#### 使用 Remote - SSH 插件

**设置 SSH 主机**

* 打开 VSCode。
* 按 `Ctrl+Shift+P` 打开命令面板。
* 输入并选择 `Remote-SSH: Connect to Host...`。
* 如果这是你第一次使用该插件，系统会提示你添加 SSH 主机。点击 `Add New SSH Host...`。
* 输入 SSH 连接字符串，例如 `user@hostname`，然后选择要存储的 SSH 配置文件（通常是 `~/.ssh/config`）。

修改配置文件，可以参考如下配置：

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

**连接到远程主机**

* 在命令面板中选择 `Remote-SSH: Connect to Host...`。
* 选择刚刚添加的主机。
* 系统会提示你输入 SSH 密码或使用 SSH 密钥进行身份验证。
* 成功连接后，你会在 VSCode 的远程窗口中看到远程文件系统。



### 安装 ESLint 扩展

#### 安装

首先，确保你已经在 VSCode 中安装了 ESLint 扩展。你可以在 VSCode 的扩展市场中搜索 “ESLint”，然后点击安装。

#### 编辑 `settings.json` 文件

在 VSCode 中打开你的工作区设置。你可以通过以下几种方法之一来打开设置：

* 通过菜单导航：点击 **File > Preferences > Settings**（Windows 和 Linux）或 **Code > Preferences > Settings**（macOS）。
* 使用快捷键：按 `Ctrl+,`（Windows 和 Linux）或 `Cmd+,`（macOS）。

在 `settings.json` 文件中，添加以下配置来指定 ESLint 的工作目录为 `superset-frontend`：

```json
{
  "eslint.workingDirectories": [
    "superset-frontend"
  ]
}
```



### 安装 PyLint 扩展

