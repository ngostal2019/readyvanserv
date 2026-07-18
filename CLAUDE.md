# Ready Van Services — project context (read me first)

This file exists so any AI assistant (e.g. Claude Code in VS Code) or person can pick up
exactly where we left off. Read it fully before making changes.

## What this project is
Rebuilding the client's website from **Google Sites** into a **static coded site**
(plain HTML + CSS) to be hosted on **GitHub Pages** at the existing custom domain
`www.readyvanservices.com`. No framework, no build step. Deploys automatically via
the GitHub Action at `.github/workflows/deploy.yml` on every push to `main`. The
`CNAME` file in the repo root holds the custom domain GitHub Pages needs.

(This project originally targeted Render.com — switched to GitHub Pages on 2026-07-17
since the site has no server-side needs and GitHub Pages is free and simpler for a
pure static site. If server-side logic is ever needed, Render remains a fine option.)

**⚠️ Hosting/repo note (2026-07-18):** this local working copy's git remote is
`git@github.com:ngostal2019/readyvanserv.git`, but the site actually LIVE at
`www.readyvanservices.com` deploys from a **different** repo:
`readyvanservices/readyvanservices.github.io` (a GitHub Pages user/org site under its
own separate `readyvanservices` GitHub account, not `ngostal2019`). That repo is where
custom-domain + HTTPS enforcement were configured and verified working. Pushes to
`ngostal2019/readyvanserv` do **not** automatically reach the live site — any change
made here needs to also be copied into the `readyvanservices.github.io` repo (which
Claude does not have push/API access to from this machine's `gh` session). Before
doing further work, confirm with the client which repo is the actual source of truth
going forward and consider consolidating to one.

## The business
- Name: **Ready Van Services**
- Services: junk removal, moving / small moves, furniture delivery, cleanouts,
  appliance removal, yard & construction debris.
- Phone (text only — **no calls**): **737-333-1410** — per client request, this number is never
  printed as visible text on the page. It only exists inside `sms:` link hrefs (and in the
  JSON-LD schema + meta description, which aren't visually rendered). If you add a new phone
  mention, follow this pattern — don't print the digits.
  - As of 2026-07-18, the client only wants to be reachable by **text or email**, not phone
    calls. The `#contact` section's phone tab is "📱 Text Us" (an `sms:` link only) — there is
    no `tel:` link anywhere on the site anymore. Don't reintroduce a "Call" button/link.
  - All `sms:` links are consolidated into the "Text Us" tab of the `#contact` section (see
    below) — every other CTA site-wide (header, hero, footer, floating button, CTA band, FAQ,
    how-it-works) links to `#contact` instead of directly to a `sms:` href. Keep new CTAs
    pointing at `#contact` rather than re-adding direct phone links elsewhere.
- Email: **readyvanservicesquotes@yahoo.com** (changed from the original readyvanservices@gmail.com)
- Service areas: Austin, Kyle, Buda, San Marcos, Lockhart, New Braunfels, Bee Cave, Lakeway (greater Austin, TX)
- Brand colors: navy `#14284a`, red `#c8102e`, white. American/patriotic style.
- Primary call to action: "Text a photo to 737-333-1410 for a free quote."

## Files
- `index.html` — the entire one-page site
- `styles.css` — all styling (navy/red theme)
- `images/` — needs the real image files (see below)
- `sitemap.xml`, `robots.txt` — SEO/indexing
- `README.md` — deploy instructions
- `preview.html` — a throwaway preview with placeholder boxes (NOT for deploy; can delete)

## Images — DONE
All real images are in `images/` under the exact filenames `index.html` expects,
verified against `images/DOWNLOAD-THESE.md`:
- `logo.jpg` — round Ready Van Services emblem
- `hero.jpg` — white van in driveway (side view)
- `tools.jpg` — "Equipped. Prepared. Ready to work." gear graphic
- `g1.jpg` … `g10.jpg` — 9 branded before/after graphics + 1 "full load" packed-van shot, in the gallery grid
- `contact-card-1.jpg`, `contact-card-2.jpg` — the two business-card graphics, shown in a
  "Save our info" section (`#contact-card`) added between FAQ and the CTA band

CONFIRMED: all image files are JPEG, so all `src` references use `.jpg` (including `logo.jpg`).

## Status / done so far
- Captured all content from the old Google Sites site.
- Built the coded site, upgraded design + added sections (services, how-it-works,
  equipped/tools, before-after gallery, why-us, areas, FAQ, CTA, footer).
- Rebranded to navy/red with logo, van hero, expanded services, email, SEO structured data.
- Mobile-responsive; sticky header; floating "Free quote" button on mobile.
- Pushed to GitHub and deployed via GitHub Pages (see the hosting/repo note above — live
  deploy is from `readyvanservices/readyvanservices.github.io`, not this repo's remote).
- Custom domain **`www.readyvanservices.com`** is live with HTTPS enforced, verified
  2026-07-18 (DNS on Namecheap: `www` CNAME → `readyvanservices.github.io`).
- Quote form reliably delivers both text fields and photo attachments via FormSubmit.co —
  see the rewritten "Contact section" details below for the (non-obvious) reasons the
  submission code looks the way it does.
- Removed the "Call Us" button/`tel:` link site-wide per client request (2026-07-18) —
  text and email/form are now the only contact methods.
- FormSubmit email confirmed/activated and swapped to its random hex token in the form
  `action` (2026-07-18) — the raw `readyvanservicesquotes@yahoo.com` address no longer
  appears in `index.html`'s form markup (still used in the JSON-LD schema/meta description,
  which is fine/expected).
- Fixed quote form submission on iOS Safari (2026-07-18) — see "Contact section" below.

## Next steps (in order)
1. ~~Add the real image files to `images/`~~ Done.
2. ~~`git init` / push to GitHub / enable Pages~~ Done.
3. ~~Add custom domain + DNS~~ Done — `www.readyvanservices.com` is live with HTTPS.
4. ~~Swap the FormSubmit email for its random hex token~~ Done 2026-07-18 — the form's
   `action` in `index.html` now points at `https://formsubmit.co/<hex-token>` instead of
   the raw email (email confirmed/activated with FormSubmit in the process).
5. ~~iOS Safari submission fix~~ Done 2026-07-18 — photos now compress at file-select time
   instead of at submit time so the popup-window submit stays synchronous with the tap (see
   "Contact section" below). **Still needs a real end-to-end retest on an iPhone once this
   fix reaches the live site** (see hosting/repo note — it was fixed in `ngostal2019/readyvanserv`
   but not yet copied to `readyvanservices/readyvanservices.github.io`).
6. Optional: apex domain (`readyvanservices.com`, no `www`) currently doesn't resolve —
   add 4 `A` records at Namecheap (`@` → `185.199.108.153/.109.153/.110.153/.111.153`) if
   the client wants the bare domain to work too (GitHub auto-redirects it to `www`).
7. SEO: keep the homepage at `/`, submit `sitemap.xml` in Google Search Console,
   confirm Google Business Profile points to the site.
8. Resolve the two-repo hosting situation (see hosting/repo note above) so future edits
   only need to be made in one place.

## Contact section (`#contact`)
One consolidated section (formerly `#quote-form`) offers a choice between two tabs, toggled by
plain JS (no framework): "📱 Text Us" (an `sms:` link only — see the phone bullet above, no
`tel:`/call option exists) and "📝 Fill Out the Form" (the quote-request form). The nav's last
link ("Contact") and every other page CTA point at `#contact`.

### Quote form submission — why it's built the way it is
The quote-request form (name, phone, email, pickup address, pickup date, photo upload) submits
to **FormSubmit.co** (form `action` attribute on `<form id="quoteForm">` — as of 2026-07-18
this is FormSubmit's random hex token, not the raw email, per their anti-scraping
recommendation; it still delivers to `readyvanservicesquotes@yahoo.com`), forwarded as an
email — no backend needed since this is a static site.
Photos are resized/compressed in-browser via `<canvas>` before upload to stay under FormSubmit's
10MB combined attachment cap (silent — no user-facing "photos are compressed" text, per client
request).

**This looks more complicated than a plain form POST because of real, tested FormSubmit
limitations discovered 2026-07-17/18 — don't "simplify" it back without re-testing:**
- FormSubmit's `/ajax/<email>` endpoint (fetch + `FormData`) delivers text fields fine but
  **silently drops file attachments** — confirmed by direct testing, not just docs.
- Submitting into a **hidden iframe** (to avoid a page redirect while still doing a real
  multipart POST) gets accepted with an HTTP 200 "Thanks!" response but **the email is never
  actually sent** — FormSubmit appears to silently discard submissions that look bot-like
  (iframe-embedded, no visible top-level navigation). Confirmed by testing to two different,
  previously-unused email addresses (one Yahoo, one Gmail) — neither ever received anything
  via the iframe route, even after checking spam and waiting 40+ minutes.
- A **real, visible top-level navigation** is what actually works reliably. Since photo
  compression is async (canvas `toBlob`), the code can't just set `target="_blank"` and call
  `.submit()` inside the async callback — popup blockers kill that. The fix: open the target
  window **synchronously**, inside the click handler, via
  `window.open('about:blank', 'qfSubmitWindow')`, then navigate that *same named window* later
  once compression finishes (`form.target = 'qfSubmitWindow'` + `HTMLFormElement.prototype
  .submit.call(form)`) — reusing an already-open window bypasses the popup blocker even
  though the navigation itself happens asynchronously. That window auto-closes itself ~4s
  after submit (client didn't want it left open) rather than being fully hidden, since fully
  hidden (iframe) is exactly the thing that doesn't deliver.
- File field naming matters: FormSubmit's own docs demonstrate attachments via a field named
  `attachment` (not our UI's `photos`), and describe multiple files as multiple separate file
  input fields rather than one `multiple` input. The code builds fresh hidden
  `attachment`/`attachment1`/`attachment2`… inputs right before submitting (via `DataTransfer`)
  instead of reusing the picker input's `photos` field — this is what fixed attachments not
  showing up in the delivered email.
- The success message is an **auto-dismissing flash/toast** (`.form-flash`, fixed-position,
  fades in/out, ~6s), not a permanent block replacing the form — the form resets itself
  (`resetQuoteForm()`) afterward so it's ready for another submission.
- **FormSubmit rate-limits/blocks by origin during heavy testing**: during development, rapid
  repeated submissions from `localhost` got silently blackholed (200 "success" response, zero
  delivery, to *any* recipient) — this was NOT a code bug, confirmed by testing from the real
  deployed domain instead, which delivered immediately. If future debugging sees "success" but
  no email again, suspect this before touching the code, and try a different network/domain.
- ~~FormSubmit recommends replacing the plain email in `action=` with a random hex token~~
  Done 2026-07-18.
- **iOS Safari left the popup window permanently blank** (fixed 2026-07-18): the desktop-tested
  version above compressed photos inside the submit handler (async, via canvas `toBlob`), then
  navigated the popup afterward. Desktop browsers tolerate that gap; iOS Safari does not — it
  only honors the delayed `form.target`-based navigation into the pre-opened popup if it happens
  (near-)synchronously with the original tap. The fix: `startCompressingSelectedPhotos()` kicks
  off compression in the background as soon as photos are picked (on the file input's `change`
  event), caching results in `compressedPhotoCache` (a `Map` keyed by the original `File`
  object). The submit handler is now fully synchronous — it just reads whatever's already in
  the cache (falling back to the uncompressed original file if someone submits within a second
  of picking photos, before background compression finishes), so `window.open()` and the actual
  `HTMLFormElement.prototype.submit.call(form)` happen in the same tick as the tap, no `Promise`
  chain in between. Don't reintroduce async work inside the submit handler itself.

Form UX notes:
- The native `<input type="file" id="qf-photos" name="photos">` is visually hidden; a styled
  "📷 Choose Photos" button triggers it via `.click()`. Selecting files renders thumbnail +
  filename previews below the button (`#qfPhotoPreviews`) — this `photos` field only drives the
  picker UI/previews now; the actual submitted attachment fields are generated separately (see
  above).
- Required fields (name, phone, email, pickup date, address) show a red `*` next to the label,
  with a "* Required field" legend above the form. Validation is custom JS (not the browser's
  native `reportValidity()` bubble) — each field gets an inline red error message on blur/submit,
  driven by `requiredFields` in the script at the bottom of `index.html`.
- Nav links get an `active` class on click (plain JS, no scroll-spy).

**One-time setup required:** the first real submission to a given recipient triggers a
confirmation email from FormSubmit — someone must open it and click the activation link, or
submissions will silently fail to deliver (still showing a generic "success" response either
way). Not yet done for `readyvanservicesquotes@yahoo.com` as of 2026-07-18 — see "Next steps".

## Possible enhancements (client may want)
- Google reviews section.
