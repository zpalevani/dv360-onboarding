# DV360 Team Onboarding

An onboarding reference and hands-on workbook for teams learning Display & Video 360. It is a single static page that reads as a scrolling guide or runs as a slide deck (press **P**, then use the arrow keys).

Live at https://dv360-onboarding.zarapalevani.com

## What's inside

- **Part I: the platform.** What DV360 buys and how (open auction, deals, programmatic guaranteed, YouTube), the partner → advertiser → campaign → insertion order → line item structure, the tools around DV360, how bidding, Google AI, audiences, Floodlight, brand suitability and frequency caps work, and the rules for each channel (CTV, digital out-of-home, audio, YouTube, Demand Gen).
- **Part II: the workbook.** A glossary with a quiz mode, five common problems and their fixes, a 30-day checklist, and a five-task scenario lab for a fictional client (Northwind Outdoor).
- **Part III: supporting DV360 clients.** When a client needs DV360, recognizing common client issues, enterprise architecture, and fees, revenue models and support.

Checklist ticks and lab answers are saved in the visitor's browser (`localStorage`). The site also loads Google Tag Manager (`GTM-TTJX54`), which sends usage data to Google Analytics, and Cloudflare Web Analytics.

## Security headers and crawling

`_headers` sets the security headers Cloudflare Pages sends with every page, including a Content-Security-Policy. It is the same policy as the GA4 and SA360 onboarding sites.

The page loads Google Tag Manager (`GTM-TTJX54`, the same container as the other onboarding sites), whose Custom HTML tags are added to the page inline. So `script-src` allows inline scripts (`'unsafe-inline'`) instead of listing them by hash. Don't add `sha256-` hashes back: when a hash is present, browsers ignore `'unsafe-inline'` and GTM's tags stop running.

The policy allows Google Tag Manager and Google Analytics (`*.googletagmanager.com`, `*.google-analytics.com`, `*.analytics.google.com`, `*.g.doubleclick.net`, `*.google.com`), and any `https:` image, because Google Signals uses country domains such as `google.ca`. **If a GTM tag starts loading a script or sending data to a new domain, add that domain to `script-src` or `connect-src` in `_headers` (on all the onboarding sites), or the browser will block it silently.** After a tag or header change, open the site and check the browser console for Content Security Policy errors.

The policy also allows Cloudflare Web Analytics, which Cloudflare adds to the page automatically: its script from `static.cloudflareinsights.com`, and its reports to this site's own `/cdn-cgi/rum` endpoint (`connect-src 'self'`) or to `cloudflareinsights.com`.

`robots.txt`, the robots `<meta>` tag and the `X-Robots-Tag` header ask search engines and AI crawlers not to index or use the site. They don't make it private: anyone with the link can still read it. `_redirects` sends `/README.md` to the home page.

## Sources and accuracy

- Every technical claim was checked against Google's official Display & Video 360 Help Center pages, the DV360 announcements page and Google's own product pages and blog on 23 November 2026. Each important claim links its source on the page, and the "Official documentation" slide lists them by topic.
- Google updates these pages regularly, and DV360 changes often (see the announcements page). Check the linked page before quoting a limit, and update the "checked on" dates when you re-verify.
- Where Google's pages disagree, the site follows the newer announcement and says so. Example: the insertion order article still lists targeting and inventory sources as insertion order settings, but since February 2026 they live only at partner and line item level.
- The lab is scenario-based: learners plan a campaign for a fictional client and compare with a model answer. Nothing is built or spent. The model answers follow Google's documentation; budget splits and frequency cap values are judgment calls and are labeled as such.
- Not yet verified:
  - whether Google offers a public DV360 demo or sandbox account (none was found in the documentation);
  - the "When DV360 may not be the right fit" list in Part III, which is judgment, not Google's wording, and wasn't checked against the Google Ads Help Center;
  - DV360 pricing and fee rates, which Google doesn't publish.
- The checklist refers to internal arrangements (an onboarding buddy, read-only access to the team's DV360 partner or advertisers). Confirm these with the team before relying on them.
- Wording: the site says "client" for the companies the team supports. "Customer" appears only in the team name Customer Success and in the Google product name Customer Match.

## Editing

Edit `index.html` directly. All content, styles and scripts are in that one file.

The page was first generated from the SA360 site by `dv360-build/build.py` (in the parent folder, not in this repo), which swaps in `dv360-build/main.html` and DV360-specific names, storage keys and interactive data. Don't re-run it after editing `index.html`: it would overwrite those edits.

After a change, check the JavaScript syntax, HTML nesting and duplicate IDs, then test navigation, present mode (P, G, T, arrow keys, Esc), the interactive parts, saved progress after a reload, and the layout at 375 px wide.

## Deploying

There is no build step. Everything is in `index.html`, which is served as-is by Cloudflare Pages:

- Framework preset: None
- Build command: *(empty)*
- Build output directory: `/`
- Custom domain: `dv360-onboarding.zarapalevani.com`
