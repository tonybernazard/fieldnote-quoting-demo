# Fieldnote

[Open the live demo](https://tonybernazard.github.io/fieldnote-quoting-demo/)

A thoughtfully designed quoting and job-tracking workspace for an independent creative studio. Built as an original, interactive portfolio demonstration for Tony Bernazard.

## What you can explore

- A live overview with metrics and a project-value chart calculated from workspace data
- Searchable quotes and a Draft / Sent / Accepted / Declined pipeline
- A quote builder with editable descriptions, fractional quantities, USD rates, discounts, and tax
- Client creation and editing
- Automatic job creation when a quote is accepted; Planning, In progress, and Complete stages
- Client-ready proposal preview, browser print / Save as PDF, and structured JSON export
- Browser-local persistence and a confirmed reset to the fictional sample workspace
- Responsive desktop, tablet, and phone layouts, semantic forms, keyboard navigation, focus indicators, reduced-motion support, and focus-trapped dialogs

## Run locally

Node.js 20 or newer is recommended. The application has no runtime dependencies and needs no build step.

```
npm start
```

Open http://localhost:4173. Any static web server can host the same files. Asset URLs are relative, so deployment under a GitHub Pages project path works without configuration. `.nojekyll` disables Jekyll processing.

## Tests

```
npm test
```

The Node test suite covers cents-based totals, fractional quantity rounding, discount/tax boundaries, quote creation and updates, idempotent accepted-quote job creation, persistence round-trips, malformed-storage fallback, and independent seed data.

An optional browser flow suite is provided. Install Playwright for testing only, start the server, and run:

```
npm install --no-save playwright
npx playwright install chromium
CHROMIUM_PATH=/path/to/chromium npm run test:e2e
```

`CHROMIUM_PATH` defaults to `/usr/bin/chromium`. `BASE_URL` defaults to http://127.0.0.1:4173. The suite exercises quote create/update/delete, live totals, jobs, client editing, search, JSON download, print PDF, reset, and mobile overflow. Screenshots and a sample PDF are written to `screenshots/`.

Validation: all seven Node tests passed. Independent live desktop browser QA verified quote creation, editing and deletion; the $97.40 calculation example; accepted-quote job creation; job-stage persistence after reload; client creation and editing; search; actual JSON download; dialog forward/reverse keyboard focus trapping and Escape focus return; linked-job deletion; and reset to four original quotes with the test client removed. No site-origin browser errors were observed. Desktop overview and proposal screenshots were visually reviewed.

The automated Chromium flow suite could not run in the build sandbox because native socket creation was blocked; it is provided for reproducible testing, not represented as passed. Responsive layouts were source-reviewed and hardened for phone/tablet breakpoints, but mobile runtime behavior has not been independently browser-tested. Print / PDF is implemented with native browser printing and print CSS; actual printed output was not verified because the cloud browser did not expose a print dialog.

## Calculation rules

USD rates are rounded to cents. Each quantity × rate line is rounded to the nearest cent; rounded lines are summed. The percentage discount is rounded to cents and subtracted, then percentage tax is calculated and rounded on the discounted subtotal. Values shown in the interface are derived from this same calculation function. Tax and discount percentages are constrained to 0–100. This is demonstration arithmetic, not tax or accounting advice.

## Demo boundaries

All clients, people, contact addresses, projects, and amounts are synthetic. The `.example` email addresses are intentionally non-deliverable. No employer code, private products, real client data, or personal financial information is included.

There is no backend, account system, authentication, payment collection, email sending, signature service, or multi-device synchronization. “Sent” is a tracking label only. Quotes and jobs are stored in your browser’s localStorage. Clearing site data removes changes. Reset replaces all changes with the original samples. Avoid entering real or sensitive information into this public demo. Data is not intended to be a durable business record.

Changing an accepted quote to another status does not remove its existing job, to preserve job progress. Deleting a quote explicitly removes its linked job after confirmation. Proposal JSON exports are portable records, not importable backups. Print / PDF uses your browser’s native print dialog.

Typography uses Google Fonts (DM Sans and Manrope) with system fallbacks when offline; this makes a standard font request to Google. The demo has no analytics or other application network calls.

## Structure

- `index.html`: static application entry
- `src/app.js`: accessible view rendering, user flows, and browser interactions
- `src/model.js`: synthetic seed data, calculations, and state operations
- `src/style.css`: original responsive visual system and print styles
- `tests/model.test.mjs`: dependency-free unit tests
- `tests/browser.mjs`: optional Playwright flow tests and screenshot capture
- `server.mjs`: minimal development-only static server

This is a portfolio demonstration, not a production quoting platform or a binding offer.
