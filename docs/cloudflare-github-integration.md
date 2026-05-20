# Connecting the GitHub Repo to Cloudflare Pages

This is a **one-time setup**. After it is done, every push to the `main`
branch of `github.com/DeepakMurugan/realestate` automatically deploys to
the live site. No manual upload is ever needed again.

## What you need before starting

- Owner or Admin access to the Cloudflare account that holds the
  `kceeproperties.com` zone and the `kcee-landing-pages` Pages project.
- The GitHub account `DeepakMurugan` (or an account with admin rights on
  the `realestate` repo) so you can authorize Cloudflare to read it.
- About 10 minutes.

> Important context for whoever does this: there is already a Pages
> project called `kcee-landing-pages` that was created via direct upload.
> We are **replacing** its source from direct upload to GitHub, in place,
> so the live URL and the Worker that points to it keep working without
> any other change.

## Repo layout this assumes

```
realestate/                  <- repo root
  README.md
  docs/
    it-team-edit-guide.md
    cloudflare-github-integration.md   <- this file
  lp/
    kcee-alankrita/
      index.html
      thank-you.html
      img/
      fonts/
```

If you cloned this repo and see a different layout, stop and check with
Sri. The deployment is designed around the `lp/<slug>/` structure.

## Step-by-step setup

### 1. Sign in to Cloudflare and confirm you are in the right account

Go to [dash.cloudflare.com](https://dash.cloudflare.com) and check the
account switcher in the top right. You must be in the KCee Properties
account, not any other client's account.

If you are not sure, sign out and back in with the KCee account
credentials. Acting in the wrong account can break unrelated client
sites.

### 2. Open the existing Pages project

In the left sidebar pick **Workers & Pages**, then click into the
`kcee-landing-pages` project.

If the project does not exist yet, create it: **Create application > Pages
> Connect to Git**, then skip to step 4. If it does exist (the normal
case), continue to step 3.

### 3. Disconnect the current direct-upload source (only if needed)

Open the **Settings** tab inside the project. Scroll to **Builds &
deployments**. If the project is currently in direct-upload mode, you
will see an option labeled **Connect to Git** or **Change source**. Click
it.

You will be asked to authorize Cloudflare to access GitHub. Approve.

### 4. Choose the repo

- **Git account:** select the GitHub account that owns
  `DeepakMurugan/realestate`. If it is not listed, click **Add account**
  and grant Cloudflare access to that repo.
- **Repository:** pick `DeepakMurugan/realestate`.
- Click **Begin setup**.

### 5. Configure the build

Fill in the form as follows:

| Field | Value | Notes |
|---|---|---|
| **Project name** | `kcee-landing-pages` | Must match the existing project name so the Worker route still resolves. Cloudflare will pick this up automatically if you came in via Settings. |
| **Production branch** | `main` | Pushes to other branches create preview deployments only. |
| **Framework preset** | `None` | This is a static HTML site, no build framework. |
| **Build command** | *(leave empty)* | Nothing to build. The HTML is served as-is. |
| **Build output directory** | `/` | Serve the entire repo root. The `lp/<slug>/` paths inside the repo match the live URL paths exactly. |
| **Root directory** | *(leave as default, which is `/`)* | |
| **Environment variables** | *(none)* | |

Click **Save and Deploy**.

### 6. Wait for the first deployment

The first deploy takes 30 to 90 seconds. When it completes you should see
a green check next to a commit hash on the project's **Deployments** tab.

### 7. Verify

Open these two URLs in a private/incognito window:

- `https://kcee-landing-pages.pages.dev/lp/kcee-alankrita/`
  This is the raw Pages URL. It should show the landing page.
- `https://kceeproperties.com/lp/kcee-alankrita/`
  This is the live URL. The Worker should be proxying it to the Pages URL
  above. It should also show the landing page.

Both should look identical. If only the `pages.dev` URL works, the Worker
route is the issue, not Pages. Ping Sri.

### 8. Tell the team it is live

Once verified, the workflow is now: edit on GitHub, push to `main`, live
within a minute. See `docs/it-team-edit-guide.md` for the editor
playbook.

## Rolling back a bad deploy

Two ways:

1. **Revert the commit in GitHub** (preferred). On the repo's Commits
   page, find the bad commit and click **Revert**. Push. A new deploy
   goes out within a minute restoring the previous state. This keeps the
   history honest.

2. **Roll back via Cloudflare**. On the Pages project's **Deployments**
   tab, find the last good deployment, click the `...` menu and choose
   **Rollback to this deployment**. This is instant but does not change
   the code in GitHub, so the next push will re-deploy the broken state.
   Use this only as a fast emergency stop, then follow up with a Git
   revert.

## What this setup does **not** change

- The Worker at `kcee-lp-router` still owns the `/lp/*` route on
  `kceeproperties.com`. We did not touch it.
- Framer continues to serve every other path on `kceeproperties.com`.
- DNS at the registrar is unchanged.

If anything outside `/lp/*` looks different after this setup, it is not
related to this change.

## Common gotchas

- **404 on assets after deploy.** Usually means a file was renamed in the
  repo but the HTML still references the old name. Check the browser's
  network tab for the missing path.
- **Old version still showing.** Cloudflare caches aggressively. In the
  Pages project go to **Settings > Functions > Cache** or purge the zone
  cache from the main Cloudflare dashboard. Hard refresh (Ctrl+Shift+R)
  for a quick local check.
- **Preview URL works, production URL does not.** The Worker route is
  missing or misconfigured. The Worker config is in `lp-router/` in Sri's
  working repo, not in this repo.
