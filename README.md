# willjordancooley.com

A static rebuild of the Wix site at willjordancooley.com — same content, modern template.
No build step, no framework, no dependencies. Three HTML files, one stylesheet, one script.

## Run locally

```bash
python3 -m http.server 4321
```

Then open http://localhost:4321.

## Structure

```
index.html                         Home — hero, work, writing, experience, about, contact
writing/
  crowd-participation-in-live-improv-event.html
  brainpops-time-zone-x.html
assets/
  css/styles.css                   All styling, design tokens at the top
  js/main.js                       Theme toggle, mobile menu, scroll reveal, form
  img/                             Images pulled from the Wix CDN at original resolution
```

## What changed from the Wix site

Content is carried over verbatim — project blurbs, experience, education, bio, services,
and both blog posts (including the full Time Zone X conference paper). The presentation is
new:

- **Dark/light theme** that follows the OS and can be overridden with the toggle; the choice
  persists in `localStorage`.
- **Responsive** single-column-to-three-column grid; no horizontal scroll at 375px.
- **Blog posts are local pages** rather than Wix-hosted, so nothing breaks when the Wix site
  is retired.
- **Positioning updated** to full-time founder/CEO of Pangea Chat, with the BrainPOP-era
  work framed as earlier work rather than a services portfolio.
- **Images are local** (`assets/img/`), downloaded from the Wix CDN at source resolution.
  Three oversized originals were downscaled: the improv photo (3648px → 1400px), the
  Time Zone X V2 screenshot (2452px → 1600px), the profile photo (960px → 600px).
- Scroll-reveal animation, sticky nav with active-section highlighting, and reduced-motion
  support (`prefers-reduced-motion` disables all of it).

## Contact form

The form posts to a third-party form relay, which forwards submissions to
**will@pangea.chat**. The email address is never in the page source, so it can't be
scraped. A hidden honeypot field (`_gotcha`) catches naive bots.

**It is not wired up yet.** The form's `action` is the placeholder `FORM_ENDPOINT_HERE`,
and `assets/js/main.js` deliberately skips binding while that placeholder is present, so
nothing silently fails. To activate:

1. Create a form on a relay service and point it at will@pangea.chat. Either works:
   - **Formspree** (formspree.io) — 50 submissions/month free. Endpoint looks like
     `https://formspree.io/f/abcdwxyz`.
   - **Web3Forms** (web3forms.com) — no account needed, free, higher limits. Endpoint is
     `https://api.web3forms.com/submit` plus a hidden `access_key` input.
2. Replace `FORM_ENDPOINT_HERE` in `index.html` with the endpoint URL.
3. Send a test message and confirm it arrives.

Both relays are host-agnostic, so this works on GitHub Pages, Cloudflare Pages, Netlify or
S3 without change. If the site ends up on Cloudflare Pages, a Pages Function plus Resend
would remove the third party entirely — more setup, no per-month cap.

Submitting with JavaScript disabled still works: the browser does a normal form POST and
the relay renders its own confirmation page.

## Deploying

The site is plain static files, so anything works — Netlify, Vercel, GitHub Pages, or S3 +
CloudFront. Upload the directory as-is; `index.html` is the entry point.

To point willjordancooley.com at it, update the domain's DNS to the new host and cancel the
Wix plan once the new site is confirmed live.

## Known gaps

- **Older blog posts.** The Wix blog's archive widget lists months from March 2016 through
  July 2021, and its "Recent Posts" sidebar mentions an *EcoChains: Arctic Crisis* post, but
  only two posts are actually reachable on the live site — both are included here. If drafts
  or unpublished posts exist behind the Wix admin, export them before cancelling the plan.
- **Time Zone X V2 video** links out to YouTube rather than embedding, matching the original.
- Image `alt` text was written fresh; the Wix originals were mostly filenames.
