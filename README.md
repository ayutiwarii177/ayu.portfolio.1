# Ayu Tiwari: portfolio

An immersive, dark, 3D-flavoured portfolio. One idea runs through it: Ayu is an evolving builder who thinks
critically, experiments with technology and turns ideas into projects.

Sections: cinematic entry, About, Skills (editable levels), **The Thinking Engine** (an interactive 3D model of
Observe, Question, Analyse, Experiment, Build, Improve), Projects (full-screen detail views), Timeline, Contact.

## Run it

```bash
npm install
npm run dev        # build, watch and serve at http://localhost:4173
npm run build      # production build into dist/
npm run preview    # serve dist/ locally
```

Needs Node 18 or newer.

## Make it yours (everything lives in `src/data/profile.js`)

| What | Where |
| --- | --- |
| Name, motto, About text | `person`, `about` |
| **Email and social links** | `contact`: fill in `email` and each `url`. Empty ones are hidden, and with no email the form says it is switched off |
| Skill levels (1 to 5) | `skillGroups`. These start low and honest; **set them to what is true**. Visitors can also play with the sliders, and their changes stay in their own browser only |
| Projects | `projects`: description, tech, open questions, `status`, `screenshots`, `links` |
| Timeline | `timeline`: add entries when things actually happen |

Screenshots: put images in `public/shots/` and add `{ src: '/shots/name.png', alt: 'what it shows' }` to a project.
Links: `{ label: 'Repository', href: 'https://…' }`.

You can also set contact details at build time instead of editing the file:

```bash
PORTFOLIO_EMAIL=you@example.com PORTFOLIO_GITHUB=https://github.com/you npm run build
```

Also supported: `PORTFOLIO_LINKEDIN`, `PORTFOLIO_INSTAGRAM`, `PORTFOLIO_X`, and `PORTFOLIO_FORM_ENDPOINT`
(a Formspree-style URL that accepts a JSON POST, so visitors can send without an email app).

**The honesty rule:** nothing on the site claims more than is true. Concepts stay "Concept", beginner skills stay
beginner levels, and the Milestones chapter says "Nothing claimed yet" until there is something real to add.

## Deploy

`dist/` is plain static files. Any static host works.

- **Vercel / Netlify:** import the repo. `vercel.json` and `netlify.toml` are already set (`npm run build`, publish `dist`).
- **GitHub Pages / anywhere else:** upload the contents of `dist/`.
- **One file:** `node scripts/build.mjs --single` also writes `dist-single/index.html` with everything inlined.

## How it is built

React 19 + Tailwind CSS v4 (its own compiler, run from `scripts/`) + esbuild. No animation or 3D library: the motion
is CSS plus small `requestAnimationFrame` loops, and the 3D (hero sphere, Thinking Engine, project models, contact
beacon) is a small projection engine in `src/lib/engine3d.js` drawn on 2D canvases.

- **Fast:** about 88 KB JS and 10 KB CSS gzipped. Every canvas stops drawing when off screen.
- **Accessible:** skip link, real landmarks and heading order, keyboard operation everywhere (arrow keys on the
  engine tabs and skill meters), focus trap and focus return in dialogs, visible focus, `prefers-reduced-motion`
  honoured (no loader, no warp, instant transitions), 16px inputs, 24px+ tap targets.
- **Responsive:** checked at 320, 390, 820, 1024, 1440 and 1920 px.
- **Desktop only extras:** the custom cursor appears only with a fine pointer.

```
src/data/profile.js   all content
src/components/       one file per section or control
src/lib/              3D engine, scenes, motion helpers
src/hooks/            canvas loop, drag-rotate, focus trap, in-view
src/styles/           tokens, base, shell, one file per section
scripts/              build, Tailwind runner, static server
tests/                browser tests (see tests/README.md)
```

## Stack note

The original brief named Next.js, Framer Motion, GSAP and Three.js. This version was built where those packages
could not be installed, so it uses the lighter stack above. The components are plain React and the content is
isolated in one data file, so moving to Next.js (`app/page.jsx` with `'use client'`) and swapping the canvas scenes
for Three.js is a contained port.
