# Exercise: Fetch School Lunch Menus & Plan Family Dinners

**with Claude Code and MCP**

---

> **Tip: Copying code and commands**
> Whenever you see a code block containing a prompt or a terminal command, hover your mouse over it and use the **Copy** button that appears in the top right corner. This is much faster and less error-prone than manually highlighting the text!

## Part 1: Introduction

Planning weekday dinners is one of those small tasks that takes more mental energy than it should — especially when you want to make sure the kids get a good variety of food throughout the day. In this exercise, you'll use Claude Code to fetch this week's school lunch menu from [skolmaten.se](https://skolmaten.se/) and then generate a complementary dinner plan so the family gets a balanced and varied diet all week.

Along the way, you'll discover an important limitation of AI coding assistants when it comes to reading modern websites — and learn how to overcome it using **MCP** (Model Context Protocol).

> **What is MCP?**
> MCP (Model Context Protocol) is an open standard that lets AI assistants like Claude Code connect to external tools and services. Think of MCP servers as **plugins** — they give Claude Code new abilities it doesn't have on its own, such as controlling a web browser, querying a database, or interacting with third-party APIs. You can add and remove MCP servers as needed, tailoring Claude Code's capabilities to the task at hand.

### Prerequisites

- Claude Code installed and working (run `claude --version` to verify)
- Google Chrome installed on your computer
- Node.js installed (run `node --version` to verify)

---

## Part 2: The Naive Approach

Let's start by trying the most straightforward approach — simply asking Claude Code to fetch the lunch menu from skolmaten.se. This is exactly what you'd do if you didn't know about the technical limitations we're about to discover, and that's the point: sometimes the first attempt teaches you the most.

### Step 1: Prepare the URL

Before writing a prompt, you need to find your school's page on skolmaten.se.

1. Open [skolmaten.se](https://skolmaten.se/) in your browser
2. Search for the school you're interested in
3. Navigate to the school's menu page
4. Copy the URL from your browser's address bar — it will look something like: `https://skolmaten.se/your-school-name/`

Keep this URL handy — you'll use it in the prompts below.

### Step 2: Launch Claude Code

Open your terminal and create a temporary working directory, then start Claude Code:

```bash
mkdir -p ~/mcp-exercise
cd ~/mcp-exercise
claude
```

### Step 3: Try the Naive Prompt

Copy the prompt below, replace the placeholder URL with your school's actual URL, and paste it into Claude Code:

> **PROMPT 1 — Edit the URL, then paste into Claude Code**
>
> ```
> Fetch this week's school lunch menu from [PASTE YOUR SKOLMATEN.SE URL HERE].
>
> For each day of the week, suggest a dinner that complements what the kids
> had for lunch. The dinners should:
> - Avoid repeating the same protein source as lunch
> - Give a good variety across the week (different cuisines, cooking methods)
> - Be family-friendly and reasonably quick to prepare (under 45 minutes)
>
> Present the result as a weekly overview with both lunch and dinner for
> each day.
> ```

### Step 4: Observe What Happens

Watch as Claude attempts to fulfill your request. It will try to fetch the webpage — likely using a tool like `WebFetch` or a `curl` command. You'll see it retrieve some HTML content, but when it tries to extract the menu, something goes wrong.

**The result will be disappointing.** Claude will either:

- Report that it couldn't find any menu data on the page
- Return a mostly empty page with no useful content
- Apologize and say the page didn't contain the expected information

Don't worry — this is expected! The next section explains why.

---

## Part 3: Why It Didn't Work

You just experienced one of the most common pitfalls when using AI assistants to interact with the web. Let's understand what happened.

> **What is client-side rendering?**
> There are two main ways a website can deliver content to your browser:
>
> **Server-side rendering:** The server builds the complete HTML page (with all the content) and sends it to your browser. When you download the page, everything is already there.
>
> **Client-side rendering:** The server sends a mostly empty HTML page along with JavaScript code. Your browser then runs that JavaScript, which fetches data from an API and builds the page content dynamically. Until the JavaScript runs, the page is essentially a blank shell.
>
> Many modern websites — including skolmaten.se — use client-side rendering. The lunch menu data isn't in the initial HTML at all. It gets loaded by JavaScript after the page opens in a browser.

When Claude Code used `WebFetch` or `curl` to download the page, it got the raw HTML — the empty shell. These tools don't run JavaScript. They're like reading a recipe card that just says "see cooking video" without being able to play the video. The actual menu content was never loaded because no browser was there to execute the JavaScript that fetches and displays it.

**To read a page like this, we need a real browser** — one that loads the page, executes the JavaScript, waits for the content to appear, and then reads the result. That's exactly what the Chrome DevTools MCP server does.

---

## Part 4: Install the Chrome DevTools MCP Server

### Step 5: Exit Claude Code

First, exit Claude Code by typing:

```
/exit
```

### Step 6: Install the MCP Server

> **What is an MCP server?**
> An MCP server is a program that runs alongside Claude Code and provides it with additional tools. The Chrome DevTools MCP server specifically gives Claude Code the ability to launch and control a real Google Chrome browser — navigating to pages, clicking buttons, reading rendered content, and more. It's like giving Claude Code the ability to sit down at your computer and browse the web.

Run the following command in your terminal to add the Chrome DevTools MCP server to Claude Code:

```bash
claude mcp add chrome-devtools --scope project npx chrome-devtools-mcp@latest
```

This tells Claude Code: "Add an MCP server called `chrome-devtools` and run it using this command." The server will be started automatically whenever Claude Code needs it.

### Step 7: Verify the Installation

Confirm that the MCP server was added successfully:

```bash
claude mcp list
```

You should see `chrome-devtools` in the list of configured MCP servers. If it's not there, try running the `claude mcp add` command again and watch for any error messages.

---

## Part 5: Try Again with MCP

Now that Claude Code has access to a real browser through the Chrome DevTools MCP server, let's try again.

### Step 8: Launch Claude Code Again

Start Claude Code from the same directory:

```bash
claude
```

When Claude Code starts, it will also start the Chrome DevTools MCP server in the background. You may notice Chrome briefly appearing — that's the MCP server launching a browser instance that Claude Code can control.

### Step 9: Use the MCP-Aware Prompt

This time, we'll write a prompt that explicitly tells Claude to use the browser MCP tools. Copy the prompt below, replace the placeholder URL, and paste it into Claude Code:

> **PROMPT 2 — Edit the URL, then paste into Claude Code**
>
> ```
> Use the Chrome DevTools MCP tools to fetch this week's school lunch menu:
>
> 1. Navigate to [PASTE YOUR SKOLMATEN.SE URL HERE]
> 2. Wait for the page to fully load and the menu to render
> 3. Extract the full weekly lunch menu (all days, all dishes)
>
> Present the menu in a clean, readable format organized by day.
> ```

### Step 10: Watch the Difference

This time, Claude Code will use the Chrome DevTools MCP to:

1. **Launch a real Chrome browser** (or connect to one)
2. **Navigate to the skolmaten.se page** and wait for it to load
3. **Let JavaScript execute** — the menu data gets fetched and rendered
4. **Read the fully rendered page** — now the lunch menu is actually visible in the DOM

You should see Claude successfully extract the complete weekly lunch menu with all dishes for each day.

> **Why does this work?**
> The Chrome DevTools MCP server controls an actual Chrome browser — the same browser you use every day. When it navigates to skolmaten.se, Chrome runs all the JavaScript on the page, fetches data from APIs, and renders the complete content. The MCP server then reads the final rendered page and sends that content back to Claude Code. It's the difference between reading a sealed envelope (raw HTML) and opening it and reading the letter inside (rendered page).

### Step 11: Review the Menu

Take a moment to verify that the extracted menu matches what you see when you visit the page in your own browser. The menu should include all weekdays with the correct dishes.

---

## Part 6: Suggest Dinner Menus

Now that Claude has the lunch menu, let's put it to good use.

### Step 12: Get Dinner Suggestions

Paste the following prompt into Claude Code:

> **PROMPT 3 — Paste directly into Claude Code**
>
> ```
> Based on the school lunch menu you just extracted, suggest a dinner for
> each day of the week that complements what the kids had for lunch.
>
> Guidelines for the dinner suggestions:
> - Avoid repeating the same main protein as lunch (e.g., if lunch had
>   chicken, suggest fish or a vegetarian dinner)
> - Vary cuisines across the week (Swedish, Italian, Asian, Mexican, etc.)
> - Keep it family-friendly and quick to prepare (under 45 minutes)
> - Include a balance of vegetables, protein, and carbohydrates
>
> Present it as a weekly overview showing both lunch and dinner side by side
> for each day, so it's easy to see the variety at a glance.
> ```

### Step 13: Review and Refine

Claude will present a weekly plan with lunch and dinner side by side. Look it over — if you want to adjust anything, just tell Claude in plain language. For example:

- _"We're vegetarian on Fridays, can you adjust that day?"_
- _"Replace the Thursday dinner with something quicker, we have practice that evening"_
- _"We don't eat seafood, please suggest alternatives"_

Keep refining until you're happy with the plan!

---

## Congratulations!

You've completed the exercise! Here's what you accomplished:

- **Discovered a real-world limitation** of simple web fetching tools when dealing with JavaScript-rendered websites
- **Learned what client-side rendering is** and why it matters when working with AI assistants
- **Installed and used an MCP server** to extend Claude Code's capabilities with a real browser
- **Solved a practical everyday problem** — planning family dinners around the school lunch menu
- **Experienced the MCP workflow** — identifying a gap in capability, finding the right MCP server, and using it to succeed where the first attempt failed

The key takeaway: when Claude Code can't do something on its own, there's often an MCP server that can help. MCP is what makes Claude Code truly extensible — you're not limited to what it can do out of the box.

---

## Bonus Challenges

Want to keep exploring? Try these on your own:

- **Shopping list:** Ask Claude to generate a grocery shopping list for all the suggested dinners, grouped by store section (produce, dairy, meat, pantry)
- **Printable plan:** Ask Claude to create a nicely formatted HTML file with the weekly meal plan that you can print and put on the fridge
- **Dietary tracking:** Ask Claude to estimate the nutritional balance across the full day (lunch + dinner) and flag any days that are heavy on one food group
- **Multiple kids:** If your children go to different schools, fetch both menus and create a combined dinner plan that works for the whole family
- **Explore other MCP servers:** Browse available MCP servers and try connecting Claude Code to other tools — for example, a file system MCP to save your meal plans automatically

---

## Troubleshooting

### Chrome DevTools MCP server won't start or connect

Make sure Google Chrome is installed and accessible from the command line. If you have Chrome running with other debugging sessions, close them first — the MCP server needs to launch its own Chrome instance.

Try restarting Claude Code after closing all Chrome windows.

### The menu page loads but the menu content is still empty

Some schools may not have published the current week's menu yet. Try visiting the URL in your own browser first to confirm the menu is actually available. If the menu shows in your browser but not through the MCP, try telling Claude to wait longer for the page to load.

### `claude mcp add` command fails

Make sure Node.js is installed (`node --version` should print a version number). The MCP server is installed via `npx`, which comes with Node.js.

### Claude doesn't seem to use the MCP tools

Make sure you exited and restarted Claude Code after adding the MCP server. Claude Code loads MCP servers at startup, so changes made while it's running won't take effect until the next session.

If the MCP server is listed in `claude mcp list` but Claude still doesn't use it, try explicitly mentioning "Use the Chrome DevTools MCP" in your prompt.

### Chrome opens visibly and is distracting

The Chrome instance launched by the MCP server may appear on your screen. This is normal — it's a real browser being controlled by Claude Code. You can minimize it, but don't close it while Claude is working.
