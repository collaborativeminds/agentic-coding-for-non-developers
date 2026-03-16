# Course Preparations — Agentic AI for Non-Developers

Welcome! Below you'll find everything you need to install and configure on your computer before the course day. Instructions are organized by tool, with separate steps for macOS and Windows where needed.

Expect the full preparation to take approximately 60–90 minutes. The most time-consuming steps are Docker Desktop (download and account creation), GitHub configuration, and Claude Code sign-in.

> **Important:** After installing a new tool that runs in the terminal, you need to close and reopen the terminal before the command becomes available. This is because the terminal needs to reload the updated paths (PATH) to find the new program.

> **Tip: Use a password manager.** During the preparations, you'll be creating accounts with several different services (GitHub, Claude, Docker, etc.). We strongly recommend using a password manager to keep track of all your logins and generate strong, unique passwords. Some good options: [1Password](https://1password.com/), [Bitwarden](https://bitwarden.com/) (free option), or [Proton Pass](https://proton.me/pass).

---

## 1. Terminal

A terminal is the program where you type commands to control your computer — for example to install tools, start programs, or run code. Several steps in this guide require running commands in the terminal, and we'll be using it throughout the course. Inside a terminal a shell is running. For an analogy with chatGPT, consider the terminal to be a chat window where you type and the shell to be the thing that interprets your commands.

### macOS: Ghostty

We recommend **Ghostty** — a fast and modern terminal. Download it from [ghostty.org](https://ghostty.org/) and drag the app to your Applications folder.

> **Tip:** macOS has a built-in terminal app called "Terminal" (in Applications > Utilities). It works too, but Ghostty provides a better experience.

### Windows: Windows Terminal

We recommend **Windows Terminal** — Microsoft's modern terminal app. It comes pre-installed on Windows 11. If you're on Windows 10, download it for free from the [Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701). The default terminal on Windows 10, Windows Console Host, does not allow for tabs etc.

Powershell is the default shell running in windows terminal, so when you start up terminal, you will see "Powershell".

> **Tip:** Always run commands in **Windows Terminal**, not in the older "Command Prompt" (cmd.exe).

Throughout the rest of this document, "open the terminal" means launching Ghostty (macOS) or Windows Terminal (Windows).

---

## 2. Package Manager

Package managers make it easy to install and update development tools directly from the terminal. Instead of hunting down and downloading installer files from websites, you can install programs with a single command.

### macOS Package Manager — Homebrew

Several tools in this guide can be installed via Homebrew, and many online guides assume you have it.

#### What is Homebrew used for?

With Homebrew, you can run `brew install <tool>` instead of downloading `<tool>` manually. It also keeps your installed tools up to date with the `brew upgrade` command.

#### Install Homebrew

Open the terminal and run:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the on-screen instructions — you may need to enter your Mac password. Installation takes a few minutes.

**Verify the installation:** Close the terminal, reopen it, and run:

```
brew --version
```

You should see a version number (e.g. `Homebrew 4.x.x`).

### Windows Package Manager — winget

#### What is winget used for?

With winget, you can `run winget install <tool>` instead of downloading <tool> manually from a website. It also makes it easy to keep your tools up to date with the `winget upgrade` command.

#### Install winget 

Note: Windows Package Manager is included with modern versions of Windows 10 and Windows 11. If it is not available on your system, install or update the App Installer app from the Microsoft Store.

Open Windows Terminal and type `winget -v` to verify that it is installed.

You should see a version number (e.g. v1.x.x).

---

## 3. Node.js

Node.js is the platform we use to run our applications. TypeScript (the language we write code in) is installed in a later step.

**macOS and Windows:**
Go to [nodejs.org/en/download](https://nodejs.org/en/download). Scroll down to the section **"Or get a prebuilt Node.js® for..."** and select your operating system:

- **macOS:** Select **macOS** and click **"macOS Installer (.pkg)"**
- **Windows:** Select **Windows** and click **"Windows Installer (.msi)"**

Choose the version marked **LTS** (Long Term Support, currently v24.14.0). Most modern Apple computers use ARM64 architecture, but if you have an older machine you may need to choose x64 instead. Run the installer and follow the instructions — the default settings work fine.

**Verify the installation:** Open the terminal and run:

```
node --version
```

You should see a version number (e.g. `v24.x.x`). Likewise, run 

```
npm --version
```

You should see a version number (e.g. `v11.x.x`).

**Windows:** 

If you see `npm : File ... cannot be loaded because running scripts is disabled on this system`, then run

```
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Start a new terminal and try again. Now it should work.

---

## 4. Docker Desktop

Docker lets us run applications in isolated environments called containers. We use it to easily start databases and other services. A free Docker account may be needed if you want to upload to Docker Hub or other cloud features, but it is not required to run containers locally.

**macOS:**
Download Docker Desktop from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/). Run the installer and follow the instructions.

**Windows:**
Download Docker Desktop from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/). For Windows there are two options: AMD64 or ARM64. ARM64 is uncommon. You can open the terminal and type `$env:PROCESSOR_ARCHITECTURE` to see which one you have. Docker Desktop requires WSL 2 (Windows Subsystem for Linux) to be enabled. The installer will help you with this, but if you run into issues there are instructions in Docker's official guide: [docs.docker.com/desktop/setup/install/windows-install](https://docs.docker.com/desktop/setup/install/windows-install/). Run the installer and follow the instructions. The default options are fine.


**Verify the installation:** Open the terminal and run:

```
docker --version
```

You should see a version number (e.g. `v29.x.x`).

When you open Docker Desktop, you can choose to enter your email address, but you can just as well skip that step. 

**Windows:**
You may also be prompted to update your wsl. Do that.

You may also get the following error: `Docker Desktop failed to start because virtualisation support wasn't detected. Sign in to try restoring access to Docker features.`
This happens when virtualization has not been enabled. `Ctrl + Shift + Esc → Performance → CPU`, then look for `Virtualization`. If `disabled` then you can enable it
either by following the suggestion to enable or by enabling in the BIOS (use chatGPT for instructions).

---

## 5. Code Editor — Visual Studio Code or Google Antigravity

You'll need a code editor to view and navigate the code that Claude Code generates. Choose **one** of the following:

### Option A: Visual Studio Code (VS Code)

Download from [code.visualstudio.com](https://code.visualstudio.com/). Select the right version for your operating system, run the installer, and follow the instructions.

### Option B: Google Antigravity

Download from [antigravity.google/download](https://antigravity.google/download). Select the right version for your operating system, run the installer, and follow the instructions.

Both options work equally well for this course.

---

## 6. Git

Git is the tool that tracks all changes to the code — think of it as an advanced version history.

**macOS:**
Open the terminal and run `brew install git`

**Windows:**
*Alternative 1:* Download Git from [git-scm.com/downloads/win](https://git-scm.com/downloads/win). Run the installer and use the default settings.

*Alternative 2:* Open the terminal and run `winget install Git.Git`. 

After installation, open a new terminal and verify by typing `git`.

### Configure Git with your name and email

Once Git is installed, open the terminal and run the following three commands (replace with your own name and email address):

```
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
git config --global push.autoSetupRemote true
```

---

## 7. GitHub Desktop, GitHub Account and GitHub CLI

GitHub is the service where we store and collaborate on code. GitHub Desktop gives you a graphical interface that makes working with Git easier. You can also use Git in VS Code / Google Antigravity or type commands directly in the terminal.

### Create a GitHub Account

If you don't already have one, go to [github.com](https://github.com/) and create an account. You'll need a **Team subscription** (from $4/month) — contact your manager or IT department if you need help with this.

### Install GitHub Desktop

Download from [desktop.github.com](https://desktop.github.com/download/). Run the installer and follow the instructions.

#### Sign in with GitHub Desktop

When you launch GitHub Desktop for the first time, click **"Sign in to GitHub.com"** and log in with your GitHub account. This connects GitHub Desktop to your account so you can push and pull code.

### Install GitHub CLI

GitHub CLI (`gh`) is a command-line tool that lets you work with GitHub directly from the terminal. We use it during the course to create and manage repositories, among other things.

**macOS:**
If you have Homebrew installed, open the terminal and run:

```
brew install gh
```

If you don't have Homebrew, you can download the installer (.pkg) from [cli.github.com](https://cli.github.com/).

**Windows:**
Open the terminal and run:

```
winget install --id GitHub.cli
```

If the command doesn't work, you can download the installer (.msi) from [cli.github.com](https://cli.github.com/).

#### Sign in with GitHub CLI

Once GitHub CLI is installed, open the terminal (close and reopen it if it was already open) and run:

```
gh auth login
```

Select **GitHub.com**, follow the instructions in the terminal, and sign in via the browser when prompted. When the login is complete, you should see a confirmation message in the terminal.

---

## 8. Claude Code

Claude Code comes in two parts: a **desktop app** (for the graphical interface) and a **CLI tool** (the command-line tool we use in the terminal to build applications).

### Claude Account

You'll need a Claude account with at least a **Pro subscription** (from $20/month). Create an account or upgrade at [claude.ai](https://claude.ai/).

### Claude Desktop App

Download from [claude.ai/download](https://claude.ai/download). Select the right version for your operating system, install it, and sign in with your Claude account.

### Claude Code CLI

**macOS:**
Open the terminal and run:

```
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows:**
Open the terminal and run:

```
irm https://claude.ai/install.ps1 | iex
```

NOTE: There might be setup notes that say that you need to add claude to the path. If so, follow the instructions in "setup notes" to do so.

#### Sign in with Claude Code CLI

Once the installation is complete, close the terminal and reopen it. Then run:

```
claude
```

The first time you run the command, you'll be guided through a sign-in flow in the browser. Follow the instructions to connect the CLI tool to your Claude account.

A full installation guide is available at [docs.anthropic.com/en/docs/claude-code/setup](https://docs.anthropic.com/en/docs/claude-code/setup).

---

## 9. TypeScript (installed via npm)

Once Node.js is installed (step 3), you can install TypeScript. Open the terminal and run:

```
npm install -g typescript
```

**Verify the installation:** Run the following in the terminal:

```
tsc --version
```

---

## 10. Additional CLI Tools for Full-Stack Development

These tools allow Claude Code to test APIs, inspect databases, format code, and manage configuration files directly from the terminal — without needing to switch context.

### Install via npm (macOS and Windows)

| Tool               | What Claude Code uses it for                                                                  |
| ------------------ | --------------------------------------------------------------------------------------------- |
| **prettier**       | Automatically formats generated code to a consistent style                                    |
| **eslint**         | Analyzes TypeScript/JavaScript code and flags issues against the project's rules              |
| **pm2**            | Starts and monitors Node.js processes; automatically restarts them if they crash              |
| **playwright**     | Automates browsers for end-to-end testing — Claude Code can write and run UI tests            |
| **playwright-cli** | CLI interface for Playwright — inspects pages, records tests, and runs them from the terminal |

```
npm install -g prettier eslint pm2 playwright @playwright/test
```

### Install via Homebrew (macOS) / Winget (Windows)

| Tool          | What Claude Code uses it for                                                              |
| ------------- | ----------------------------------------------------------------------------------------- |
| **python**    | Runs Python scripts and is used by many CLI tools as a dependency                         |
| **curl**      | Sends HTTP requests to test APIs and download resources                                   |
| **mongosh**   | Command-line shell for MongoDB — inspects and debugs the database directly                |
| **psql**      | The equivalent for PostgreSQL — runs SQL queries and inspects the schema                  |
| **git-delta** | Enhances git diffs with syntax highlighting, making code changes easier to read           |
| **jq**        | Processes and filters JSON responses from APIs in the terminal                            |
| **yq**        | Like jq but for YAML — reads and writes configuration files like `docker-compose.yml`     |
| **terraform** | Describes cloud infrastructure as code; Claude Code can generate and apply configurations |

**macOS:**

```
brew install python curl mongosh libpq git-delta jq yq terraform
brew link --force libpq
```

**Windows:**

```
winget source update
winget install Python.Python.3.14 cURL.cURL MongoDB.Shell PostgreSQL.PostgreSQL.17 dandavison.delta jqlang.jq MikeFarah.yq Hashicorp.Terraform
```

**Verify the Python installation:** Close the terminal, reopen it, and run:

```
python3 --version
```

You should see a version number (e.g. `Python 3.x.x`).

> **Note:** On Windows, the command may be `python` instead of `python3`.

---

## Checklist

Open the terminal and run the following commands, one at a time. Make sure all of them return a version number or expected output:

- [ ] `brew --version` / `winget --version` shows a version number (optional)
- [ ] `node --version` shows a version number
- [ ] `npm --version` shows a version number
- [ ] `docker --version` shows a version number
- [ ] VS Code or Antigravity launches without issues
- [ ] `git --version` shows a version number
- [ ] GitHub Desktop is installed and signed in to your GitHub account
- [ ] `gh auth status` shows that you are logged in
- [ ] Claude Desktop app is installed and signed in
- [ ] `claude` starts in the terminal and is connected to your account
- [ ] `tsc --version` shows a version number
- [ ] `python3 --version` shows a version number

---

## Need Help?

If you run into any issues with the installation, send an email to erik.hellman@collaborativeminds.se and we'll help you before the course day.
