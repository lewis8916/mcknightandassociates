# McKnight & Associates

Marketing site for McKnight & Associates — a Dallas-based real estate brokerage serving Richardson, Plano, Garland, and Frisco.

Built with [Astro](https://astro.build), plain CSS, and deployed to [Cloudflare Pages](https://pages.cloudflare.com/).

## Local development

```bash
npm install
npm run dev
```

Then open <http://localhost:4321>.

## Build

```bash
npm run build
```

Output goes to `dist/`.

## Project structure

```
src/
  layouts/     — shared HTML shell (Layout.astro)
  components/  — Nav, Footer, AgentCard
  pages/       — one .astro file per route (index, agents, about, contact)
  styles/      — global.css (design tokens + shared styles)
public/        — static assets copied as-is (favicon, robots.txt, _headers)
```

## Editing content

- Copy for each page lives inline in the `.astro` file for that route
- Agent list: `src/pages/agents.astro` — the `agents` array at the top
- Colors & typography: CSS variables at the top of `src/styles/global.css`
- Contact form: currently posts to a Formspree placeholder — replace the `action` URL in `src/pages/contact.astro` with a real endpoint before launch

## Deploy to Cloudflare Pages

See `DEPLOY.md` for the full walkthrough.

Short version:
1. Push this repo to GitHub
2. In Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git
3. Build command: `npm run build`
4. Build output directory: `dist`
5. Add custom domain `mcknight.com`
