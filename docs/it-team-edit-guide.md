# IT / Dev Team Edit Guide

This is the day-to-day playbook for changing things on the KCee landing
pages. It assumes you already have access to the GitHub repo at
[github.com/DeepakMurugan/realestate](https://github.com/DeepakMurugan/realestate).

## The big picture

You edit files in GitHub. Cloudflare watches the repo. The moment you push
to the `main` branch, the live site updates within about one minute. There
is no FTP, no Cloudflare login needed, no manual upload.

If you break something, every previous version of every file is still in
GitHub. You can roll back any change with one click.

## Where things live

```
lp/
  kcee-alankrita/
    index.html         <- the page itself (HTML, text, layout, links)
    thank-you.html     <- the page shown after a form submission
    img/               <- all images for this LP
    fonts/             <- custom fonts for this LP
```

Each project is a separate folder under `lp/`. Edits to one project never
affect another.

## Two ways to edit

### Option A: edit directly on GitHub (easiest, no install)

1. Open the repo on github.com and navigate into
   `lp/kcee-alankrita/` (or your project folder).
2. Click the file you want to edit (for example `index.html`).
3. Click the pencil icon at the top right of the file view.
4. Make your change.
5. Scroll to the bottom, write a short message like
   `update phone number`, and click **Commit changes**.
6. Wait about one minute. Refresh the live site to confirm.

This is the fastest path for small text edits. No tools to install.

### Option B: clone the repo and use VS Code (for bigger changes)

1. Install [GitHub Desktop](https://desktop.github.com/) or use the `git`
   command line.
2. Clone `https://github.com/DeepakMurugan/realestate`.
3. Open the folder in VS Code (or any editor).
4. Make changes, save, commit, push to `main`.

Use Option B when you are replacing images, changing many lines, or want
to preview locally before pushing. To preview locally, just double-click
`lp/kcee-alankrita/index.html` and it opens in your browser.

---

## Common edits

### 1. Change the phone number

The current phone number `7740077499` appears in **three places** in
`lp/kcee-alankrita/index.html`. All three must be updated together or the
page will be inconsistent. (Line numbers below are approximate, since the
file changes over time. Search for the number itself, not the line.)

| Where | What it does |
|---|---|
| Around line 350 (`<a href="tel:7740077499"`) | Top header click-to-call link |
| Around line 368 (visible text `7740077499`) | The number shown to the user |
| Around line 1032 (`<a href="tel:7740077499"`) | Sticky bottom call button on mobile |

**How to do it safely:** Use Find and Replace (Ctrl+H on Windows,
Cmd+F then the dropdown on GitHub web editor) and replace all three
occurrences of the current number with the new one in a single action.

After replacing, eyeball the file once to confirm it shows up three times
and nowhere else.

### 2. Replace an image

Two images are actively used on the current LP:

| File | What it is |
|---|---|
| `lp/kcee-alankrita/img/finalhero.jpeg` | The main hero image at the top of the page |
| `lp/kcee-alankrita/img/kcclogo.webp` | The KCee logo (appears in header and footer) |

You will also see an older file `lp/kcee-alankrita/img/heroba.jpeg` in
the folder. It is no longer referenced by the HTML and can be ignored.
It is kept in the repo for history. Safe to delete when you do a
cleanup pass, but not urgent.

**To replace an active image:**

1. Prepare the new image with the **exact same filename and extension**.
   If your new hero image is a PNG, rename it to `finalhero.jpeg` first.
   This is the simplest path because no HTML changes are needed.
2. On GitHub: navigate into `lp/kcee-alankrita/img/`, click the old file,
   click the trash icon to delete it, then **Add file > Upload files** and
   drop the new file in.
3. Commit. Done.

**To use a new filename instead:** Upload the new file, then open
`index.html` and change every reference to the old filename. References
look like `src="./img/finalhero.jpeg"`.

**Image quality tip:** Compress your image first at
[tinypng.com](https://tinypng.com/). A 2 MB hero image makes the page slow
on mobile.

### 3. Change page text or headlines

Open `lp/kcee-alankrita/index.html` in the GitHub editor. Use Ctrl+F to
search for the existing text you want to change. Edit between the HTML
tags (do not touch the tags themselves, which look like `<h1>` or `<p>`).
Commit.

**Example.** To change the headline from "Premium 2BHK Apartments" to
something else, search for `Premium 2BHK Apartments`, edit only the text,
leave the surrounding `<...>` tags alone.

### 4. Change the page title and SEO description

These show up on Google and on the browser tab.

- **Browser tab title:** line 7 of `index.html`, inside `<title>...</title>`.
- **Meta description:** search for `<meta name="description"` if present.

### 5. Update the contact form

The form on the page is an embedded **Sell.Do** form. The form fields
themselves (name, phone, email, etc.) are managed in the Sell.Do dashboard,
not in this HTML. You cannot add or remove fields from the GitHub side.

What you **can** change from the HTML:

- The heading and supporting text around the form.
- The thank-you page redirect: search for `thank-you.html` in
  `index.html`. That is the page the user lands on after submitting.
- The Sell.Do form IDs themselves. There are **two** IDs you may see:
  - **Container ID** `69dc9028e11487515f3e97a9` — appears once in an
    active line and once commented out (under `SELL.DO TRACKING CODE`).
  - **Form ID** `6a0abe6b735dafac03894bec` — appears in the script that
    actually loads the form (one active reference + one commented-out
    older variant the dev left behind).

  If Sell.Do gives you new IDs, search for the **active** (uncommented)
  references and replace them. Leave the commented-out lines alone, or
  delete them if you want to tidy up.

To change the thank-you page content, edit `lp/kcee-alankrita/thank-you.html`
directly.

### 6. Change Google Tag Manager (analytics)

The GTM container ID is `GTM-57RV4XNS`. It appears twice in `index.html`
(around lines 282 and 293). If you ever move to a new GTM container, find
and replace both occurrences.

### 6a. A note on commented-out code

In `index.html` you will see some HTML and JavaScript lines that are
commented out. They look like `<!-- ... -->` for HTML and `// ...` for
JavaScript. These are old versions of code that the previous developer
left as a reference. They do not run. Do not "uncomment" them unless you
know what you are doing, and you can safely delete them during a cleanup
pass.

### 7. Add a brand new landing page for another project

1. In the GitHub web UI, navigate to the `lp/` folder.
2. Click **Add file > Create new file**.
3. In the filename box, type `<your-project-slug>/index.html` (the slash
   creates the folder for you). Slug must be lowercase, hyphens only, no
   spaces. Example: `skyview-residency/index.html`.
4. Easiest way to start: open `lp/kcee-alankrita/index.html` in another
   browser tab, copy the entire contents, and paste into your new file as
   a starting point. Then change text, images, phone numbers as needed.
5. Upload your project's images into a sibling `img/` folder
   (`lp/<your-slug>/img/`).
6. Commit. Within a minute the new page is live at
   `kceeproperties.com/lp/<your-slug>/`.

---

## Safety net: rolling back a bad change

If a change went wrong:

1. On GitHub, click **Commits** at the top of the repo.
2. Find the bad commit.
3. Click the commit, then click **Revert** (or click the `...` next to the
   file you broke and pick **View at this point in history > Restore**).
4. Push the revert. The site reverts within a minute.

You cannot lose work permanently. Every save is tracked.

## Things to avoid

- Do not rename folders inside `lp/`. The folder name **is** the URL. If
  you rename `kcee-alankrita/` to `kcee-alankrita-v2/`, the old URL
  immediately stops working and any ads or printed flyers pointing to the
  old URL break.
- Do not edit files outside `lp/` unless you know what they do. The Worker
  and routing config live elsewhere in the project.
- Do not commit binary files larger than 5 MB. Compress images first.
- Do not paste secrets (API keys, passwords) into any file. The repo is
  public.
- The `package-lock.json` file at the repo root is empty and harmless. No
  build tools are used. Do not modify or rely on it.

## Who to contact

Anything you are not sure about, ping Sri (the developer) before pushing.
A 30-second check beats a broken live site.
