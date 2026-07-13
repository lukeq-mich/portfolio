# Portfolio on GitHub Pages — Complete Setup Guide

## What you are building

Your portfolio lives entirely inside one GitHub repository. There is no third-party database and no server to maintain.

- **`index.html`** — the public portfolio recruiters see, styled as an editorial "drawing register."
- **`admin.html`** — your private editor. It has a password login and lets you create, read, update, and delete projects, skills, tools, artefact links, and site details without ever touching a file.
- **`data/data.json`** — the single source of truth. All content lives here. The editor reads it and writes it back for you.
- **`.github/workflows/deploy.yml`** — the workflow. Every time the editor saves (which is a real git commit), this workflow automatically republishes the public site. Edits go live in about a minute.

How editing works under the hood: the editor page talks directly to GitHub's own API using a personal access token that only you hold. When you press **Save & publish**, it commits the updated `data.json` to the repository, which triggers the workflow, which redeploys the site. You never open the JSON file yourself.

**Roles:** everyone who visits the site is a viewer. The editor is a separate page (`admin.html`), reached from the small **Editor login** button in the site footer or directly by its address, and locked behind your password. A **View site** button in the editor's header takes you back to the public view at any time. Without the password (and behind that, the token), the editor cannot read or change anything.

---

## Part 1 — Create the repository

1. Go to https://github.com and sign in (create a free account if you don't have one).
2. Click the **+** icon in the top-right corner, then **New repository**.
3. Set **Repository name** to `portfolio` (lowercase, no spaces). You can choose another name, but this guide assumes `portfolio`.
4. Set visibility to **Public**. This matters: free GitHub accounts can only use GitHub Pages on public repositories. (Public means people could view the repository files if they went looking — your public site shows the same content anyway, and your token is never stored in the repository.)
5. Tick **Add a README file**. This gives the repository a `main` branch immediately, which you need.
6. Click **Create repository**. You'll land on the repository's front page.

## Part 2 — Add the four files

You have five files from me: `index.html`, `admin.html`, `data.json`, `deploy.yml`, and the zip containing all of them in the correct structure. The safest path in the web interface is below — the workflow file needs special handling because its folder starts with a dot, which some computers hide from upload windows.

**Upload the three ordinary files:**

7. On your repository's front page, click **Add file** (near the green Code button), then **Upload files**.
8. Drag `index.html` and `admin.html` into the upload area.
9. Click the green **Commit changes** button at the bottom. Leave the message as suggested.
10. Now the data file, which must sit inside a folder called `data`. Click **Add file → Create new file**.
11. In the filename box at the top, type exactly: `data/data.json` — the moment you type the `/`, GitHub turns `data` into a folder.
12. Open the `data.json` file I gave you in any text editor (Notepad is fine), select everything (Ctrl+A), copy it, and paste it into the big text area on GitHub.
13. Click **Commit changes**, then **Commit changes** again in the dialog.

**Create the workflow file:**

14. Click **Add file → Create new file** again.
15. In the filename box, type exactly: `.github/workflows/deploy.yml` — including the leading dot. GitHub will create both folders as you type each `/`.
16. Open the `deploy.yml` I gave you, copy all of it, and paste it into the text area. Indentation matters in this file, so paste rather than retype.
17. Click **Commit changes**, then confirm.

Your repository should now show: `README.md`, `admin.html`, `index.html`, a `data` folder, and a `.github` folder.

## Part 3 — Turn on GitHub Pages

18. In your repository, click the **Settings** tab (top row, far right).
19. In the left sidebar, click **Pages**.
20. Under **Build and deployment → Source**, open the dropdown and choose **GitHub Actions** (not "Deploy from a branch"). This tells Pages to use your workflow file.
21. Click the **Actions** tab at the top of the repository. You may already see a run of "Deploy portfolio to GitHub Pages." If not, click the workflow name in the left sidebar, then the **Run workflow** button, then the green **Run workflow** confirm.
22. Wait for the run to show a green tick (usually under a minute).
23. Your site is now live at: `https://YOUR-USERNAME.github.io/portfolio/` — replace YOUR-USERNAME with your GitHub username, all lowercase. Open it and you should see the portfolio with my sample content.

Your editor lives at: `https://YOUR-USERNAME.github.io/portfolio/admin.html` — also reachable from the **Editor login** button in the footer of the public site.

## Part 4 — Create your access token (the editor's key)

This token is what gives the editor permission to write to your repository. It is scoped to this one repository and nothing else.

24. Click your profile picture (top-right of GitHub) → **Settings**.
25. Scroll to the very bottom of the left sidebar → **Developer settings**.
26. Click **Personal access tokens → Fine-grained tokens**, then the **Generate new token** button.
27. Fill it in:
    - **Token name:** `portfolio editor`
    - **Expiration:** choose **Custom** and set it about a year out (the maximum). Put a reminder in your calendar for a week before — renewing takes two minutes (see Part 7).
    - **Resource owner:** your own account.
    - **Repository access:** choose **Only select repositories**, then pick `portfolio` from the dropdown.
    - **Permissions:** expand **Repository permissions**, find **Contents**, and set it to **Read and write**. Leave everything else at "No access."
28. Click **Generate token** at the bottom.
29. GitHub shows the token exactly once — a long string starting with `github_pat_`. Click the copy icon. Keep this tab open until Part 5 is done, or paste it somewhere temporary and private.

Never paste this token into the repository, an email, or anywhere public. The editor never stores it in the repository either — only encrypted in your own browser.

## Part 5 — Log in to the editor (one-time setup)

30. Open `https://YOUR-USERNAME.github.io/portfolio/admin.html`.
31. You'll see **First-time setup**. Fill it in:
    - **GitHub username:** your username exactly as it appears in your profile URL.
    - **Repository name:** `portfolio`
    - **Branch:** `main` (already filled in)
    - **Fine-grained personal access token:** paste the token from Part 4.
    - **Choose a password / Confirm:** any password you'll remember. This unlocks the editor on this device from now on.
32. Click **Verify & save connection**. The editor checks the token against GitHub, loads your data, encrypts the token with your password (AES-256, done entirely inside your browser), and opens the editor.

From now on, visiting the editor on this device shows a simple password prompt. Wrong password: nothing unlocks. **Lock** (top right) signs you out. **Reset connection** on the login screen wipes the stored credentials from the device if you ever want to start fresh — you'd need the token again (or a new one) to reconnect.

To edit from a second device (say, your phone), just repeat this Part on that device — the token setup is once per device.

## Part 6 — Using the editor day to day

The editor has four tabs. Every tab follows the same rhythm: make your edits, click **Apply** on the form, and when you're done editing, click **Save & publish** in the top bar. Applying stages the change; Save & publish makes the actual commit and triggers the republish. The red **● Unsaved changes** marker in the header tells you there's something staged but not yet published.

**Projects tab.** Click **Add project** to open the full form: name, tagline, description, category, type, status, year of study, role, outcome, and a Featured toggle (featured projects appear first on the site with a red stamp). Below that is the cover image: drop an image onto the box (or click it to browse) and it uploads into a `cover-images` folder in the repository, with a preview and a Remove button shown once it's up — the image appears at the top of the project on the public site. Below the fields you'll see checkboxes for every skill and tool in your catalogue — tick the ones this project used; that's the many-to-many link, and the public site renders them as tags on the project. At the bottom, add artefacts. The quickest way is the drop box: drag one or several files onto it (or click it to browse) and each file uploads into the `artefacts` folder in the repository, appearing as a row with its title and type filled in automatically from the filename — adjust either if the guess is wrong. The **Upload** button on an existing row swaps in a different file, and **+ Add link-only artefact** creates a row where you paste an external link instead (useful for code repositories or large videos; uploads are capped at 25 MB). The public site numbers artefacts automatically like drawing sheets (RPT-01, DRG-01). Edit and Delete buttons sit beside every project in the list.

**Skills and Tools tabs.** Each is a simple catalogue: name, category (or type, for tools), and proficiency. The list shows how many projects use each entry. Deleting one warns you and cleanly removes it from any projects that referenced it — no broken links, ever.

**Site details tab.** Your name, the register title, the introduction paragraph, and contact email.

After **Save & publish**, give it about a minute, then refresh the public site to see the change live. You can watch the republish happen in the repository's **Actions** tab if you're curious.

**On uploads:** one difference from everything else in the editor — uploading a file (artefact or cover image) commits it to the repository *immediately* (that's how it gets stored), while the project entry that points to it still waits for **Save & publish** like any other change. So if you upload a file and then cancel the form, the file will sit unused in its folder (`artefacts` or `cover-images`); harmless, and you can delete it from the folder on GitHub if you like. Deleting a project or artefact row likewise removes the *reference*, not the stored file.

## Part 7 — When the token expires

The editor will tell you plainly: "GitHub rejected the token — it may have expired." To fix: repeat Part 4 to generate a fresh token (or use the **Regenerate** button on the old token's page), then on the editor login screen click **Reset connection** and run the first-time setup again with the new token. Two minutes, once a year.

## Troubleshooting

**The public site shows "Could not load data/data.json."** The data file isn't where the page expects. Check your repository shows a `data` folder containing `data.json` — the exact path matters.

**The editor says it can't find the file or repository.** Re-check the username and repository name in setup — they must match your repository's URL exactly, and the branch must be `main`.

**"GitHub rejected the token."** Either the token expired (Part 7) or, when creating it, Contents wasn't set to Read and write, or the wrong repository was selected. Create a fresh one and reset the connection.

**"Save conflict."** The file changed on GitHub after the editor loaded it — this only happens if you edited from two places at once. Click **Lock**, unlock again (which reloads fresh data), and re-apply the change.

**I saved but the public site hasn't changed.** Wait a full minute, then hard-refresh (Ctrl+Shift+R). Check the **Actions** tab — the newest run should be green. If a run is red, open it and read the error; if Pages was never set to "GitHub Actions" as the source (Part 3, step 20), fix that first.

**The Actions tab shows nothing at all.** The workflow file is missing or misplaced. It must be at exactly `.github/workflows/deploy.yml`.
