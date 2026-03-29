# Exercise: Build & Deploy a Time Tracking App

**with Claude Code, Next.js, PostgreSQL, and Docker**

---

> **Tip: Copying code and commands**
> Whenever you see a code block containing a prompt or a terminal command, hover your mouse over it and use the **Copy** button that appears in the top right corner. This is much faster and less error-prone than manually highlighting the text!

## Part 1: Set Up the Project Repository

In this exercise you will build a full-stack time tracking and invoicing application. Unlike the weather app (which ran entirely in the browser), this app has a **server** that stores data in a **database**. You will build it step by step — starting simple and adding complexity one layer at a time.

### Step 1: Verify Your Setup

Open your terminal and run each of these commands one at a time:

```bash
node --version
gh auth status
claude --version
docker --version
```

Each command should print a version number or show that you are logged in. If any command fails, go back to the preparations document and complete the setup for that tool before continuing.

Also make sure **Docker Desktop** is running — you should see the Docker icon (a whale) in your menu bar (Mac) or system tray (Windows). If it's not running, open Docker Desktop and wait for it to start before continuing.

> **What is Docker?**
> Docker lets you run programs in isolated boxes called **containers**. Instead of installing a database directly on your computer, you can start one inside a container with a single command. When you're done, you stop the container — nothing is left behind on your system.

### Step 2: Create a GitHub Repository

In the terminal, navigate to the folder where you keep your projects. Then run:

```bash
gh repo create my-time-tracker --private --clone
cd my-time-tracker
```

Replace `my-time-tracker` with whatever you want to call your project.

> **Why `--private` this time?**
> The weather app repository was public because GitHub Pages requires it for free accounts. This app will be deployed differently (using Docker), so there's no reason to make the code public. Private repositories are the default for most real projects.

### What is a "full-stack" app?

The weather app you built earlier was a **frontend-only** app — all the code ran in the browser, and it fetched data from someone else's server (OpenWeatherMap). This time you're building a **full-stack** app, which means you're building both parts:

- **The frontend** — the pages and forms the user sees in the browser
- **The backend** — the server that receives data, stores it, and sends it back when needed

Think of it like a restaurant. The frontend is the dining room — it's what the customer interacts with. The backend is the kitchen — it does the actual work of preparing and storing things. The waiter (the network) carries requests back and forth.

---

## Part 2: Scaffold the Next.js App

### Step 3: Launch Claude Code

Make sure you're in the project directory in your terminal, then start Claude Code:

```bash
claude
```

### Step 4: Switch to Plan Mode

Press **Shift+Tab** to cycle through modes. The modes cycle in this order: **Normal** → **Plan** → **Auto**. Stop when you see **Plan** selected.

### Step 5: Write the Scaffold Prompt

We'll build the app step by step. The first step is to create the basic structure — pages you can click through, but with fake placeholder data. No database, no login, no Docker yet.

To prepare your first prompt, we recommend drafting it outside the terminal:

1. Copy the prompt template below.
2. Paste it into a new markdown or text file (preferably stored outside your project folder).
3. Edit the placeholders (`[YOUR APP NAME]` and the design description) with your own preferences.
4. Copy your customized prompt and paste it into Claude Code.

> **PROMPT 1 — Edit before pasting into Claude Code**
>
> ```
> Build a time tracking and invoicing app called [YOUR APP NAME] using
> Next.js 15 with the App Router, TypeScript, Tailwind CSS v4, and
> shadcn/ui components.
>
> Create the following pages with a sidebar navigation layout:
> - Dashboard: show a summary with placeholder numbers (total hours this
>   week, number of projects, pending invoices)
> - Projects: a list of 3 example projects with name, description, and
>   hourly rate, displayed as cards
> - Time Entries: a table showing 5 example time entries with date,
>   project name, description, and duration
> - Invoices: a list of 2 example invoices with invoice number, project
>   name, amount, and status (Draft, Sent, Paid)
>
> Use hardcoded example data directly in the page components — do not set
> up a database, API, or server actions yet. We will add those later.
>
> UI and design:
> - [DESCRIBE YOUR PREFERRED THEME AND VISUAL STYLE HERE. For example:
>   "A professional clean design with a blue/gray color palette" or
>   "A warm modern design with orange accents and rounded corners"]
> - The sidebar should collapse to icons on mobile
> - Use shadcn/ui components for buttons, cards, tables, and badges
>
> Create a comprehensive .gitignore that includes: .env, .env.local,
> .env.*.local, node_modules/, .next/, dist/, .DS_Store, and IDE folders
> (.vscode/, .idea/).
> ```

Replace `[YOUR APP NAME]` with your app's name and update the design description to match your taste.

### Step 6: Review the Plan

Claude will present a plan. Here's what to look for:

- **Pages:** Does the plan mention all four pages (Dashboard, Projects, Time Entries, Invoices)?
- **Navigation:** Does it describe a sidebar layout?
- **Components:** Does it plan to use shadcn/ui?
- **No database:** Make sure the plan does NOT set up a database or Docker — that comes later.

If you want changes, type your feedback and Claude will update the plan.

> **What is Next.js?**
> In the weather app exercise you used **Vite + React** — a setup where the browser does most of the work. **Next.js** is a framework built on top of React that adds a server. This means your app can do things that browsers can't, like connecting to a database or keeping secrets safe. Next.js uses **file-based routing** — each file inside the `app/` folder automatically becomes a page at a matching URL.

### Step 7: Accept the Plan and Build

Switch back to Normal mode (press **Shift+Tab**, cycle past Auto). Then type:

```
Go ahead and implement the plan.
```

Claude will create files and run commands. Approve each command after reading it.

---

## Part 3: Test and Commit the Scaffold

### Step 8: Start the Development Server

Open a **new terminal window** (keep Claude Code running). Navigate to your project folder and start the development server:

```bash
cd my-time-tracker
npm run dev
```

Open `http://localhost:3000` in your browser. You should see:

- A sidebar with links to all four pages
- The Dashboard with placeholder summary numbers
- Click through to Projects, Time Entries, and Invoices to verify they show example data

> **Something not working?**
> Press **F12** (or **Cmd+Option+I** on Mac) to open browser developer tools. Check the **Console** tab for red error messages. Copy the error and paste it into Claude Code.

### Step 9: Commit the Scaffold

```bash
git status
```

Check that `.env` is NOT listed (though we haven't created one yet, it's good to build the habit). Then:

```bash
git add .
git commit -m "Set up project scaffold with navigation and placeholder pages"
```

> **Why commit after each step?**
> Each commit is a save point. If something breaks later, you can always go back to the last working version. Committing often, with clear messages, is one of the most important habits in software development.

---

## Part 4: Add In-Memory Data and Server Actions

Now we'll replace the hardcoded placeholder data with actual functionality — creating projects, logging time, and generating invoices. But we'll start simple: all data will be stored **in the server's memory**.

### Step 10: Write the In-Memory Data Prompt

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 2 — Review and then paste into Claude Code**
>
> ```
> Replace the hardcoded example data with a working in-memory data layer.
>
> Create an in-memory store (using plain TypeScript arrays and objects) that
> holds projects, time entries, and invoices. Seed it with 2-3 example
> projects and a few time entries when the server starts.
>
> Implement server actions for:
> - Create, edit, and delete projects (name, description, hourly rate)
> - Create and delete time entries (select a project, enter description,
>   date, and duration in hours and minutes)
> - Create invoices: pick a project and a date range, see matching time
>   entries, and generate a draft invoice that calculates the total based
>   on the project's hourly rate
>
> Update all pages to use the in-memory store:
> - Dashboard: show total hours tracked this week, number of active
>   projects, and count of draft invoices
> - Projects page: list real projects from the store, with a form to
>   create new ones
> - Time Entries page: list entries from the store, with a form to log
>   new entries
> - Invoices page: list invoices from the store, with a button to create
>   a new invoice
>
> Store all data in server memory using plain arrays and objects. Do not
> use a database. The data will reset when the server restarts — that is
> expected for now.
> ```

Let Claude implement the changes. When it's done, go back to your browser to test.

> **What is "server memory"?**
> When your app runs, the server uses your computer's RAM (memory) to store data. Think of it like a whiteboard — you can write and read information quickly, but when you turn off the server (erase the board), everything is gone. This is fine for now because it lets us build and test features without setting up a database first.

> **What is a "server action"?**
> A server action is a function that runs on the server when you submit a form or click a button. When you type a project name and click "Create", the browser sends that data to the server, and the server action adds it to the in-memory store. The important thing is that this code runs on the server, not in the browser — which means it can access things like databases and files.

### Step 11: Test the In-Memory Features

Try the following:

- Create a new project with a name, description, and hourly rate
- Log a time entry for that project
- Check that the Dashboard numbers update
- Create an invoice from time entries
- Now **restart the dev server**: press **Ctrl+C** in the terminal, then run `npm run dev` again
- Verify that your data is **gone** — this proves it was stored in memory

The data disappearing is expected! This is exactly why we'll add a database in the next step.

### Step 12: Commit the In-Memory Data Layer

```bash
git add .
git commit -m "Add in-memory data storage and server actions"
```

---

## Part 5: Set Up Docker and PostgreSQL

Time for the big upgrade: adding a real database so your data survives restarts. We'll use **PostgreSQL** (a popular database) running inside a **Docker container**.

### Step 13: Start Docker Desktop

Make sure Docker Desktop is running before continuing. You should see the Docker icon (a whale) in your menu bar or system tray. If it's not running, open Docker Desktop and wait for it to fully start.

### Step 14: Write the Database Prompt

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 3 — Review and then paste into Claude Code**
>
> ```
> Add a PostgreSQL database to the project using Docker and Prisma ORM.
>
> Docker setup:
> - Create a docker-compose.yml that runs a PostgreSQL container using the
>   postgres:17-alpine image
> - Configure the container using the environment variables POSTGRES_USER,
>   POSTGRES_PASSWORD, and POSTGRES_DB (these are the standard variables
>   that the PostgreSQL Docker image uses for initial setup)
> - Use a named volume so the database data survives when the container
>   stops
> - Expose PostgreSQL on port 5432
>
> Database schema (using Prisma):
> - Project: id, name, description, hourlyRate (decimal), isActive
>   (boolean, default true), createdAt
> - TimeEntry: id, projectId (links to Project), description, date,
>   durationMinutes (integer), createdAt
> - Invoice: id, projectId (links to Project), status (DRAFT, SENT, or
>   PAID), totalAmount (decimal), currency (default "USD"), notes,
>   issuedAt, dueAt, createdAt
> - InvoiceItem: id, invoiceId (links to Invoice), description, quantity
>   (decimal), unitPrice (decimal), totalPrice (decimal)
>
> Do not add a User model yet — authentication will be added later.
>
> Create a .env file with POSTGRES_USER, POSTGRES_PASSWORD, POSTGRES_DB,
> and DATABASE_URL. The DATABASE_URL should reference the other three
> variables (e.g. postgresql://USER:PASSWORD@localhost:5432/DBNAME).
> Also create a .env.example with placeholder values for all four
> variables.
>
> Create a seed script that adds 2-3 example projects with time entries.
>
> Replace all in-memory store code with Prisma database queries. Keep all
> the server actions working the same way from the user's perspective —
> the pages should look and behave identically, but now data is stored in
> the database.
> ```

> **What is a database?**
> A database is a program that specializes in storing and retrieving data permanently. If server memory is a whiteboard (wiped when you restart), a database is a filing cabinet — the data stays even when you turn everything off and back on. PostgreSQL is one of the most widely used databases in the world.

> **What is an ORM?**
> **Prisma** is an ORM (Object-Relational Mapper). It translates your TypeScript code into database commands. Instead of writing raw database queries, you write things like `prisma.project.findMany()` and Prisma figures out the rest. It also manages the **database schema** — the structure of your tables and their relationships.

> **What is a migration?**
> A migration is a script that tells the database how to create or change its tables — like a blueprint. When you run `npx prisma migrate dev`, Prisma looks at your schema file, compares it to the current database, and generates the necessary changes. This ensures your database structure always matches your code.

### Step 15: Set Up the Database

After Claude finishes, you need to start the database and set up the tables. Run these commands in your terminal:

```bash
docker compose up -d
```

This starts the PostgreSQL container in the background. The `-d` flag means "detached" — the container runs in the background so you can keep using the terminal.

```bash
npx prisma migrate dev
```

This creates the database tables based on the schema Claude defined. You may be asked to name the migration — something like "initial schema" is fine.

```bash
npx prisma db seed
```

This adds the example data from the seed script.

Now start the development server:

```bash
npm run dev
```

### Step 16: Test Data Persistence

This is the moment of truth — your data should now survive restarts:

1. Open `http://localhost:3000` in your browser
2. Create a new project and log a time entry
3. Stop the dev server with **Ctrl+C**
4. Start it again with `npm run dev`
5. Check that your project and time entry are **still there**

If the data is still there, the database is working. Compare this to Part 4 where data disappeared on restart — that's the difference between memory and a database.

> **Something not working?**
> If you see database connection errors, make sure Docker Desktop is running and that you ran `docker compose up -d`. You can check if the container is running with `docker compose ps`.

### Step 17: Commit the Database

```bash
git add .
git commit -m "Add PostgreSQL database with Prisma ORM"
```

---

## Part 6: Polish the Core Features

The app works, but let's make it feel complete and professional before adding more complexity.

### Step 18: Write the Polish Prompt

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 4 — Review and then paste into Claude Code**
>
> ```
> Polish the time tracking and invoicing features:
>
> 1. Printable invoice view: create a dedicated page for each invoice that
>    is optimized for printing. Use CSS @media print styles to hide the
>    sidebar and navigation, and show a clean layout with the invoice
>    number, date, line items, and total. The user should be able to print
>    it with Ctrl+P / Cmd+P.
>
> 2. Invoice status workflow: add colored badges for each status
>    (Draft = gray, Sent = blue, Paid = green). Add buttons to change the
>    status from Draft to Sent, and from Sent to Paid.
>
> 3. Weekly time summary: on the Time Entries page, show entries grouped
>    by day with a daily total, and a weekly total at the bottom.
>
> 4. Form validation: show user-friendly error messages when required
>    fields are empty or when values are invalid (e.g. negative hours).
>
> 5. Delete confirmations: show a confirmation dialog before deleting a
>    project, time entry, or invoice.
>
> Do not add authentication, user accounts, or PDF export.
> ```

### Step 19: Test the Polish

- Open an invoice and try the browser's print preview (**Ctrl+P** / **Cmd+P**) — the printable view should show a clean invoice without navigation
- Change an invoice status from Draft to Sent to Paid — the badge color should change
- Go to Time Entries and check the weekly summary with daily totals
- Try submitting a form with empty fields — you should see error messages
- Delete a time entry — you should see a confirmation dialog first

### Step 20: Commit the Polish

```bash
git add .
git commit -m "Polish invoicing and time tracking features"
```

---

## Part 7: Add Authentication

Right now, anyone who opens the app can see and edit all the data. Let's add user accounts so each person has their own private projects, time entries, and invoices.

### Step 21: Write the Authentication Prompt

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 5 — Review and then paste into Claude Code**
>
> ```
> Add user authentication with NextAuth.js v5 using a credentials provider
> (email and password).
>
> Database changes:
> - Add a User model to the Prisma schema with: id, email, passwordHash,
>   name, createdAt
> - Add a userId field to Project, TimeEntry, and Invoice so each record
>   belongs to a specific user
> - Generate a new Prisma migration for these schema changes
>
> Authentication features:
> - Create a registration page where new users can sign up with name,
>   email, and password
> - Create a login page where existing users can sign in
> - Add a logout button in the sidebar
> - Hash passwords with bcryptjs — never store plain text passwords
> - Add middleware that redirects unauthenticated users to the login page
>
> Data isolation:
> - Update ALL database queries to filter by the logged-in user's ID
> - One user must never be able to see another user's projects, time
>   entries, or invoices
>
> Add NEXTAUTH_SECRET and NEXTAUTH_URL to the .env and .env.example files.
> Generate a random value for NEXTAUTH_SECRET.
> ```

> **What is authentication?**
> **Authentication** answers the question "who are you?" — it's the process of verifying a user's identity with a username and password. **Authorization** answers "are you allowed to do this?" — in our case, users are only authorized to see their own data.

> **What is password hashing?**
> When you create an account, the app doesn't store your actual password. Instead, it runs the password through a mathematical function called a **hash** that scrambles it into a long string of random-looking characters. This process is one-way — you can verify that a password matches the hash, but you can't reverse the hash to get the password back. This means that even if someone steals the database, they can't read anyone's password.

> **What is a session?**
> After you log in, the server gives your browser a **session token** — think of it like a wristband at a concert. On every page you visit, the browser shows the wristband, and the server checks that it's valid. This way you don't have to type your password on every single page.

### Step 22: Apply the Database Migration

Claude will have updated the Prisma schema. You need to apply the changes to your database:

```bash
npx prisma migrate dev
```

If Prisma asks about data loss (because existing records don't have a userId), that's expected — our test data will be recreated when we register and start using the app.

Now restart the dev server:

```bash
npm run dev
```

### Step 23: Test Authentication

1. Open `http://localhost:3000` — you should be redirected to the login page
2. Click the link to register a new account (use any name, email, and password)
3. After registering, you should be logged in and see the Dashboard
4. Create a project and a time entry
5. Click the logout button
6. Register a **second** account with a different email
7. Verify that the second account sees **no projects or time entries** — the first user's data is private
8. Log out, log back in as the first user, and verify your data is still there

### Step 24: Commit Authentication

```bash
git add .
git commit -m "Add user authentication with NextAuth.js"
```

---

## Part 8: Add Clients

Invoices need to be sent to someone. Let's add a Client entity so invoices can show who they're billed to.

### Step 25: Write the Clients Prompt

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 6 — Review and then paste into Claude Code**
>
> ```
> Add a Client entity for managing who invoices are billed to.
>
> Database changes:
> - Add a Client model: id, userId, name, contactEmail, address (text),
>   currency (default "USD"), createdAt
> - Add a clientId field to Project (a project belongs to a client)
> - Add a clientId field to Invoice (an invoice is billed to a client)
> - Generate a new Prisma migration
>
> New features:
> - Add a Clients page in the sidebar: list, create, and edit clients
> - Update the Project form to include a dropdown to select a client
> - Update the Invoice creation flow: the user selects a client, then
>   sees that client's projects and time entries to build the invoice
> - Update the printable invoice view to show the client's name, email,
>   and address
>
> Make sure clients are scoped to the logged-in user (one user cannot see
> another user's clients).
> ```

> **What is a foreign key?**
> When we add a `clientId` field to the Project table, we're creating a **foreign key** — a link between two tables. It's like writing "belongs to Client #5" on a project folder. The database enforces this link: you can't create a project pointing to a client that doesn't exist. Foreign keys are how databases represent relationships between different types of data.

> **Why add clients now instead of at the beginning?**
> We could have designed the entire data model upfront, but that would have made the first prompts much more complex. By adding features incrementally, each step is manageable and testable. This is how professional software teams work — start with the core, then extend.

### Step 26: Apply the Migration and Test

```bash
npx prisma migrate dev
npm run dev
```

Test the new features:

1. Create a client with a name, email, and address
2. Create a project and select that client from the dropdown
3. Log time entries on the project
4. Create an invoice — the client should be pre-selected
5. Open the printable invoice view — the client's name and address should appear

### Step 27: Commit Clients

```bash
git add .
git commit -m "Add client management and link to projects and invoices"
```

---

## Part 9: Dockerize the Full Application

So far, you've been running the Next.js app directly on your computer with `npm run dev`, and only PostgreSQL runs in Docker. Now we'll package the **entire application** into Docker — both the app and the database. This is how the app will run when it's deployed to the cloud.

### Step 28: Write the Docker Prompt

Copy this prompt to your markdown file to review it, then paste it into Claude Code:

> **PROMPT 7 — Review and then paste into Claude Code**
>
> ```
> Dockerize the entire application so both Next.js and PostgreSQL run in
> Docker containers.
>
> Create a multi-stage Dockerfile for the Next.js app:
> - Use node:22-alpine as the base image
> - Set output: 'standalone' in next.config to create a minimal production
>   build
> - The final image should only contain the built app, not the source code
>   or development dependencies
>
> Create an entrypoint.sh script that:
> - Runs database migrations automatically (npx prisma migrate deploy)
> - Then starts the app (node server.js)
>
> Update docker-compose.yml to include both services:
> - The PostgreSQL container (as before, with named volume)
> - The Next.js app container (built from the Dockerfile, depends on
>   PostgreSQL, exposes port 3000)
>
> Create a .dockerignore file that excludes: node_modules, .next, .git,
> and .env files.
>
> Add a health check endpoint at /api/health that returns { status: "ok" }.
>
> After this change, running "docker compose up --build" should start both
> the database and the app. The app should be accessible at
> http://localhost:3000 with migrations applied automatically.
> ```

> **What is a Dockerfile?**
> A Dockerfile is a recipe that describes how to package your app into a container. It lists the steps: start with a base system (Node.js on Alpine Linux), copy the code, install dependencies, build the app, and define how to start it. Anyone with Docker can follow this recipe to create an identical copy of your app.

> **What is a multi-stage build?**
> Building an app requires a lot of tools (compilers, development libraries) that aren't needed to run it. A multi-stage Dockerfile uses one stage to build the app and a second stage to package only the finished result. This keeps the final container small and fast — like using a full workshop to build a chair, then shipping just the chair.

> **What is an entrypoint?**
> The entrypoint is the first command that runs when a container starts. Our `entrypoint.sh` script runs database migrations (to make sure tables are up to date) and then starts the app. This way, every time the container starts, the database is automatically prepared.

### Step 29: Build and Test with Docker

Stop the dev server if it's running, and also stop the standalone PostgreSQL container:

```bash
docker compose down
```

Now build and start everything in Docker:

```bash
docker compose up --build
```

This may take a few minutes the first time — Docker needs to download images and build your app. You'll see logs from both the database and the app in the terminal.

Once you see that the app has started, open `http://localhost:3000` in your browser. The full app should work — register, create data, and verify everything functions.

Test data persistence:

1. Register a user and create some data
2. Press **Ctrl+C** to stop Docker
3. Run `docker compose up` again (no `--build` needed this time since nothing changed)
4. Verify your data is still there

Also check the health endpoint: visit `http://localhost:3000/api/health` — you should see `{"status":"ok"}`.

> **"docker compose up --build" vs "docker compose up"**
> The `--build` flag tells Docker to rebuild the app container from the Dockerfile. You need this whenever you change your code. If you only stopped and restarted without changing code, `docker compose up` (without `--build`) is faster because it reuses the existing image.

### Step 30: Commit and Push to GitHub

```bash
git add .
git commit -m "Dockerize the full application for deployment"
```

Now push all your commits to GitHub:

```bash
git push
```

This uploads all eight commits at once. Visit your repository on GitHub to verify the code is there.

> **Why push now?**
> Committing saves snapshots locally. Pushing uploads them to GitHub. We waited until now because the deployment step (next) needs the code to be on GitHub. It's also a natural stopping point — the app is feature-complete and packaged for deployment.

---

## Part 10: Deploy to the Cloud (Teacher-Guided, Optional)

This part is guided by the teacher during the course. The steps are documented here for reference, but you don't need to do them on your own.

> **What does "deploy to the cloud" mean?**
> Right now, your app runs on your computer. Deploying to the cloud means putting it on someone else's computer (a server) that is always on and connected to the internet. After deployment, anyone with the link can use your app — not just you.

### Step 31: Create a Sliplane Account

Go to [sliplane.io](https://sliplane.io/) and create an account. The teacher will walk you through this.

### Step 32: Create a Server

In the Sliplane dashboard, create a new server. The teacher will recommend a server size.

### Step 33: Deploy the PostgreSQL Database

Deploy a PostgreSQL service on your server:

- **Image:** `postgres:17-alpine`
- **Mode:** Private (not exposed to the internet — only your app can reach it)
- **Volume:** Add a volume mounted at `/var/lib/postgresql/data` (so data survives redeployments)
- **Environment variables:**
  - `POSTGRES_USER` — choose a database username
  - `POSTGRES_PASSWORD` — choose a strong password
  - `POSTGRES_DB` — choose a database name

### Step 34: Deploy the Next.js App

Deploy the Next.js service from your GitHub repository:

- **Source:** Your GitHub repository
- **Mode:** Exposed (public, accessible from the internet)
- **Protocol:** TCP/HTTP
- **Port:** 3000
- **Health check:** `/api/health`
- **Environment variables:**
  - `DATABASE_URL` — use the PostgreSQL service's **internal hostname** (shown in the Postgres service settings after deployment):
    ```
    postgresql://USER:PASSWORD@INTERNAL_HOSTNAME:5432/DBNAME
    ```
  - `NEXTAUTH_SECRET` — a random string (at least 32 characters). Generate one with: `openssl rand -base64 32`
  - `NEXTAUTH_URL` — the public URL of your deployed app (the teacher will help you find this)

### Step 35: Verify the Deployment

Once both services are running, visit the public URL of your app. You should see the login page. Register a new account and verify that everything works — projects, time entries, invoices, and the printable invoice view.

---

## Part 11: Make It Your Own

Now it's your turn to practice independently. Pick a feature, prompt Claude Code, test it, commit, and push.

### Step 36: Add a Personal Touch

Some ideas:

- **Start/stop timer:** Add a button that starts a timer and creates a time entry when you stop it — no more manual duration entry
- **Dashboard chart:** Show a bar chart of hours tracked per project this week
- **CSV export:** Add a button to download time entries as a CSV file
- **Dark mode:** Add a toggle for dark and light mode
- **Custom branding:** Add a company logo and change the color scheme

Go to Claude Code and describe the feature you want. Once it's implemented:

1. Test it in the browser
2. If you're running with Docker, rebuild: `docker compose up --build`
3. Commit and push:

```bash
git add .
git commit -m "Add [describe your feature]"
git push
```

If your app is deployed to Sliplane, pushing to GitHub will trigger an automatic redeployment.

---

## Congratulations!

You've completed the exercise! Here's what you accomplished:

- Built a full-stack application with a frontend and a backend
- Started with in-memory data and upgraded to a real PostgreSQL database
- Used Docker to run a database and package your entire app
- Added user authentication so each person has private data
- Extended the database schema incrementally (adding clients)
- Created a printable invoice view with professional polish
- Practiced the Git workflow: status, stage, commit, push — after every feature
- Optionally deployed your app to the cloud so anyone can use it

---

## Bonus Challenges

Want to keep going? Each one is a chance to practice the prompt, plan, review, test, commit workflow:

- **PDF invoices:** Generate downloadable PDF invoices instead of relying on browser print
- **Recurring invoices:** Automatically generate invoices at the end of each month
- **Team access:** Let users invite team members to share projects
- **Email notifications:** Send an email when an invoice is marked as Sent
- **Dashboard analytics:** Charts showing hours per week, revenue per month, or top clients
- **Multi-currency support:** Let each client have a different currency, with conversion rates

---

## Troubleshooting

### "Command not found" when running `node`, `gh`, `claude`, or `docker`

Close your terminal and open a new one. The terminal needs to reload its paths after installing new tools.

### "Cannot connect to the Docker daemon"

Docker Desktop is not running. Open Docker Desktop and wait for it to fully start (the whale icon should stop animating). Then try your command again.

### Port 3000 is already in use

Another app is using port 3000. Find and stop it, or tell Claude Code: "Change the app to use port 3001 instead of 3000."

### Port 5432 is already in use

Another PostgreSQL instance is running — possibly from another project. Run `docker compose down` in the other project folder first, or tell Claude Code to use a different port for PostgreSQL.

### Prisma migration fails

The database might be in a bad state. Try:

```bash
npx prisma migrate reset
```

This deletes all data and recreates the tables. If that doesn't work:

```bash
docker compose down -v
docker compose up -d
npx prisma migrate dev
```

The `-v` flag removes the database volume (all data), giving you a fresh start.

### The page is blank or shows "Module not found"

Run `npm install` to make sure all dependencies are present, then try `npm run dev` again.

### Data disappears after restarting the dev server (Part 4)

This is expected! In-memory storage resets when the server restarts. This is exactly why you add a database in Part 5.

### Data disappears after `docker compose down -v`

The `-v` flag deletes volumes, including your database data. Use `docker compose down` (without `-v`) to stop containers while keeping your data.

### Authentication redirect loop (page keeps reloading)

The `NEXTAUTH_URL` in your `.env` file might not match the actual URL. Make sure it says `http://localhost:3000` for local development.

### "Invalid environment variable" or NextAuth errors

Make sure your `.env` file has `NEXTAUTH_SECRET` set to a random string (at least 32 characters). You can generate one with:

```bash
openssl rand -base64 32
```

### Docker build fails or runs out of memory

Docker Desktop may need more memory. Open Docker Desktop → **Settings** → **Resources** and increase the memory limit to at least 4 GB.

### The deployed app shows "502 Bad Gateway"

The app container might still be starting — wait 30 seconds and refresh. If it persists, check the service logs in the Sliplane dashboard for error messages.

---

## Clean Up

If you want to remove what you've created after the exercise:

### Stop and remove Docker containers

```bash
docker compose down -v
```

The `-v` flag removes the database volume and all stored data.

### Delete the repository

```bash
gh repo delete my-time-tracker --yes
```

### Delete Sliplane resources (if deployed)

Go to the Sliplane dashboard and delete your services and server.

### Remove the local folder

```bash
cd ..
rm -rf my-time-tracker
```
