# Course Preparations — Agentic AI for Non-Developers

Welcome! Below you'll find everything you need to install and configure on your computer before the course day. Instructions are organized by tool, with separate steps for macOS and Windows where needed.

Expect the full preparation to take approximately 60–90 minutes. The most time-consuming steps are Docker Desktop (download and account creation), GitHub configuration, and Claude Code sign-in.

> **Important:** After installing a new tool that runs in the terminal, you need to close and reopen the terminal before the command becomes available. This is because the terminal needs to reload the updated paths (PATH) to find the new program.

> **System requirements:** Make sure your computer meets the minimum requirements below before you begin the installations.

### Computer and operating system

- **macOS:** We recommend a Mac with an Apple Silicon processor (M1, M2, M3, M4, or M5) and the latest version of macOS (macOS 26).
- **Windows:** We recommend Windows 11. Windows 10 may work, but we cannot guarantee that all tools will function correctly.

### Web browser

We recommend having [Google Chrome](https://www.google.com/chrome/) installed, as Claude Code has ready-made plugins for it. Other browsers may work, but during the course we will be using Chrome.

### Local administrator

You must be a **local administrator** on your computer to be able to install all the required tools. Contact your IT department well in advance if you don't have administrator privileges.

> **Tip: Use a password manager.** During the preparations, you'll be creating accounts with several different services (GitHub, Claude, Docker, etc.). We strongly recommend using a password manager to keep track of all your logins and generate strong, unique passwords. Some good options: [1Password](https://1password.com/), [Bitwarden](https://bitwarden.com/) (free option), or [Proton Pass](https://proton.me/pass).

---

## 1. Terminal

A terminal is the program where you type commands to control your computer — for example to install tools, start programs, or run code. Several steps in this guide require running commands in the terminal, and we'll be using it throughout the course.

### macOS: Ghostty

We recommend **Ghostty** — a fast and modern terminal. Download it from [ghostty.org](https://ghostty.org/) and drag the app to your Applications folder.

> **Tip:** macOS has a built-in terminal app called "Terminal" (in Applications > Utilities). It works too, but Ghostty provides a better experience.

### Windows: Windows Terminal

We recommend **Windows Terminal** — Microsoft's modern terminal app. It comes pre-installed on Windows 11. If you're on Windows 10, download it for free from the [Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701).

> **Tip:** Always run commands in **Windows Terminal** (or PowerShell), not in the older "Command Prompt" (cmd.exe).

Throughout the rest of this document, "open the terminal" means launching Ghostty (macOS) or Windows Terminal (Windows).

---

## 2. Homebrew — Package Manager for macOS

Homebrew is a package manager for macOS that makes it easy to install and update development tools directly from the terminal. Instead of hunting down and downloading installer files from websites, you can install programs with a single command. Several tools in this guide can be installed via Homebrew, and many online guides assume you have it.

### What is Homebrew used for?

With Homebrew, you can run `brew install <tool>` instead of downloading `<tool>` manually. It also keeps your installed tools up to date with the `brew upgrade` command.

### Install Homebrew

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

> **Note:** Homebrew is only available for macOS. Windows users should use `winget` (pre-installed on Windows 10/11) for similar functionality.

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

You should see a version number (e.g. `v24.x.x`).

---

## 4. Docker Desktop

Docker lets us run applications in isolated environments called containers. We use it to easily start databases and other services.

> **Windows users:** Docker Desktop requires WSL 2 (Windows Subsystem for Linux) to be enabled. The installer will help you with this, but if you run into issues there are instructions in Docker's official guide: [docs.docker.com/desktop/setup/install/windows-install](https://docs.docker.com/desktop/setup/install/windows-install/)

**macOS and Windows:**
Download Docker Desktop from [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/). Run the installer and follow the instructions.

You'll need to create a free Docker account if you don't already have one — you can do this on the same page.

**Verify the installation:** Open the terminal and run:

```
docker --version
```

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
Open the terminal and run:

```
brew install git
```

**Windows:**
Open the terminal and run:

```
winget install --id Git.Git
```

### Configure Git with your name and email

Once Git is installed, open the terminal and run the following three commands (replace with your own name and email address):

```
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"
git config --global push.autoSetupRemote true
```

---

## 7. GitHub Account and GitHub CLI

GitHub is the service where we store and collaborate on code.

### Create a GitHub Account

If you don't already have one, go to [github.com](https://github.com/pricing) and create an account. You'll need a **Team subscription** (from $4/month) — contact your manager or IT department if you need help with this.

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

### Sign in with GitHub CLI

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

### Sign in with Claude Code CLI

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
| **curl**      | Sends HTTP requests to test APIs and download resources                                   |
| **mongosh**   | Command-line shell for MongoDB — inspects and debugs the database directly                |
| **psql**      | The equivalent for PostgreSQL — runs SQL queries and inspects the schema                  |
| **git-delta** | Enhances git diffs with syntax highlighting, making code changes easier to read           |
| **jq**        | Processes and filters JSON responses from APIs in the terminal                            |
| **yq**        | Like jq but for YAML — reads and writes configuration files like `docker-compose.yml`     |
| **terraform** | Describes cloud infrastructure as code; Claude Code can generate and apply configurations |

**macOS:**

```
brew install curl mongosh libpq git-delta jq yq terraform
brew link --force libpq
```

**Windows:**

```
winget install cURL.cURL MongoDB.Shell PostgreSQL.PostgreSQL dandavison.delta jqlang.jq MikeFarah.yq Hashicorp.Terraform
```

---

## Checklist

Open the terminal and run the following commands, one at a time. Make sure all of them return a version number or expected output:

- [ ] `brew --version` shows a version number (optional, macOS only)
- [ ] `node --version` shows a version number
- [ ] `docker --version` shows a version number
- [ ] VS Code or Antigravity launches without issues
- [ ] `git --version` shows a version number
- [ ] `gh auth status` shows that you are logged in
- [ ] Claude Desktop app is installed and signed in
- [ ] `claude` starts in the terminal and is connected to your account
- [ ] `tsc --version` shows a version number

---

## Need Help?

If you run into any issues with the installation, send an email to erik.hellman@collaborativeminds.se and we'll help you before the course day.
