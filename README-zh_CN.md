# 生成式 AI 课程

这是市面上最全面的生成式 AI 课程之一。

我们将从最基本的原则讲起——什么是人工智能（AI）、它是如何被发明的，以及大型语言模型（LLM）的崛起。然后，我们将进入动手实践环节，包括构建基础聊天机器人、实现检索增强生成（RAG）、创建 AI 代理（Agents）、探索模型上下文协议（MCP），并深入理解构建生产级 AI 应用所需的知识。

我曾与数百名客户合作，深知构建可扩展、真实世界的 AI 解决方案需要什么。本课程没有行话，没有废话——一切都从头开始解释。市面上大多数 AI 课程要么令人困惑，要么过于学术化，要么缺乏实践意义。这门课则与众不同。

我们正处在一个 AI 应用需求爆炸性增长的时代——而入门门槛却前所未有地低。这就是为什么 **AI 工程**——在强大、现成的模型之上构建应用的过程——是技 ��� 领域发展最快的学科之一。

在机器学习模型之上构建应用并非新鲜事。早在 LLM 出现之前，我们就已经在为欺诈检测、产品推荐等构建 AI 应用。许多原则仍然适用，但 LLM 带来了新的挑战和机遇。

**传统机器学习与 AI 工程的主要区别：**

1. AI 工程更侧重于适配模型（如提示工程、微调），而不是训练模型。
2. AI 工程处理的是需要更多计算资源的大型模型——这意味着更高的延迟和不同的基础设施需求。
3. AI 模型通常产生开放式输出，使得评估比传统机器学习更复杂。

**课程内容概览：**

- 部署本地 LLM
- 构建端到端的 AI 聊天机器人并管理上下文
- 提示工程（Prompt Engineering）
- 防御性提示与防止常见的 AI 攻击
- 检索增强生成（Retrieval-Augmented Generation, RAG）
- AI 代理（Agents）及其高级用例
- 模型上下文协议（Model Context Protocol, MCP）
- 大语言模型运维（LLMOps）
- 优质 AI 训练数据的标准

---

## 环境设置指南

请按照以下步骤设置您的开发环境，以便运行实验练习。

### 第 1 步：安装 Visual Studio Code

1.  访问 [https://code.visualstudio.com](https://code.visualstudio.com)
2.  根据您的操作系统，点击蓝色的“Download”按钮：
    - **Windows**: 点击 "Download for Windows"
    - **Mac**: 点击 "Download for Mac"
    - **Linux**: 点击 "Download for Linux"
3.  运行下载的安装程序，并按照提示完成安装。
4.  安装完成后打开 VS Code。

### 第 2 步：安装 Git

**Windows 系统：**

1.  访问 [https://git-scm.com/download/win](https://git-scm.com/download/win)
2.  下载并运行安装程序。
3.  在整个安装过程中使用默认设置。
4.  安装后重启 VS Code。

**Mac 系统：**

1.  打开“终端”（Terminal）应用（按 `Cmd + Space`，输入 "Terminal"，然后按 Enter）。
2.  输入命令：`git --version`
3.  如果 Git 未安装，系统会提示您安装 Xcode 命令行工具——点击“安装”。
4.  等待安装完成。

**Linux 系统：**

```bash
sudo apt update
sudo apt install git
```

### 第 3 步：克隆本代码仓库

**首先，获取仓库 URL：**

1.  在您正在阅读 �� 指南的 GitHub 页面顶部，找到绿色的 "**< > Code**" 按钮。
2.  点击该按钮。
3.  确保选择了 "HTTPS" 选项。
4.  点击 URL 旁边的复制图标（📋）以复制它。

**现在，克隆仓库：**

1.  打开 VS Code。
2.  按 `Ctrl+Shift+P` (Windows/Linux) 或 `Cmd+Shift+P` (Mac) 打开命令面板。
3.  输入 "Git: Clone" 并选择它。
4.  **粘贴您刚刚复制的 URL** (`Ctrl+V` 或 `Cmd+V`)。
5.  在您的电脑上选择一个文件夹来保存项目（例如，桌面或文档）。
6.  点击 "Clone"。
7.  当系统提示“您想打开克隆的仓库吗？”，点击“打开”。

**或者，使用终端进行克隆：**

1.  从上方绿色的 "Code" 按钮处复制仓库 URL。
2.  打开终端或命令提示符。
3.  切换到您想存放项目的目录：`cd Desktop` (或您偏好的其他位置)。
4.  运行：`git clone [在此处粘贴 URL]`
5.  运行：`cd [仓库名称]` (将 `[仓库名称]` 替换为实际创建的文件夹名称)。
6.  运行：`code .` 以在 VS Code 中打开项目。

### 第 4 步：安装 Python 扩展

1.  在 VS Code 的左侧边栏中，�� 击扩展图标（看起来像积木）。
2.  搜索 "Python" 并安装由 Microsoft 官方发布的 Python 扩展。
3.  搜索 "Jupyter" 并安装由 Microsoft 官方发布的 Jupyter 扩展。

### 第 5 步：创建您的环境文件

1.  在 VS Code 的左侧文件浏览器中，您应该能看到项目文件。
2.  **导航到 `content` 文件夹** —— 点击 `content` 文件夹以展开它。
3.  在 `content` 文件夹内部右键单击，选择“新建文件”。
4.  将文件精确命名为：`.env` (包括前面的点)。
5.  打开 `.env` 文件并添加以下内容：
    ```
    OPENAI_API_KEY=your_openai_key_will_go_here
    ```
6.  保存文件 (`Ctrl+S` on Windows/Linux, `Cmd+S` on Mac)。

### 第 6 步：获取您的 OpenAI API 密钥

1.  访问 [https://platform.openai.com](https://platform.openai.com)
2.  如果您没有账户，请点击“Sign up”注册；如果已有账户，请点击“Log in”登录。
3.  完成注册流程（需要验证您的邮箱和电话号码）。
4.  登录后，点击右上角的个人资料图标。
5.  从下拉菜单中选择 "View API keys"。
6.  点击 "Create new secret key"。
7.  给它一个名称，如 "AI Labs Project"。
8.  **重要提示**：请立即复制密钥——您之后将无法再次看到它。
9.  返回 VS Code，将 `.env` 文件中的 `your_openai_key_will_go_here` 替换为您的真实密钥。
10. 保存 `.env` 文件。

**现在您的 .env 文件应该看起来像这样：**

```
OPENAI_API_KEY=sk-proj-abc123...your_actual_key_here
```

### 第 7 步：设置 Python 虚拟环境

1.  打开 VS Code 的集成终端：
    - Windows/Linux: 按 `Ctrl+`` (反引号)
    - Mac: 按 `Cmd+`` (反引号)
2.  创建一个虚拟环境：
    ```bash
    python -m venv venv
    ```
3.  激活虚拟环境：
    - **Windows**: `venv\Scripts\activate`
    - **Mac/Linux**: `source venv/bin/activate`
4.  激活后，您应该会在终端提示符的开头看到 `(venv)`。
5.  安装所需的依赖包：
    ```bash
    pip install openai python-dotenv jupyter
    ```

### 第 8 步：测试您的设置

1.  在 VS Code 中，打开任意一个 `.ipynb` (Jupyter notebook) 文件。
2.  如果提示选择内核（kernel），请选择来自您 `venv` 文件夹中的 Python 解 �� 器。
3.  运行加载 API 密钥的第一个代码单元格——您应该会看到 "✅ API key configured" 的输出。
4.  如果您看到此消息，说明您已准备好开始实验了！

## 问题排查

**"Command 'python' not found" (找不到 'python' 命令):**

- 尝试使用 `python3` 代替 `python`。
- 在 Windows 上，您可能需要从 [python.org](https://python.org) 安装 Python。

**"No module named 'openai'" (没有名为 'openai' 的模块):**

- 确保您的虚拟环境已激活（终端中应显示 `(venv)`）。
- 重新运行 `pip install openai python-dotenv`。

**API 密钥不工作:**

- 仔细检查您的 `.env` 文件中没有多余的空格。
- 确保文件名就是 `.env` (而不是 `.env.txt`)。
- 确认您的 OpenAI 账户已设置付款信息。

**需要帮助？**

- 检查您的 `.env` 文件是否位于 `content` 文件夹中（与 notebook 文件在同一文件夹）。
- 确保在添加 API 密钥后已保存 `.env` 文件。
- 如果问题依旧，尝试重启 VS Code。

---

**安全提示**：切勿与任何人分享您的 `.env` 文件或 API 密钥。`.env` 文件应仅保留在您的本地计算机上。
