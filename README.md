<p align="left">
    <a href="README-zh_CN.md">中文</a>
</p>

# Generative AI Course

This is the most comprehensive Generative AI course you'll come across.

We'll start from absolute first principles - what AI actually is, how it was invented, and the rise of large language models (LLMs). From there, we'll move into hands-on tutorials, including building basic chatbots, implementing Retrieval-Augmented Generation (RAG), creating AI agents, exploring MCP, and understanding what it takes to build production-grade AI applications.

I've worked with hundreds of clients and understand what it truly takes to build real-world AI solutions that scale. This course has no jargon, no fluff - everything is explained from the ground up. Most AI courses out there are either confusing, overly academic, or lack practical relevance. This one is different.

We're in a moment where the demand for AI applications is exploding - and the barrier to entry is lower than ever. That's why **AI engineering** - the process of building applications on top of powerful, readily available models - is one of the fastest-growing disciplines in tech.

Building apps on top of machine learning models isn't new. Long before LLMs, we were building AI apps for fraud detection, product recommendations, and more. Many principles still apply, but LLMs introduce new challenges and opportunities.

**The key differences between traditional ML and AI engineering:**

1. AI engineering is less about training models and more about adapting them (e.g. prompt engineering, fine-tuning).
2. AI engineering deals with larger models that require more compute - which means higher latency and different infrastructure needs.
3. AI models often produce open-ended outputs, making evaluation more complex than traditional ML.

**Here's a snapshot of what we'll cover:**

- Deploying local LLMs
- Building end-to-end AI chatbots and managing context
- Prompt engineering
- Defensive prompting and preventing common AI exploits
- Retrieval-Augmented Generation (RAG)
- AI Agents and advanced use cases
- Model Context Protocol (MCP)
- LLMOps
- What good data looks like for AI

---

## Setup Instructions

Follow these steps to set up your development environment and run the lab exercises.

### Step 1: Install Visual Studio Code

1. Go to [https://code.visualstudio.com](https://code.visualstudio.com)
2. Click the blue "Download" button for your operating system:
   - **Windows**: Click "Download for Windows"
   - **Mac**: Click "Download for Mac"
   - **Linux**: Click "Download for Linux"
3. Run the downloaded installer and follow the installation prompts
4. Open VS Code when installation completes

### Step 2: Install Git

**Windows:**

1. Go to [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Download and run the installer
3. Use default settings throughout the installation
4. Restart VS Code after installation

**Mac:**

1. Open Terminal (press `Cmd + Space`, type "Terminal", press Enter)
2. Type: `git --version`
3. If Git isn't installed, it will prompt you to install Xcode Command Line Tools - click "Install"
4. Wait for installation to complete

**Linux:**

```bash
sudo apt update
sudo apt install git
```

### Step 3: Clone This Repository

**First, get the repository URL:**

1. At the top of this GitHub page (where you're reading these instructions), look for the green "**< > Code**" button
2. Click the green "**< > Code**" button
3. Make sure "HTTPS" is selected
4. Click the copy icon (📋) next to the URL to copy it

**Now clone the repository:**

1. Open VS Code
2. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac)
3. Type "Git: Clone" and select it
4. **Paste the URL you just copied** (Ctrl+V or Cmd+V)
5. Choose a folder on your computer to save the project (e.g., Desktop or Documents)
6. Click "Clone"
7. When prompted "Would you like to open the cloned repository?", click "Open"

**Alternative method using Terminal:**

1. Copy the repository URL from the green "Code" button above
2. Open Terminal/Command Prompt
3. Navigate to where you want the project: `cd Desktop` (or your preferred location)
4. Run: `git clone [paste the URL here]`
5. Run: `cd [repository-name]` (replace with the actual folder name created)
6. Run: `code .` to open VS Code

### Step 4: Install Python Extensions

1. In VS Code, click the Extensions icon in the left sidebar (looks like building blocks)
2. Search for "Python" and install the official Python extension by Microsoft
3. Search for "Jupyter" and install the Jupyter extension by Microsoft

### Step 5: Create Your Environment File

1. In VS Code, you should see your project files in the left sidebar
2. **Navigate to the `content` folder** - click on the `content` folder to open it
3. Right-click inside the `content` folder and select "New File"
4. Name the file exactly: `.env` (including the dot at the beginning)
5. Open the `.env` file and add this line:
   ```
   OPENAI_API_KEY=your_openai_key_will_go_here
   ```
6. Save the file (`Ctrl+S` on Windows/Linux, `Cmd+S` on Mac)

### Step 6: Get Your OpenAI API Key

1. Go to [https://platform.openai.com](https://platform.openai.com)
2. Click "Sign up" if you don't have an account, or "Log in" if you do
3. Complete the registration process (you'll need to verify your email and phone number)
4. Once logged in, click your profile icon in the top-right corner
5. Select "View API keys" from the dropdown menu
6. Click "Create new secret key"
7. Give it a name like "AI Labs Project"
8. **Important**: Copy the key immediately - you won't be able to see it again
9. Go back to VS Code and replace `your_openai_key_will_go_here` in your `.env` file with your actual key
10. Save the `.env` file

**Your .env file should now look like:**

```
OPENAI_API_KEY=sk-proj-abc123...your_actual_key_here
```

### Step 7: Set Up Python Virtual Environment

1. Open VS Code's integrated terminal:
   - Press `Ctrl+`` (backtick) on Windows/Linux
   - Press `Cmd+`` (backtick) on Mac
2. Create a virtual environment:
   ```bash
   python -m venv venv
   ```
3. Activate the virtual environment:
   - **Windows**: `venv\Scripts\activate`
   - **Mac/Linux**: `source venv/bin/activate`
4. You should see `(venv)` at the beginning of your terminal prompt
5. Install required packages:
   ```bash
   pip install openai python-dotenv jupyter
   ```

### Step 8: Test Your Setup

1. In VS Code, open one of the `.ipynb` (Jupyter notebook) files
2. If prompted to select a kernel, choose the Python interpreter from your `venv` folder
3. Run the first cell that loads the API key - you should see "✅ API key configured"
4. If you see this message, you're ready to start the labs!

## Troubleshooting

**"Command 'python' not found":**

- Try using `python3` instead of `python`
- On Windows, you may need to install Python from [python.org](https://python.org)

**"No module named 'openai'":**

- Make sure your virtual environment is activated (you should see `(venv)` in your terminal)
- Run `pip install openai python-dotenv` again

**API key not working:**

- Double-check there are no extra spaces in your `.env` file
- Make sure the file is named exactly `.env` (not `.env.txt`)
- Verify your OpenAI account has billing set up

**Need help?**

- Check that your `.env` file is in the `content` folder (same folder as the notebook files)
- Make sure you've saved the `.env` file after adding your API key
- Restart VS Code if things aren't working

---

**Security Note**: Never share your `.env` file or API key with anyone. The `.env` file should stay on your computer only.
