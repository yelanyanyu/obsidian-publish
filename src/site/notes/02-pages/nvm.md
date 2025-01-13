---
{"dg-publish":true,"permalink":"/02-pages/nvm/","tags":["personal/blog","program/frontend"]}
---

最常用的管理 Node. js 版本的工具是 **nvm（Node Version Manager）**。`nvm` 是一个允许你在同一台机器上安装和管理多个 Node. js 版本的命令行工具，适用于 Unix 系统（如 macOS 和 Linux）。对于 Windows 用户，还有 `nvm-windows` 这一类似的工具。

### 一、nvm（Node Version Manager）

#### 1. 安装 nvm

在 Unix 系统上安装 `nvm` 通常通过运行提供的安装脚本完成。以下是安装步骤：

**使用 cURL 安装：**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

**使用 Wget 安装：**
```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

安装完成后，重新加载终端配置文件（例如 `.bashrc`、`.zshrc`）：
```bash
source ~/.bashrc
```
或
```bash
source ~/.zshrc
```

**验证安装：**
```bash
nvm --version
```

#### 2. 使用 nvm 管理 Node. js 版本

**安装特定版本的 Node. js：**
```bash
nvm install 14.17.0
```

**安装最新的 LTS（长期支持）版本：**
```bash
nvm install --lts
```

**查看已安装的 Node. js 版本：**
```bash
nvm ls
```

**查看可用安装的 Node. js 版本：**
```bash
nvm ls-remote
```

**切换到指定版本的 Node. js：**
```bash
nvm use 14.17.0
```

**设置默认的 Node. js 版本（新开的终端会自动使用该版本）：**
```bash
nvm alias default 14.17.0
```

**卸载某个版本的 Node. js：**
```bash
nvm uninstall 14.17.0
```

#### 3. 使用实例

**示例 1：安装并使用最新的 Node. js 版本**
```bash
nvm install node
nvm use node
node -v
```

**示例 2：安装多个版本并切换**
```bash
nvm install 12.22.1
nvm install 16.13.0
nvm ls
nvm use 12.22.1
node -v
nvm use 16.13.0
node -v
```

**示例 3：设置默认的 Node. js 版本**
```bash
nvm install 14.17.0
nvm alias default 14.17.0
```

### 二、nvm-windows

对于 Windows 用户，`nvm` 原版不兼容，可以使用 `nvm-windows`。其安装和使用方法稍有不同。

#### 1. 安装 nvm-windows

1. 访问 [nvm-windows 的 GitHub 页面](https://github.com/coreybutler/nvm-windows/releases)。
2. 下载最新版本的安装程序（如 `nvm-setup.exe`）。
3. 运行安装程序并按照提示完成安装。

#### 2. 使用 nvm-windows 管理 Node. js 版本

**安装特定版本的 Node. js：**
```cmd
nvm install 14.17.0
```

**查看已安装的 Node. js 版本：**
```cmd
nvm list
```

**切换到指定版本的 Node. js：**
```cmd
nvm use 14.17.0
```

**卸载某个版本的 Node. js：**
```cmd
nvm uninstall 14.17.0
```

#### 3. 使用实例

**示例 1：安装并使用特定版本**
```cmd
nvm install 16.13.0
nvm use 16.13.0
node -v
```

**示例 2：查看所有已安装的版本**
```cmd
nvm list
```

**示例 3：设置默认版本**
```cmd
nvm use 14.17.0
```

### 三、其他 Node. js 版本管理工具

除了 `nvm` 和 `nvm-windows`，还有一些其他的 Node. js 版本管理工具，如：

1. **n**：一个简单的 Node. js 版本管理工具，适用于 Unix 系统。安装和使用相对简单。
   - **安装：**
     ```bash
     npm install -g n
     ```
   - **使用：**
     ```bash
     n latest
     n lts
     n 14.17.0
     ```

2. **asdf**：一个通用版本管理器，支持多种语言，包括 Node. js。
   - **安装 asdf：**
     请参考 [asdf 的官方文档](https://asdf-vm.com/)。
   - **安装 Node. js 插件并管理版本：**
     ```bash
     asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git
     asdf install nodejs 14.17.0
     asdf global nodejs 14.17.0
     ```

### 四、总结

**nvm** 是最常用且功能强大的 Node. js 版本管理工具，适用于 Unix 系统，通过简单的命令即可安装、切换和管理多个 Node. js 版本。对于 Windows 用户，`nvm-windows` 提供了类似的功能。根据不同的开发环境和需求，开发者可以选择最适合自己的版本管理工具，以提升开发效率和环境的灵活性。