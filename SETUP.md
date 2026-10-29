# Advice in Bloom — Setup Guide

Set aside about 30 minutes. You do every step that involves an account, a password, or an API key; nobody else ever needs to see them.

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The website itself |
| `favicon.svg`, `sitemap.xml` | Browser icon, and a file that helps Google find the site |
| `apps-script/Code.gs` | The moderator and storage. A copy is kept here for safekeeping; the working copy runs inside your Google Sheet |

---

## Step 1: Create the Google Sheet (the database)

1. Go to sheets.google.com and create a blank spreadsheet. Name it **Advice in Bloom – Entries**.
2. Click **Extensions → Apps Script**.
3. Delete the sample code, paste in everything from `apps-script/Code.gs`, and click **Save** (the disk icon).
4. In the function dropdown at the top, choose **setup** and click **Run**. Google asks you to authorize it: choose your account, click **Advanced → Go to project (unsafe)**, then **Allow**. "Unsafe" only means you wrote the script yourself and Google hasn't reviewed it.
5. Return to the Sheet. You'll see three tabs, **Approved**, **Pending** and **Rejected**, plus an **Advice in Bloom** menu (reload the page if the menu isn't there yet).

## Step 2: Connect the AI moderator

1. Sign in at console.anthropic.com.
2. Go to **Settings → Limits** and set a monthly **spend limit** you're comfortable with (for example $5).
3. Go to **API Keys → Create Key**. Name it `advice-in-bloom` and copy it.
4. In Apps Script, click the gear icon (**Project Settings**). Scroll to **Script Properties → Add script property**:
   - Property: `ANTHROPIC_API_KEY`, Value: *paste your key*
5. Optional properties:
   - `MODEL`: which Claude model moderates. The default is `claude-opus-5-5`, the most careful option. `claude-haiku-5-5` costs about 1/40th as much.
   - `MAX_AI_CHECKS_PER_DAY`: a safety cap on daily AI checks (default 300). Past the cap, entries wait in Pending for you.
6. Click **Save script properties**.
7. Back in the Sheet, click **Advice in Bloom → Test the AI moderator**. You should see the good sample approved and the bad sample rejected.

**Never paste your API key into `index.html`, GitHub, an email, or a chat.** It belongs only in Script Properties.

## Step 3: Publish the moderator as a web app

1. In Apps Script, click **Deploy → New deployment**.
2. Click the gear next to "Select type" and choose **Web app**.
3. Set **Execute as: Me** and **Who has access: Anyone**.
4. Click **Deploy** and copy the **Web app URL** (it ends in `/exec`).
5. Open `index.html`, find `API_URL: ""` near the top of the script, and paste the URL between the quotes. (Or send the URL to Claude, which can add it for you. The URL is not secret.)

> When you change `Code.gs` later, use **Deploy → Manage deployments → Edit (pencil) → Version: New version → Deploy**. That keeps the same URL.

## Step 4: Put the site on GitHub Pages

The site files live in the repo `kimberlybruton-sys/adviceinbloom.github`, on the **site-launch** branch.

1. Make sure the repository is **Public** (**Settings → General → Danger Zone → Change visibility**). Free GitHub Pages requires a public repo.
2. Go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, branch **site-launch** (or **main**, if you've merged it), folder **/ (root)**, then **Save**.
3. After a minute or two, the site is live at **`https://kimberlybruton-sys.github.io/adviceinbloom.github/`**.

> Wait to turn on Pages until Step 3 is done and the web app URL is in `index.html`. Before then, the page runs in preview mode, where entries are saved only in each visitor's own browser.

## Step 5: Help people find it on Google

1. Go to search.google.com/search-console and add your site address as a **URL prefix** property.
2. Verify it with the **HTML tag** method: copy the `<meta name="google-site-verification" ...>` tag into the `<head>` of `index.html`, then re-upload the file.
3. Under **Sitemaps**, submit `sitemap.xml`.
4. Share the link: friends, social media, email signatures. Links from other sites help Google rank it.

---

## Looking after the garden (a few minutes a week)

- **Pending tab:** entries the moderator couldn't decide on, or that arrived while it was unavailable or over the daily cap. Tick **Approve?** on the ones you like, then click **Advice in Bloom → Publish ticked Pending entries**. Delete the rest.
- **Approved tab:** everything that's live. To remove an entry, untick **Visible** or delete the row. Entries reported by 2 visitors hide themselves automatically and say so in the **Note** column.
- **Rejected tab:** what the moderator blocked and why. If something was blocked unfairly, copy it to Approved and tick Visible.
- **Costs:** check console.anthropic.com occasionally. When your credit runs out, nothing breaks: new entries simply wait in Pending until you top up.

## How long will it last?

GitHub Pages and Google Sheets/Apps Script are free, have no expiry date, and need no server maintenance. Your entries stay in your own Google account. A Google Sheet holds 10 million cells, which works out to more than a million entries.
