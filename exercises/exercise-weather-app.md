# Exercise: Build & Deploy a Weather App

**with Claude Code, React, and GitHub Pages**

---

> **Tip: Copying code and commands**
> Whenever you see a code block containing a prompt or a terminal command, hover your mouse over it and use the **Copy** button that appears in the top right corner. This is much faster and less error-prone than manually highlighting the text!

## Part 1: Set Up the Project Repository

In this first part you will create a new GitHub repository for your weather app using the terminal. All commands in this exercise are run in the terminal — that's an important part of the learning!

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
gh repo create my-weather-app --public --clone
cd my-weather-app
```

Replace `my-weather-app` with whatever you want to call your project. The `--public` flag makes the repository public (required for free GitHub Pages hosting), and `--clone` automatically clones it into a new folder. Change this to `--private` if you have a paid GitHub account and want to keep the project hidden.

> **First time using gh?**
> If you haven't used the GitHub CLI before, you may need to authenticate first. Run `gh auth login` and follow the prompts. Choose "GitHub.com", then "HTTPS", and authenticate via your browser.

---

## Part 2: Get an OpenWeatherMap API Key

### What is an API?

An **API** (Application Programming Interface) is a way for programs to talk to each other. In our case, your weather app will send a request to OpenWeatherMap's servers asking "What's the weather in Stockholm?" and the API will send back the answer as structured data that your app can display.

An **API key** is a unique code that identifies your app to the service. It's how OpenWeatherMap knows who is making the request. Think of it like a membership card — you need to show it every time you ask for data.

### Step 3: Create an OpenWeatherMap Account

Go to [openweathermap.org](https://openweathermap.org) and create a free account. Once logged in, navigate to **"API keys"** in your profile menu. You should see a default key already created. If not, create a new one. Copy the key — you'll need it in the next step.

> **Keep Your API Key Secret!**
> An API key is like a password. Anyone who has it can make requests on your behalf (and potentially run up costs on paid plans). Never paste it directly into your source code, and never commit it to Git. We'll store it safely in a `.env` file next.

> **New API keys take time to activate!**
> After you create your OpenWeatherMap account, the API key can take **up to 2 hours** to become active. If you get errors about an invalid API key later in the exercise, this is likely the reason. You can continue building the app and test it once the key activates.

### Step 4: Store the API Key Locally

Back in your terminal (make sure you're inside the project folder), create a `.env` file:

```bash
echo "VITE_WEATHER_API_KEY=your_api_key_here" > .env
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
> Build a weather app called [YOUR APP NAME] using Vite + React + TypeScript
> and shadcn/ui components.
>
> The app should show a header with the app name and a search bar where users
> can type a city name. For now, when someone searches, just display the city
> name they typed below the search bar — we'll connect real weather data in
> the next step.
>
> UI and design:
> - [DESCRIBE YOUR PREFERRED THEME AND VISUAL STYLE HERE. For example: "A
>   clean minimalist design with a blue/white color palette and rounded cards"
>   or "A bold colorful design with gradients that change based on the current
>   weather condition" or "A cozy warm design inspired by a weather journal
>   with serif fonts"]
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

- _"I'd like the search bar to be centered on the page, not in the top nav"_
- _"Use a different shade of blue"_
- _"Make the header sticky so it stays visible when scrolling"_

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
cd my-weather-app
npm run dev
```

Vite will print a local URL (usually `http://localhost:5173`). Open it in your browser.

You should see your app with a header and a search bar. Try typing a city name and verify that it appears on the page. The design should match the style you described.

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

## Part 5: Add Weather Data

### How OpenWeatherMap APIs Work Together

Before we ask Claude to write the code, it helps to understand how we'll get the data. OpenWeatherMap requires a two-step process when searching for weather by a city name:

1. **Direct Geocoding API:** First, we send the city name to the Geocoding API to get its corresponding geographic coordinates (latitude and longitude).
2. **Current Weather Data API:** Then, we use those exact coordinates to fetch the actual weather conditions at that location.

Why use this two-step process? OpenWeatherMap deprecated their old way of directly passing a city name to the weather endpoint. The Geocoding API is the modern, recommended approach because it prevents errors and ambiguities. Many cities around the world share the same name, and by converting the name to precise coordinates first, we guarantee we're getting the weather for the correct place.

### Step 13: Connect the APIs

As before, copy this prompt to your markdown file to review it, then paste it into the Claude Code terminal:

> **PROMPT 2 — Review and then paste into Claude Code**
>
> ```
> Connect the app to the OpenWeatherMap API for real weather data. The API key
> is stored in the environment variable VITE_WEATHER_API_KEY (in the .env
> file). Use import.meta.env.VITE_WEATHER_API_KEY to access it. Never
> hard-code the API key.
>
> We need to use a two-step process to fetch the data:
> 1. Use the Direct Geocoding API to convert the searched city name into
>    coordinates (latitude and longitude).
> 2. Use the Current Weather Data API with those coordinates to get the
>    current weather, and the appropriate 5-day forecast API to get the forecast.
>
> When a user searches for a city:
> - Display current temperature (in Celsius), weather condition, humidity,
>   wind speed, and a weather icon
> - Show a 5-day forecast with daily high/low temperatures
>
> Show a friendly error message if the city is not found or if the API
> request fails.
> ```

Claude may ask to run commands or edit files — review and approve as before. Once it's done, go back to your browser (the page should auto-reload) and search for a real city.

> **Getting an "invalid API key" error?**
> If your OpenWeatherMap account is brand new, your API key may not be active yet. It can take up to 2 hours. Try again later. You can continue with the next steps in the meantime — the app will work once the key activates.

### Step 14: Commit the Weather Data Feature

Once the weather data is displaying correctly (or you've confirmed the code looks right and are waiting for the API key to activate), commit your progress:

```bash
git add .
git commit -m "Add weather data and 5-day forecast"
```

---

## Part 6: Add More Features

### Step 15: Add Dark Mode and Light Mode

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 3 — Review and then paste into Claude Code**
>
> ```
> Add dark mode and light mode support:
> - Add a toggle switch in the header to switch between dark and light mode
> - Use the user's system preference as the default mode
> - Add smooth transitions when switching between modes
> - Make sure all text, backgrounds, and cards look good in both modes
> ```

Test the toggle in your browser after Claude finishes. Also try changing your system's dark/light mode setting to see if the app picks it up.

### Step 16: Commit Dark Mode

```bash
git add .
git commit -m "Add dark mode and light mode toggle"
```

### Step 17: Add Geolocation and Recent Searches

**Understanding Geolocation and City Names**
When you use the browser's Geolocation API, it provides exact coordinates (latitude and longitude), which is perfect for fetching weather data. However, coordinates aren't very readable for users. To display the actual name of your current city (like "Stockholm" instead of "59.3293, 18.0686"), we must use OpenWeatherMap's **Reverse Geocoding API**. It does the opposite of the Direct Geocoding API we used earlier: it takes coordinates and returns the location name.

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 4 — Review and then paste into Claude Code**
>
> ```
> Add two more features:
>
> 1. Geolocation: When the app loads for the first time, offer to detect the
>    user's location and show their local weather automatically.
>    - Use the browser's Geolocation API to get the user's coordinates.
>    - Use the OpenWeatherMap Reverse Geocoding API
>      (https://openweathermap.org/api/geocoding-api?collection=other#reverse)
>      to convert those coordinates into a city name.
>    - Show a friendly message if the user declines or if geolocation is not available.
>
> 2. Recent searches: Save the last 5 searched cities to localStorage so they
>    appear as quick-access buttons below the search bar. Clicking a recent
>    city should load its weather.
> ```

Test both features:

- Reload the page — you should be asked to share your location
- Search for a few cities, then check that they appear as recent searches
- Click a recent search to verify it loads the weather

### Step 18: Commit and Push All Features

```bash
git add .
git commit -m "Add geolocation and recent searches"
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
> - Add the VITE_WEATHER_API_KEY as a repository secret reference in the
>   build step using the env: field
> ```

Claude will create the workflow file and may also update `vite.config.ts` to set the correct base path. The base path is necessary because GitHub Pages serves your site at `https://username.github.io/repo-name/` rather than at the root domain.

### Step 21: Add the API Key as a Repository Secret

The GitHub Actions workflow needs access to your API key to build the app, but you don't want the key in your code. GitHub Secrets is the solution — it stores values securely and makes them available to workflows. Run:

```bash
gh secret set VITE_WEATHER_API_KEY
```

You'll see a prompt waiting for input (the cursor will just blink). Paste your OpenWeatherMap API key and press **Enter**. You won't see the key as you paste it — that's normal, it's hidden for security. You should see a confirmation message that the secret was set.

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

Once the workflow finishes successfully (green checkmark), your weather app is live! Visit:

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
- Add a Celsius/Fahrenheit toggle
- Change the color scheme
- Add a footer with your name

Go to Claude Code and describe the change you want. For example:

```
Add a toggle button next to the temperature that lets users switch between
Celsius and Fahrenheit.
```

Once Claude makes the change, test it in your browser, then commit and push:

```bash
git add .
git commit -m "Add temperature unit toggle"
git push
```

Watch the Actions tab — your change will be live in a minute or two.

---

## Congratulations!

You've completed the exercise! Here's what you accomplished:

- Created a GitHub repository from the terminal using the GitHub CLI
- Learned to keep secrets out of version control with `.env` and `.gitignore`
- Used Claude Code's Plan mode to design and review before building
- Built a weather app incrementally — one feature at a time
- Built a responsive weather app with dark/light mode and real API data
- Practiced the core Git workflow: status, stage, commit, push
- Set up automated deployment with GitHub Actions
- Published a live website on GitHub Pages
- Made your own change and deployed it independently

---

## Bonus Challenges

Want to keep going? Here are some more features to try. Each one is a chance to practice the prompt → review → test → commit workflow:

- **PWA support:** Ask Claude to add a Web App Manifest and Service Worker so the app can be installed on mobile devices and works offline
- **Hourly forecast:** Show a timeline of temperatures for the next 24 hours
- **Weather alerts:** Display severe weather warnings when they exist
- **Multiple saved locations:** Let users save favorite cities and see all their weather at a glance
- **Animated weather backgrounds:** Change the background animation based on the current weather condition (rain, sun, snow, clouds)

---

## Troubleshooting

### "Command not found" when running `node`, `gh`, or `claude`

Close your terminal and open a new one. The terminal needs to reload its paths after installing new tools.

### The page is blank after `npm run dev`

Check the terminal for error messages. If you see errors about missing modules, run `npm install` and try again.

### "Invalid API key" error when searching for a city

New OpenWeatherMap API keys can take up to 2 hours to activate. Wait and try again. You can verify your key at [openweathermap.org/api_keys](https://home.openweathermap.org/api_keys).

### Weather data shows but the 5-day forecast doesn't work

The OpenWeatherMap free tier has limitations. If the 5-day forecast endpoint returns errors, ask Claude Code: "The 5-day forecast API is returning an error. Can you check which endpoint we're using and switch to one that works with the free tier?"

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

Go to [openweathermap.org/api_keys](https://home.openweathermap.org/api_keys) and delete the API key you created. This prevents anyone from using it if it was accidentally exposed.
