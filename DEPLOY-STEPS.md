# Deploy steps — GitHub + Vercel

The repo folder is ready at `~/Documents/Claude/Projects/Dating Event/byos-website-repo/`.
Open Terminal and copy-paste these blocks in order.

## 0. Make sure you have the tools

```bash
# Check if these are installed; if not, install them.
git --version
gh --version          # GitHub CLI — https://cli.github.com  (brew install gh)
vercel --version      # Vercel CLI — npm i -g vercel
```

If any are missing on macOS:

```bash
brew install git gh
npm install -g vercel
```

## 1. Authenticate (one-time)

```bash
gh auth login         # pick GitHub.com, HTTPS, "Login with a web browser"
vercel login          # opens a browser to log into Vercel
```

## 2. Initialize the repo and push to GitHub

```bash
cd "$HOME/Documents/Claude/Projects/Dating Event/byos-website-repo"

git init -b main
git add .
git commit -m "Initial commit: Bring Your Singles landing page"

# Creates the public repo on your GitHub account, sets origin, pushes main
gh repo create dating-events --public --source=. --remote=origin --push
```

After this step, `https://github.com/<your-username>/dating-events` will exist.

## 3. Deploy to Vercel

Run from the same folder:

```bash
vercel --yes          # first deploy — creates a Preview URL
vercel --prod         # promotes the latest deploy to Production
```

When `vercel --yes` asks "Link to existing project?" — answer **No**, then accept the defaults (project name `dating-events` is fine, root directory `./`, no build/output overrides — it autodetects as a static site).

You'll get a URL like `https://dating-events-<hash>.vercel.app`. The `--prod` step gives you `https://dating-events.vercel.app` (or whatever name Vercel assigns).

## 4. Wire GitHub → Vercel for auto-deploys (optional but recommended)

So future `git push` automatically deploys:

1. Open https://vercel.com/dashboard → click the `dating-events` project.
2. **Settings → Git → Connect Git Repository** → pick the `dating-events` GitHub repo.
3. Done. Every push to `main` now redeploys to production.

## 5. (Later) Custom domain `bringyoursingles.com`

In the Vercel project: **Settings → Domains → Add** → enter `bringyoursingles.com`. Vercel will show you the DNS records to set at your registrar (an A record or CNAME). Once DNS propagates, it's live and HTTPS is automatic.

---

## If something goes wrong

- **`gh: command not found`** — install with `brew install gh`, then `gh auth login`.
- **`vercel: command not found`** — `npm install -g vercel`. If npm isn't installed, install Node from https://nodejs.org first.
- **`gh repo create` says the name is taken** — pick a different name: `gh repo create dating-events-denver --public --source=. --remote=origin --push`.
- **Want to redo from scratch** — delete the repo on GitHub, run `rm -rf .git .vercel` in this folder, start over at step 2.

## Web-UI fallback (no CLI needed)

If you'd rather click than type:

1. Go to https://github.com/new → name: `dating-events`, public, **don't** initialize with README. Create.
2. On the empty repo page, click "uploading an existing file" → drag `index.html`, `README.md`, `.gitignore` from this folder. Commit.
3. Go to https://vercel.com/new → "Import Git Repository" → pick `dating-events`. Accept the defaults. Click Deploy.

Same result, no terminal.
