# KCee Properties Landing Pages

This repo holds all landing pages for KCee Properties projects. Every push to
`main` deploys automatically to Cloudflare Pages and goes live on
`kceeproperties.com/lp/<project-slug>/`.

## Folder rule

The folder path inside `lp/` is the URL path on the live site. That is the
only rule. If you remember this, you cannot go wrong.

```
lp/
  kcee-alankrita/        ->  https://kceeproperties.com/lp/kcee-alankrita/
    index.html
    thank-you.html
    img/
    fonts/
  <new-project-slug>/    ->  https://kceeproperties.com/lp/<new-project-slug>/
    index.html
    ...
```

## Adding a new landing page

1. Copy an existing project folder (for example `lp/kcee-alankrita/`) and
   rename the copy with your new project slug. Use lowercase letters,
   numbers, and hyphens only. No spaces.
2. Edit the files inside the new folder.
3. Commit and push to `main`. The new page is live within about one minute
   at `kceeproperties.com/lp/<your-new-slug>/`.

## Daily edits

See [`docs/it-team-edit-guide.md`](docs/it-team-edit-guide.md) for the
day-to-day playbook (changing images, phone numbers, the contact form, and
so on).

## How deployment works (one-liner)

GitHub `main` is connected to a Cloudflare Pages project called
`kcee-landing-pages`. A Cloudflare Worker on the live domain forwards every
request under `/lp/*` to that Pages project. You do not need to touch
Cloudflare to ship a change. Just push to GitHub.
