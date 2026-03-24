# 离线安装指南 (Install Offline)

本文档适用于**无法访问互联网**的环境，需要预先下载所有依赖工具。

## 环境要求

| 组件     | 最低版本                            | 备注          |
| -------- | ----------------------------------- | ------------- |
| Node.js  | v20.10.0+                           | 运行 npx 需要 |
| 操作系统 | Windows 10+/macOS 11+/Ubuntu 20.04+ |               |

---

## 一、必选组件安装

### 1. 安装 Node.js

下载离线安装包：

- **Windows**: https://nodejs.org/dist/v20.18.1/node-v20.18.1-x64.msi
- **macOS**: https://nodejs.org/dist/v20.18.1/node-v20.18.1.pkg
- **Linux**: https://nodejs.org/dist/v20.18.1/node-v20.18.1-linux-x64.tar.xz

### 2. 安装 AionUi

从 Release 页面下载：

```
https://github.com/iOfficeAI/AionUi/releases
```

下载对应平台的安装包或 zip 便携版。

---

## 二、Agent 工具离线安装

### 方式 A: 使用 npx 预下载 (推荐)

在有网络的机器上执行以下命令，会下载到 npm 缓存目录：

```bash
# 预先下载所有 ACP Agent 包
npx --yes @zed-industries/claude-agent-acp@0.20.2 --version
npx --yes @zed-industries/codex-acp@0.9.5 --version
npx --yes @tencent-ai/codebuddy-code --version
npx --yes @qwen-code/qwen-code --version
```

然后将缓存目录复制到离线机器：

| 操作系统    | 缓存目录                    |
| ----------- | --------------------------- |
| Windows     | `%LOCALAPPDATA%\npm-cache\` |
| macOS/Linux | `~/.npm/`                   |

### 方式 B: 全局安装 CLI 工具

#### Claude Code CLI (可选，用于 MCP 功能)

```bash
# 安装 Claude Code
npm install -g @anthropic-ai/claude-code

# 验证安装
claude --version
```

#### Qwen CLI (用于 Qwen 后端)

```bash
# 安装 Qwen Code
npm install -g @qwen-code/qwen-code

# 验证安装
qwen --version
```

#### OpenCode (用于 OpenCode 后端)

```bash
# 安装 OpenCode
npm install -g opencode

# 验证安装
opencode --version
```

#### Codex (用于 Codex 后端)

从 https://github.com/sourcegraph/codex/releases 下载对应平台的二进制文件，添加到 PATH。

---

## 三、复制到离线服务器

### 需要复制的文件/目录

1. **Node.js 安装目录** 或确保离线机器已安装 Node.js
2. **npm 缓存目录** (如果使用方式 A):
   - Windows: `C:\Users\<用户名>\AppData\Local\npm-cache\`
   - macOS: `~/.npm/`
3. **全局 node_modules** (如果使用方式 B):
   - Windows: `C:\Users\<用户名>\AppData\Roaming\npm\node_modules\`
   - macOS/Linux: `~/.nvm/versions/node/*/lib/node_modules/`
4. **AionUi 安装包**

### 复制命令示例

```bash
# Windows: 复制 npm 缓存
xcopy /E /I "%LOCALAPPDATA%\npm-cache\" "\\server\share\npm-cache\"

# 离线机器恢复缓存
xcopy /E /I "\\server\share\npm-cache\" "%LOCALAPPDATA%\npm-cache\"
```

---

## 四、配置 AionUi 离线使用

### 1. 设置 npm 镜像 (可选)

如果离线机器可以访问内网 npm 镜像：

```bash
npm config set registry https://your-internal-npm-mirror.com
npm config set prefix "C:\Users\<用户名>\AppData\Roaming\npm"
```

### 2. 配置环境变量

在系统环境变量中添加：

```
NODE_PATH=C:\Users\<用户名>\AppData\Roaming\npm\node_modules
```

### 3. 离线验证

启动 AionUi 前，在命令行验证：

```bash
# 验证 Node.js
node --version

# 验证 npx 可用
npx --version

# 验证 Claude Agent ACP (如果缓存已复制)
npx --yes @zed-industries/claude-agent-acp@0.20.2 --version
```

---

## 五、各 Agent 对应关系

| AionUi 后端 | 需要的工具                         | 安装方式                |
| ----------- | ---------------------------------- | ----------------------- |
| claude      | `@zed-industries/claude-agent-acp` | npx (自动下载) 或预下载 |
| codex       | `@zed-industries/codex-acp`        | npx                     |
| codebuddy   | `@tencent-ai/codebuddy-code`       | npx                     |
| qwen        | `@qwen-code/qwen-code`             | npm install -g          |
| opencode    | opencode                           | npm install -g          |
| gemini      | 无需额外安装                       | 需要 Google API Key     |

---

## 六、离线网络方案

如果服务器完全隔离，考虑以下方案：

### 方案 1: 人工搬运

1. 在有网络的机器上下载所有依赖
2. 通过 U 盘复制到离线服务器

### 方案 2: 搭建内部 npm 镜像

使用 Verdaccio 搭建私有 npm 注册表：

```bash
# 安装 Verdaccio
npm install -g verdaccio

# 启动私有注册表
verdaccio --listen 4873
```

### 方案 3: 使用离线安装包

部分 CLI 工具提供独立的可执行文件：

- **claude-agent-acp**: https://github.com/zed-industries/claude-agent-acp/releases
- **Codex**: https://github.com/sourcegraph/codex/releases

下载后添加到 PATH 即可。

---

## 七、故障排查

### 问题 1: npx 无法下载包

```
Error: Cannot find package xxx
```

**解决方案**: 使用方式 B 全��安装，或确保 npm 缓存已复制。

### 问题 2: Node.js 版本过低

```
Node.js v18.x.x is too old for Claude ACP bridge
Minimum required: v20.10.0
```

**解决方案**: 升级 Node.js 到 v20.10.0+。

### 问题 3: 找不到 CLI 工具

```
'claude' CLI not found
```

**解决方案**:

1. 确认已安装对应 CLI
2. 检查 PATH 环境变量
3. 或在 AionUi 设置中手动指定 CLI 路径

---

## 八、快速检查清单

- [ ] Node.js v20.10.0+ 已安装
- [ ] AionUi 已安装
- [ ] npm 缓存已复制 (如果使用 npx)
- [ ] 全局 CLI 已安装 (如果使用方式 B)
- [ ] 环境变量已配置
- [ ] 离线验证通过

运行以下命令验证环境：

```bash
node --version        # 应该显示 v20.10.0+
npm --version
npx --yes @zed-industries/claude-agent-acp@0.20.2 --version
```
