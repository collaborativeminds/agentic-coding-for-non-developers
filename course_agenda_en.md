# Agentic AI for Non-Developers

[Swedish version](course_agenda_sv.md)

---

## About the Course

This course is aimed at people who work in tech but don't have a background in software development — for example product owners, UX designers, project managers, or managers. Over the course of a full day, you'll learn to use Claude Code to build simple systems and applications, with no prior coding experience required.

---

## Agenda (Approximate Times)

### Morning

**09:00 – Welcome and Introduction**
Introduction to the course, its structure, and expectations.

**09:15 – How Does an LLM Work?**
We start by building a foundational understanding of how large language models (LLMs) actually work. You don't need to understand the mathematics — but you do need to understand the concepts in order to use the tools effectively — and because it's genuinely interesting.

- How an LLM works under the hood — an overview of the Transformer architecture and key milestones in its development
- Tokenizer — how the model reads and interprets text
- Context window — what happens when the model's "working memory" runs out
- Lost-in-the-middle — why the order of your information matters
- System Prompt, reasoning models, and tool calls
- What is an agent? Understanding concepts like agent, principal, and why "micromanagement" doesn't work
- Common mistakes when working with LLMs

**Coffee Break**

**10:30 – Effective Prompting — Hands-On Exercise**
A practical exercise where you learn to formulate clear and effective instructions for an AI model. We work through an everyday scenario to practice techniques you can then apply in more technical contexts.

**11:15 – Fundamentals of Software Development**
A crash course in the concepts you need to know in order to guide an AI coding agent in the right direction. No prior knowledge required.

- Programming languages and frameworks — what's out there and when is each used?
- Backend and frontend — what's the difference?
- Design patterns — recurring solutions to common problems
- Protocols — how systems communicate with each other
- DevOps — from code to running service
- Security, authentication, and authorization — the foundation of building secure systems

**12:30 – Lunch**

### Afternoon

**13:30 – Security and Quality**
We dive deeper into the aspects of security and quality assurance that are especially important to understand when AI is writing the code for you.

- Authentication and authorization in practice (OAuth)
- Managing API keys and secrets
- Testing — unit tests, integration tests, system tests, and UI tests
- Automated testing with Playwright

**Coffee Break**

**14:45 – Claude Code — Your Tool for Agentic Coding**
Now we get into the tool you'll be using. We walk through how Claude Code works and how to get the most out of it.

- AGENTS.md — how to control the agent's behavior
- Including context with files, URLs, and other sources
- Planning vs. code generation — when should the agent think and when should it code?
- MCP integrations and Skills
- Sub-agents — letting the agent delegate tasks
- Integration with GitHub — version control and collaboration

**Coffee Break**

**16:00 – Build a Full-Stack Application**
The closing session of the day where you put everything into practice. Using Claude Code, you'll build a complete application. Choose one of the following projects:

- **Time tracking and invoice generation** — a tool for logging time and creating invoices
- **Slack clone** — a simple messaging service
- **Trello clone** — a visual task manager
- **Dashboard with open data** — visualize data from public APIs (e.g. transport, weather, or statistics services)

**17:00 – Closing**

---

## Preparations

Detailed installation instructions are available in the [preparations document](preparations_en.md).

Before the course, you will receive an email with instructions. You'll need to have the following in place:

- **Claude Code** with at least a Pro subscription (from $20/month)
- **GitHub account** with a Team subscription (from $4/month)
- Required applications installed (instructions will be sent for both Windows and macOS)

The course also includes an optional segment where we deploy the application to the cloud. This may incur a small cost, but is nothing you need to prepare in advance.
