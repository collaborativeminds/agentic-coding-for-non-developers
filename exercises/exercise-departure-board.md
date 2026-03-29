# Exercise: Build & Deploy a Public Transport Departure Board

**with Claude Code, React, and GitHub Pages**

---

> **Tip: Copying code and commands**
> Whenever you see a code block containing a prompt or a terminal command, hover your mouse over it and use the **Copy** button that appears in the top right corner. This is much faster and less error-prone than manually highlighting the text!

## Part 1: Set Up the Project Repository

In this first part you will create a new GitHub repository for your departure board app using the terminal. All commands in this exercise are run in the terminal — that's an important part of the learning!

### Step 1: Verify Your Setup

Before diving in, let's make sure the tools you need are working. Open your terminal and run each of these commands one at a time:

```bash
node --version
gh auth status
claude --version
```

Each command should print a version number or show that you are logged in. If any command fails, go back to the preparations document and complete the setup for that tool before continuing.

### Step 2: Create a GitHub Repository

In the terminal, navigate to the folder where you keep your projects. Then run the following command to create a new repository on GitHub and clone it to your machine:

```bash
gh repo create my-departure-board --public --clone
cd my-departure-board
```

Replace `my-departure-board` with whatever you want to call your project. The `--public` flag makes the repository public (required for free GitHub Pages hosting), and `--clone` automatically clones it into a new folder. Change this to `--private` if you have a paid GitHub account and want to keep the project hidden.

> **First time using gh?**
> If you haven't used the GitHub CLI before, you may need to authenticate first. Run `gh auth login` and follow the prompts. Choose "GitHub.com", then "HTTPS", and authenticate via your browser.

---

## Part 2: Get a Trafiklab API Key

### What is an API?

An **API** (Application Programming Interface) is a way for programs to talk to each other. In our case, your departure board will send a request to Trafiklab's servers asking "What departures are coming from Centralstationen?" and the API will send back the answer as structured data that your app can display.

An **API key** is a unique code that identifies your app to the service. It's how Trafiklab knows who is making the request. Think of it like a membership card — you need to show it every time you ask for data.

### Step 3: Create a Trafiklab Account

Go to [developer.trafiklab.se](https://developer.trafiklab.se) and create a free account. Once logged in:

1. Click **"My projects"** and create a new project (give it any name, e.g. "Departure Board").
2. In the project, click **"Add API"** and add the **"Trafiklab Realtime"** API.
3. An API key will be generated for your project. Copy it — you'll need it in the next step.

> **Keep Your API Key Secret!**
> An API key is like a password. Anyone who has it can make requests on your behalf (and potentially run up costs on paid plans). Never paste it directly into your source code, and never commit it to Git. We'll store it safely in a `.env` file next.

> **Rate limits on the free tier**
> The free Bronze tier allows 25 API calls per minute and 100,000 per month. This is plenty for development and personal use. The API caches responses for 60 seconds anyway, so refreshing more often than once per minute gives no new data.

### Step 4: Store the API Key Locally

Back in your terminal (make sure you're inside the project folder), create a `.env` file:

```bash
echo "VITE_TRAFIKLAB_API_KEY=your_api_key_here" > .env
```

Replace `your_api_key_here` with the actual key you copied. The `VITE_` prefix is important — Vite (the build tool we'll use) only exposes environment variables to your app if they start with this prefix.

> **What is a `.env` file?**
> A `.env` file stores configuration values that your app needs but that you don't want in your code. The file stays on your computer and is never uploaded to GitHub. This is the standard way to handle secrets like API keys.

---

## Part 3: Build the App with Claude Code

### Step 5: Launch Claude Code

Make sure you're in the project directory in your terminal, then start Claude Code:

```bash
claude
```

Claude Code will start up and you'll see a prompt where you can type messages.

### Step 6: Switch to Plan Mode

Before writing your first prompt, switch to **Plan mode** so Claude creates a detailed plan before writing any code.

In the Claude Code prompt, press **Shift+Tab** to cycle through the available modes. You'll see the mode indicator change at the bottom of the prompt. The modes cycle in this order: **Normal** → **Plan** → **Auto**. Stop when you see **Plan** selected.

In Plan mode, Claude will analyze your request and produce a step-by-step implementation plan instead of immediately writing code. This gives you the chance to review, adjust, and approve the approach before any files are created.

### Step 7: Scaffold the Project

We'll build the app step by step rather than all at once. This way you can test each piece as it's added, and if something goes wrong, you'll know exactly which part caused the issue. This is also how real software is built — one feature at a time.

For this project, we are asking Claude to use a standard modern tech stack:

- **[React](https://react.dev/)**: A popular library for building user interfaces out of reusable components.
- **[Vite](https://vitejs.dev/)** _(pronounced "veet")_: A lightning-fast development server and build tool that instantly updates the browser when code changes.
- **[TypeScript](https://www.typescriptlang.org/)**: A version of JavaScript that adds type checking, which helps catch errors before the code even runs.
- **[shadcn/ui](https://ui.shadcn.com/)**: A collection of beautifully designed, accessible UI components. We use it instead of typical component libraries because it adds the component code directly into your project, giving you full control to customize the design while saving hours of building from scratch.

To prepare your first prompt, we recommend drafting it outside the terminal:

1. Copy the prompt template below.
2. Paste it into a new markdown or text file (preferably stored outside your project folder).
3. Edit the placeholders (`[YOUR APP NAME]` and the design description) with your own preferences.
4. Copy your customized prompt and paste it into Claude Code.

> **PROMPT 1 — Edit before pasting into Claude Code**
>
> ```
> Build a public transport departure board app called [YOUR APP NAME] using
> Vite + React + TypeScript and shadcn/ui components.
>
> The app should show a header with the app name and a search bar where users
> can type a stop name (e.g. "Centralstationen" or "Slussen"). For now, when
> someone searches, just display the stop name they typed below the search
> bar — we'll connect real departure data in the next step.
>
> UI and design:
> - [DESCRIBE YOUR PREFERRED THEME AND VISUAL STYLE HERE. For example: "A
>   clean Scandinavian design with dark blue and white, inspired by SL's
>   branding" or "A modern dashboard style with color-coded transport modes
>   (blue for bus, red for metro, green for tram)" or "A minimal dark-theme
>   design that looks like an actual departure board at a train station"]
> - Fully responsive design that works well on mobile, tablet, and desktop
> - Ensure high accessibility compliance (WCAG) for all interactive elements
> - Use shadcn/ui components for inputs, buttons, and cards
>
> Create a comprehensive .gitignore that includes: .env, .env.local,
> .env.*.local, node_modules/, dist/, .DS_Store, and IDE folders (.vscode/,
> .idea/).
> ```

### Step 8: Review the Plan

Once you submit the prompt, Claude will think for a moment and then present a detailed plan. Take your time reading through it. Here's what to look for:

- **File structure:** Does the proposed folder layout make sense?
- **Design:** Does the plan describe the visual style you asked for?
- **Dependencies:** What packages does it plan to install? If you don't recognize something, ask Claude what it is.
- **.gitignore:** Does the plan mention creating a .gitignore file?

#### Tweaking the Plan

If there's anything you'd like to change, just type your feedback directly in the Claude Code prompt. For example:

- _"I'd like the search results to appear as a dropdown list, not below the search bar"_
- _"Make the header look like a real departure board with a dark background"_
- _"Add transport mode icons (bus, train, tram, metro) in the header"_

Claude will update the plan based on your feedback. You can go back and forth as many times as you want until you're happy.

### Step 9: Accept the Plan

When you're satisfied with the plan, switch back from Plan mode to Normal mode by pressing **Shift+Tab** (cycle past Auto back to Normal). Then tell Claude to go ahead:

```
Go ahead and implement the plan.
```

### Step 10: Watch Claude Work

Claude will now start creating files and running commands. Here's what to expect:

- **Command approvals:** Claude Code will ask for permission before running terminal commands (like `npm install` or `npm create vite`). Read each command before approving — you approve by pressing **Enter** or the **y** key.
- **File creation:** Claude will create and edit files automatically. You don't need to approve each file change, but you can see what's being created in the output.
- **Patience:** The implementation can take a few minutes. Claude is setting up the project, installing dependencies, and writing the code. Let it finish before moving on.

> **Read Before You Approve!**
> Always read the command Claude wants to run before pressing Enter. This is a good habit when using any AI coding assistant. While Claude Code is generally safe, you should understand what each command does.

---

## Part 4: Test and Commit the Scaffold

### Step 11: Start the Development Server

Once Claude is done, open a **new terminal window** (keep Claude Code running in the original one). Navigate to your project folder and start the development server:

```bash
cd my-departure-board
npm run dev
```

Vite will print a local URL (usually `http://localhost:5173`). Open it in your browser.

You should see your app with a header and a search bar. Try typing a stop name and verify that it appears on the page. The design should match the style you described.

> **Something not working?**
> Open your browser's developer tools by pressing **F12** (or **Cmd+Option+I** on Mac). Click the **Console** tab — any errors will appear here as red messages. Copy the error text and paste it into Claude Code so it can help you fix it.
>
> If the page is blank, check that you're in the right folder and that `npm run dev` didn't show any errors in the terminal.

### Step 12: Commit the Scaffold

Once the scaffold is working, it's time to save this progress with Git. In the terminal where you ran `npm run dev`, the dev server is still running — open **another terminal window** (or use the one with Claude Code if you prefer). Navigate to your project folder and run:

```bash
git status
```

This shows you all the files that have been created or changed. **Check carefully that `.env` is NOT listed.** If your .gitignore is working correctly, the `.env` file should not appear at all. If you do see `.env` in the list, **stop** and ask Claude Code for help — your API key could end up on GitHub.

Once you've verified that `.env` is not listed, stage and commit:

```bash
git add .
git commit -m "Set up project scaffold with search bar"
```

> **What do "stage" and "commit" mean?**
> Think of it like sending a package. **Staging** (`git add`) is putting items in the box. **Committing** (`git commit`) is sealing the box and writing a label on it. Later, **pushing** (`git push`) sends the box to GitHub. You can stage files one at a time or all at once, and you can review what's in the box before sealing it.

> **Why commit after each step?**
> Each commit is a save point. If something breaks later, you can always go back to the last working version. Committing often, with clear messages, is one of the most important habits in software development.

---

## Part 5: Add Departure Data

### How the Trafiklab Realtime APIs Work Together

Before we ask Claude to write the code, it helps to understand how we'll get the data. Trafiklab requires a two-step process when searching for departures by stop name:

1. **Stop Lookup API:** First, we send the stop name to the Stop Lookup API to find matching stops and get their IDs. The response includes stop groups with names, IDs, and the transport modes available at each stop (BUS, TRAIN, TRAM, METRO).
2. **Departures API:** Then, we use the stop ID to fetch upcoming departures for that stop. The API returns a 60-minute window of departures with scheduled times, real-time estimated times, delays, cancellation status, route numbers, destinations, and platform information.

Why use this two-step process? Stop names are informal and often ambiguous — there might be multiple stops with similar names, or a single station might have several child stops for different transport modes. The Stop Lookup API resolves the name to a precise stop ID, ensuring we fetch departures for the correct location.

> **Real-time vs. static data**
> Some transport operators provide real-time predictions (actual estimated arrival times based on vehicle GPS), while others only provide static timetable data (the originally scheduled times). The API indicates which is which. Your app will display whichever is available, preferring real-time when present.

### Step 13: Connect the APIs

As before, copy this prompt to your markdown file to review it, then paste it into the Claude Code terminal:

> **PROMPT 2 — Review and then paste into Claude Code**
>
> ```
> Connect the app to the Trafiklab Realtime APIs for live departure data.
> The API key is stored in the environment variable VITE_TRAFIKLAB_API_KEY
> (in the .env file). Use import.meta.env.VITE_TRAFIKLAB_API_KEY to access
> it. Never hard-code the API key.
>
> We need a two-step process to fetch the data:
> 1. Use the Stop Lookup API to search for stops matching the user's input:
>    GET https://realtime-api.trafiklab.se/v1/stops/name/{searchValue}?key={key}
>    This returns stop groups with id, name, and transport_modes.
>
> 2. When the user selects a stop, use the Departures API to fetch upcoming
>    departures:
>    GET https://realtime-api.trafiklab.se/v1/departures/{stopId}?key={key}
>    This returns departures for the next 60 minutes.
>
> When a user searches for a stop:
> - Show a dropdown/list of matching stops from the Stop Lookup API so the
>   user can select the correct one
> - Display each stop's available transport modes (BUS, TRAIN, TRAM, METRO)
>   with appropriate icons or labels
>
> When a stop is selected, show its departures:
> - Route number/line, destination name, and departure time
> - Show real-time estimated time when available, otherwise show scheduled
>   time
> - Indicate delays (e.g. "+3 min") and cancelled departures clearly
> - Group or color-code departures by transport mode
> - Show platform/track information when available
>
> Add a small attribution footer: "Data from Trafiklab.se" that links to
> https://www.trafiklab.se (required by the CC-BY 4.0 license).
>
> Show a friendly error message if no stops are found or if the API request
> fails.
> ```

Claude may ask to run commands or edit files — review and approve as before. Once it's done, go back to your browser (the page should auto-reload) and search for a real stop name (try "Centralstationen" or "Slussen").

> **No departures showing?**
> If you search during late night hours, there may genuinely be no departures in the next 60 minutes. Try searching for a major station like "Stockholm City" or "T-Centralen" which typically has frequent service. Also check the browser console (F12) for any API errors.

### Step 14: Commit the Departure Data Feature

Once the departure data is displaying correctly, commit your progress:

```bash
git add .
git commit -m "Add stop search and live departure data"
```

---

## Part 6: Add More Features

### Step 15: Add Auto-Refresh and Dark Mode

A real departure board updates continuously. We'll add auto-refresh so the departure list stays current, plus dark mode support — which is especially useful for a departure board, since many real station displays use a dark background for readability.

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 3 — Review and then paste into Claude Code**
>
> ```
> Add two features:
>
> 1. Auto-refresh: Automatically refresh the departure data every 30 seconds
>    when a stop is selected.
>    - Show a subtle countdown or indicator so the user knows when the next
>      refresh will happen
>    - Show a brief "Updating..." indicator during refresh without clearing
>      the current departures (so the board doesn't flash empty)
>    - Stop auto-refresh when no stop is selected
>
> 2. Dark mode and light mode support:
>    - Add a toggle switch in the header to switch between dark and light mode
>    - Use the user's system preference as the default mode
>    - Add smooth transitions when switching between modes
>    - Make sure all text, backgrounds, and cards look good in both modes
>    - The dark mode should feel like an actual station departure board
> ```

Test the auto-refresh by watching the departure times update. Test the dark mode toggle and try changing your system's dark/light mode setting to see if the app picks it up.

### Step 16: Commit Auto-Refresh and Dark Mode

```bash
git add .
git commit -m "Add auto-refresh and dark mode toggle"
```

### Step 17: Add Geolocation and Favorite Stops

**Understanding Geolocation for Public Transport**

When you use the browser's Geolocation API, it provides your exact coordinates (latitude and longitude). We can use these coordinates to find stops near you. The Stop Lookup API's stop data includes latitude and longitude for each stop, so we can calculate distances to find the closest ones. This makes it easy to quickly pull up departures for your nearest stop without typing anything.

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 4 — Review and then paste into Claude Code**
>
> ```
> Add two more features:
>
> 1. Geolocation - find nearby stops: When the app loads for the first time,
>    offer to detect the user's location.
>    - Use the browser's Geolocation API to get the user's coordinates.
>    - Search for stops near the user. Use the Trafiklab stop lookup API to
>      search for common stop names, then calculate the distance from the
>      user's coordinates to each stop's lat/lon coordinates in the response.
>      Show the closest stops so the user can pick one.
>    - Show a friendly message if the user declines location access or if
>      geolocation is not available.
>
> 2. Favorite stops: Let users save stops as favorites using localStorage.
>    - Show a star/heart icon next to each stop that can be toggled
>    - Display favorite stops as quick-access buttons below the search bar
>    - Clicking a favorite should immediately load its departures
>    - Favorites should persist across page reloads
> ```

Test both features:

- Reload the page — you should be asked to share your location
- Search for a few stops and add them as favorites
- Click a favorite to verify it loads the departures
- Check that favorites persist after reloading the page

### Step 18: Commit and Push All Features

```bash
git add .
git commit -m "Add geolocation and favorite stops"
```

Now it's time to push all your commits to GitHub. Up to this point, your commits have only been saved locally on your computer. Pushing uploads them all at once:

```bash
git push
```

> **Why push now and not after every commit?**
> Committing and pushing are separate actions. Committing saves a snapshot locally — it's fast and you can do it as often as you like. Pushing uploads your commits to GitHub so they're backed up and visible online. It's common to make several commits and push them together when you reach a good stopping point.

You can verify your code is on GitHub by visiting your repository in the browser. You should see all the files and, if you click the commit history, all four commits you've made so far.

---

## Part 7: Deploy to GitHub Pages

### Step 19: Enable GitHub Pages

You need to configure your GitHub repository to use GitHub Actions for deploying to GitHub Pages. Go to your repository on GitHub in your browser:

1. Click **Settings** (in the top menu bar of your repository)
2. Click **Pages** (in the left sidebar)
3. Under **Build and deployment**, change the **Source** dropdown to **GitHub Actions**

> **Why GitHub Actions?**
> GitHub Actions lets you automate tasks — in our case, building the app and publishing it as a website every time you push new code. This is called CI/CD (Continuous Integration / Continuous Deployment), and it's how professional teams deploy software.

### Step 20: Create the Deployment Workflow

Copy this prompt to your markdown file to review it, then paste it into the Claude Code terminal:

> **PROMPT 5 — Review and then paste into Claude Code**
>
> ```
> Create a GitHub Actions workflow file at .github/workflows/deploy.yml that:
> - Triggers on push to the main branch
> - Uses Node.js 22
> - Installs dependencies with npm ci
> - Builds the Vite project with npm run build
> - Deploys the dist/ folder to GitHub Pages using the official
>   actions/deploy-pages action
> - Make sure vite.config.ts sets the base path to the repository name so
>   assets load correctly on GitHub Pages
> - Add the VITE_TRAFIKLAB_API_KEY as a repository secret reference in the
>   build step using the env: field
> ```

Claude will create the workflow file and may also update `vite.config.ts` to set the correct base path. The base path is necessary because GitHub Pages serves your site at `https://username.github.io/repo-name/` rather than at the root domain.

### Step 21: Add the API Key as a Repository Secret

The GitHub Actions workflow needs access to your API key to build the app, but you don't want the key in your code. GitHub Secrets is the solution — it stores values securely and makes them available to workflows. Run:

```bash
gh secret set VITE_TRAFIKLAB_API_KEY
```

You'll see a prompt waiting for input (the cursor will just blink). Paste your Trafiklab API key and press **Enter**. You won't see the key as you paste it — that's normal, it's hidden for security. You should see a confirmation message that the secret was set.

### Step 22: Commit and Push the Workflow

Now commit the new workflow file (and any changes to vite.config.ts) and push:

```bash
git add .
git status
```

Check the listed files — you should see the workflow file and possibly `vite.config.ts`. Then commit and push:

```bash
git commit -m "Add GitHub Pages deployment workflow"
git push
```

### Step 23: Watch the Deployment

Go to your repository on GitHub in your browser. Click the **Actions** tab. You should see a workflow run in progress or just completed. Click on it to watch the steps execute.

Once the workflow finishes successfully (green checkmark), your departure board is live! Visit:

```
https://<your-username>.github.io/<your-repo-name>/
```

Replace `<your-username>` with your GitHub username and `<your-repo-name>` with your repository name.

It may take a minute or two for the deployment to fully propagate. If the page is blank, wait a moment and refresh.

> **API Key on the Client Side**
> Because this is a client-side React app, the API key will be embedded in the built JavaScript bundle. For a personal learning project this is fine. For a production app, you'd want a backend proxy that keeps the key server-side. This is an important architectural concept to be aware of.

---

## Part 8: Make It Your Own

Now it's your turn to practice the full workflow independently — make a change, test it, commit, and deploy.

### Step 24: Add a Personal Touch

Think of a small change you'd like to make. Here are some ideas:

- Change the app title or add a subtitle
- Add filtering by transport mode (show only buses, only trains, etc.)
- Change the color scheme or add transport-mode-specific colors
- Add a footer with your name
- Show a map with the selected stop's location

Go to Claude Code and describe the change you want. For example:

```
Add a row of filter buttons above the departure list that let users show
only departures for a specific transport mode (Bus, Train, Tram, Metro)
or all of them.
```

Once Claude makes the change, test it in your browser, then commit and push:

```bash
git add .
git commit -m "Add transport mode filter"
git push
```

Watch the Actions tab — your change will be live in a minute or two.

---

## Congratulations!

You've completed the exercise! Here's what you accomplished:

- Created a GitHub repository from the terminal using the GitHub CLI
- Learned to keep secrets out of version control with `.env` and `.gitignore`
- Used Claude Code's Plan mode to design and review before building
- Built a departure board app incrementally — one feature at a time
- Worked with a two-step API flow (stop lookup then departures)
- Built a responsive real-time departure board with dark/light mode and live data
- Practiced the core Git workflow: status, stage, commit, push
- Set up automated deployment with GitHub Actions
- Published a live website on GitHub Pages
- Made your own change and deployed it independently

---

## Bonus Challenges

Want to keep going? Here are some more features to try. Each one is a chance to practice the prompt → review → test → commit workflow:

- **PWA support:** Ask Claude to add a Web App Manifest and Service Worker so the app can be installed on mobile devices and shows cached departures when offline
- **Filtering and sorting:** Add controls to filter departures by transport mode, destination, or time range
- **Multi-stop dashboard:** Let users pin multiple stops and see all their departures on a single dashboard view
- **Departure map:** Show upcoming departures on a map with route lines using Leaflet or a similar mapping library
- **Accessibility announcements:** Add screen reader announcements when departures update or when a departure is cancelled

---

## Troubleshooting

### "Command not found" when running `node`, `gh`, or `claude`

Close your terminal and open a new one. The terminal needs to reload its paths after installing new tools.

### The page is blank after `npm run dev`

Check the terminal for error messages. If you see errors about missing modules, run `npm install` and try again.

### "Invalid API key" or 403 error when searching for stops

Verify your API key at [developer.trafiklab.se](https://developer.trafiklab.se). Make sure you added the Realtime API to your project. Check that the `.env` file contains the key with the exact variable name `VITE_TRAFIKLAB_API_KEY` and that you restarted the dev server after creating or editing the `.env` file (Vite only reads `.env` at startup).

### Stop search returns no results

Try searching for a well-known stop like "T-Centralen", "Slussen", or "Centralstationen". The API searches Swedish stop names, so use Swedish characters where applicable (e.g. "Göteborg" not "Goteborg").

### Departures show but times seem wrong

The API returns times in the Swedish timezone (CET/CEST). If you are in a different timezone, the times may appear offset. Ask Claude Code: "The departure times seem to be in the wrong timezone. Can you make sure we display them in the user's local timezone, or clearly label them as Swedish time?"

### No departures appear for a valid stop

During late night or early morning hours, there may be no departures in the 60-minute window. Try a major hub station or test during daytime hours.

### Rate limit errors (HTTP 429)

The free tier allows 25 calls per minute. If you are refreshing frequently during development, you may hit this limit. Wait a minute and try again. The auto-refresh interval of 30 seconds is well within limits for normal use.

### GitHub Pages deployment fails

Go to the **Actions** tab on GitHub, click the failed run, and look for the red error step. Common issues:

- **Pages not enabled:** Make sure you completed Step 19 (setting Source to GitHub Actions)
- **Secret not set:** Make sure you completed Step 21 (setting the API key secret)
- **Branch name mismatch:** If your default branch is `master` instead of `main`, tell Claude Code: "Update the deploy workflow to trigger on the master branch instead of main"

### The deployed site is blank but local dev works fine

This usually means the base path is wrong. Tell Claude Code: "The deployed site on GitHub Pages is blank. Can you check that the base path in vite.config.ts matches my repository name?"

### The app worked before but now shows old content

If you added PWA/Service Worker support, the old version may be cached. Open developer tools (F12), go to the **Application** tab, click **Service Workers**, and click **Unregister**. Then reload the page.

---

## Clean Up (optional)

If you want to remove what you've created after the exercise:

### Delete the GitHub Pages site (optional)

Go to your repository on GitHub → **Settings** → **Pages** → click **Unpublish** (if available), or simply delete the repository.

### Revoke the API key (optional)

Go to [developer.trafiklab.se](https://developer.trafiklab.se), open your project, and remove the API key or delete the project.

> **Remember the license**
> The Trafiklab data is provided under CC-BY 4.0. If you keep the app running publicly, make sure the attribution "Data from Trafiklab.se" remains visible. If you take the app down, no further action is needed.
