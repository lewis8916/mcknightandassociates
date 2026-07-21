# Deploying to Cloudflare Pages

This site is designed to deploy to Cloudflare Pages (free tier) via a connected GitHub repo. Every push to `main` builds and deploys automatically.

---

## Step 1 — Push this project to GitHub

From this directory (`C:\Users\lewis\dev\mcknight-website`):

```powershell
git init
git add .
git commit -m "Initial commit: McKnight & Associates site"
```

Then create the remote repo. Easiest way (uses the GitHub CLI that's already installed):

```powershell
gh auth status                                          # confirm you're logged in
gh repo create mcknight-website --private --source=. --remote=origin --push
```

If you'd rather use github.com directly: create an empty repo named `mcknight-website`, then:

```powershell
git remote add origin https://github.com/<your-username>/mcknight-website.git
git branch -M main
git push -u origin main
```

---

## Step 2 — Connect the repo to Cloudflare Pages

1. Go to <https://dash.cloudflare.com/> → **Workers & Pages** → **Create** → **Pages** tab → **Connect to Git**
2. Authorize Cloudflare to access your GitHub account and pick the `mcknight-website` repo
3. On the build config screen, use:

| Setting                    | Value             |
|----------------------------|-------------------|
| Framework preset           | **Astro**         |
| Build command              | `npm run build`   |
| Build output directory     | `dist`            |
| Root directory             | *(leave blank)*   |
| Node version (env var)     | `NODE_VERSION=20` |

4. Click **Save and Deploy**. First build takes ~1–2 minutes.
5. You'll get a URL like `mcknight-website.pages.dev` — check it renders correctly.

---

## Step 3 — Point mcknight.com at the site

In the Pages project → **Custom domains** → **Set up a custom domain** → enter `mcknight.com`.

- **If mcknight.com is already on Cloudflare DNS:** Cloudflare adds the CNAME/apex record automatically. You're done in ~1 minute.
- **If mcknight.com is on a different DNS host:** Cloudflare shows you a CNAME record to add. Add it at your registrar, wait for DNS to propagate (usually minutes, up to a few hours).

Also add `www.mcknight.com` as a second custom domain and set it to redirect to the apex — Cloudflare has a one-click option for this in the Custom domains screen.

---

## Step 4 — Wire up the contact form

The contact form currently posts to a **placeholder** Formspree URL that won't work. Pick one:

- **Formspree** (easiest): sign up at <https://formspree.io>, create a form, copy your form ID, and replace `your-form-id` in `src/pages/contact.astro` (the `action` attribute of the `<form>`)
- **Cloudflare Pages Functions**: add a `functions/contact.ts` file that receives the POST and forwards to your email provider (more control, no third party)
- **Basin, Web3Forms, etc.**: any static-form service works — just swap the `action` URL

---

## Ongoing: how deploys work

- Every push to `main` → Cloudflare builds and deploys to production (`mcknight.com`)
- Every push to any other branch → Cloudflare deploys a preview URL like `<hash>.mcknight-website.pages.dev` so you can review before merging
- You can roll back to any previous deployment from the Pages dashboard with one click

---

## Local checks before you push

```powershell
npm run build        # make sure the build passes
npm run preview      # serve the built site at http://localhost:4321
```

If `npm run build` fails, Cloudflare's build will fail too — fix it locally first.
