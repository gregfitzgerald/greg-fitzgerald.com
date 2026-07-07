# Website TODOs - greg-fitzgerald.com

## Settled Facts (do not re-litigate; confirmed by Greg 2026-07-24)

- **Enrichment effect-size gap is `2.5x`, site-wide.** It appears on `about.html`, `cv.html`, and
  `research.html` and must stay identical in all three. The old `g = 0.44` / `g = 0.16` values on
  `research.html` were removed: they divide to 2.75 and could not be traced to the dissertation
  (the working draft still has `[[TK]]` placeholders for the pooled effects). Do not reintroduce
  specific g-values until the final numbers are locked.
- **Teaching is "six years," date range `2020 -- 2026`.** Never "seven semesters" or another count.
- **No "fifteen years" / "15 years" aggregate claim anywhere.** The dated CV entries carry the span.
- **Dash style on non-blog pages is ` -- ` (space, two hyphens, space).** No em dashes anywhere on
  the site. Note the blog guide (`posts/research-literacy-guide.html`) deliberately uses the tighter
  ` --word` form at Greg's request; leave it as-is.
- **Project display names:** Figure Extractor, Meta-Reference Toolkit, Nimble (not "Nimble Lab
  Manager"), Active Reader.
- **The CNS Neuroscience & Therapeutics paper is a "first-author publication,"** never "a review."
- **CSHL wording is "managed operations" / "coordinated,"** never "led" or "directed".

## Git History Policy (added 2026-07-06)

**This is a PUBLIC repo. Keep it at a single commit so browsable history never exposes deleted/sensitive files** (e.g., an old resume PDF once leaked a phone number via history). History was squashed to one commit on 2026-07-06.

Going forward, do NOT add stacked commits. To deploy changes:

```bash
git add -A
git commit --amend --no-edit          # fold changes into the single commit
git push --force origin main
```

Keep the commit message generic ("Personal website -- greg-fitzgerald.com"). If a message change is ever needed, use `--amend` (without `--no-edit`). Never `git push` without `--force` after amending. A local-only `pre-squash-backup` branch holds the old 147-commit history if ever needed; it is never pushed.

Note: a force-push does not instantly purge old commit SHAs from GitHub's cache -- they can remain reachable by direct SHA until GitHub garbage-collects. For guaranteed removal of anything truly sensitive, open a GitHub Support request to purge dangling commits.

## Blog Content TODO (added 2026-07-04)

- [ ] **Update the Zotero portions of the Research Literacy guide blog post.** Zotero shipped a major overhaul in the past few months, so several screenshots and step-by-step instructions in the guide (annotation toolbar, PDF reader, metadata editing, Word plugin, library organization) are now outdated. Recapture the affected screenshots and revise those steps.
  - Published post: `posts/research-literacy-guide*.html` (rendered from the source doc).
  - Source doc: `My Drive/academic/FALL 2025/APSY_381_CONSOLIDATED/Assignments/Research Literacy/RESEARCH LITERACY COMPREHENSIVE GUIDE.docx` -- update it first, then re-run the docx->HTML converter in the session scratchpad (or re-convert).
  - Note: the box/arrow callouts on the screenshots are baked into the source images (not added during conversion), so replacing them means recapturing + re-annotating.


## Tutoring/Consulting Page TODO (added 2026-07-06)

- [ ] **Build a dedicated `tutoring.html` page.** Deferred for now (currently a redirect stub -> contact.html; NOT in nav). When built: consulting pitch (repurpose the About "Teaching, tutoring & consulting" copy), plain text links to Calendly (`calendly.com/gregfitzgerald`) and Clarity.fm (`clarity.fm/gregfitzgerald`), plus conventional-tutoring blurb. Then add "Tutoring" to the site nav across all 7 pages (between Blog and Contact) and add the page to `sitemap.xml`. A drafted version was built and reverted on 2026-07-06 -- see git history if useful. Widgets (Calendly inline embed / Clarity button) were considered and declined; keep to plain links unless Greg asks otherwise.

## QA: Dogfood After Substantial Changes

After any substantial website change, run a visual QA pass ("dogfood"):

1. Open each page in headless browser (profile: openclaw)
2. Take full-page screenshots
3. Check browser console for errors
4. Look for: broken links, placeholder text, layout issues, CSS leaks
5. Save screenshots to `dogfood-output/screenshots/`
6. Update `dogfood-output/report.md` with findings

**dogfood-output/ is gitignored** -- QA artifacts stay local.

Last dogfood: 2026-08-08 (22 widths from 320 to 1600 x 8 pages = 176 combos: 0 horizontal
overflow, 0 console errors). **Sweep a width range, not two fixed viewports.** The
2026-07-24 pass tested only 1280 and 390 and therefore missed a live overflow bug that
affected every width from 821 to 953px.

## Layout Invariants (added 2026-08-08 -- do not break these)

- **Gutters are `32px`, not `2rem`.** The root is 19px, so `2rem` = 38px, which makes the
  content area 904px and breaks the rail arithmetic. `.site-header`, `.site-footer`,
  `.page`, and `body > article` must all use 32px so `568 + 348 = 916` holds and the text
  column, the rail, and the nav share one right edge (x=1098 at 1280px).
- **The rail breakpoint is `959px`, not 820px.** Below 980px the text column's right edge
  is pinned at `32 + 568 = 600`, so a rail float lands at `600 + 348 = 948` regardless of
  viewport. Anything below 959 must use the stacked treatment.
- **Margin elements must live inside a 568px text block** (a `<p>`, or a `<summary>` inside
  a `.research-item`), never as a direct child of `.page` -- a float's negative pull is
  measured from its containing block.
- **The blog-post layout rule is scoped to `body > article`.** Unscoped, it also caught
  `<article class="post-preview">` on writing.html and indented it 206px.
- **`.ri-status`'s responsive reset must come after the RESEARCH ITEMS block** in the file.
  Same specificity, so source order decides it.
- **One link colour** (`#00008b`). The old green for external links was 6.6:1 against the
  navy's 15.3:1 and read as a disabled link.
- **Nav is Research / CV / Writing / About / Contact**, plain text (no boxed pills), with
  `aria-current="page"` on the active item. The wordmark carries Home.
- **The nav is always ONE row** (Greg's requirement, 2026-09-19). Name and nav side by side
  need 713px, so below 768px the header stacks (name on top, nav beneath) and the nav uses
  `space-between` with `nowrap`, capped at 22rem. Below 360px the nav drops to 0.9rem. Verify
  with a width sweep: `rows == 1` for `.site-nav a` at every width from 320 up.
- **` -- ` in visible text carries a word joiner** (`-⁠-`) so it can't break across
  lines. Attributes are deliberately left alone.

---

## Recent Updates (2026-02-21)

**Beats System Implemented:**
- ✅ Created `js/beats.js` for rendering activity badges
- ✅ Added beats CSS to `style.css`
- ✅ Created import scripts in `scripts/`:
  - `import-zotero.mjs` - Papers from Zotero
  - `import-github.mjs` - GitHub activity
  - `import-twitter.mjs` - Tweets (requires SocialData API)
  - `import-til.mjs` - TILs from `data/tils.md`
  - `import-all.mjs` - Master script
- ✅ Added "Recent Activity" section to homepage
- ✅ Created `activity.html` for full activity timeline
- ✅ Added beats widget to right sidebar
- ✅ Updated sitemap

**To keep beats fresh:**
```bash
cd /mnt/c/Users/gregs/gregfitzgerald.github.io
node scripts/import-all.mjs --quick
git add . && git commit -m "Update beats" && git push
```

---

## Recent Updates (2026-02-04)

**Completed by Sophie (Subagent - Ralph Wiggum Cycles):**
- ✅ Created `jimmy.html` stub (was broken link from about.html)
- ✅ Fixed GitHub placeholder URLs in `projects.html` (now clearly marked)
- ✅ Fixed academic profile placeholder URLs in `contact.html` (now clearly marked)
- ✅ Fixed navigation consistency in `cv.html` and `gallery.html` (now match site-wide nav)
- ✅ Fixed RSS feed URLs (changed from gregfitzgerald.github.io to greg-fitzgerald.com)
- ✅ Created 4 blog post stubs in `posts/`:
  - `claude-code-researchers.html`
  - `research-literacy-overview.html`
  - `meta-analysis-methodology.html`
  - `ai-resistant-learning.html`
- ✅ Created `CONTENT-NEEDED.md` documenting all content Greg needs to supply
- ✅ Updated `sitemap.xml` with new pages

**See `CONTENT-NEEDED.md` for comprehensive list of content Greg needs to provide.**

---

## Critical (Blocks Site Launch)

- [ ] **Configure DNS on Cloudflare** - Add CNAME records pointing to gregfitzgerald.github.io
  - Record 1: @ → gregfitzgerald.github.io (DNS only, gray cloud)
  - Record 2: www → gregfitzgerald.github.io (DNS only, gray cloud)
  - Reference: GITHUB-PAGES-DEPLOYMENT-GUIDE.md

- [ ] **Enable HTTPS** - After DNS propagates, enable "Enforce HTTPS" in GitHub Pages settings

## High Priority (Enhance Functionality)

- [ ] **Add Calendly URL** - Replace placeholder in tutoring.html (line 108)
  - Current: `https://calendly.com/YOURLINK/tutoring`
  - Action: Get Calendly account, replace YOURLINK with actual username

- [ ] **Add GitHub Username** - Replace placeholders in projects.html
  - Line 99: `https://github.com/username/meta-analysis-enrichment`
  - Line 164: `https://github.com/username/research-literacy-guide`
  - Action: Determine GitHub username, replace "username" with actual username

- [ ] **Replace Placeholder Testimonials** - Update data/testimonials.json with real student feedback
  - Current: 3 placeholder testimonials
  - Action: Collect real testimonials and update JSON file

## Content Creation (Medium Priority)

These blog posts would strengthen the portfolio:

- [ ] **Claude Code Guide for Researchers** - Write comprehensive guide
  - Demonstrates AI tool proficiency (key differentiator)
  - Target audience: Researchers, data scientists, biotech companies
  - Estimated time: 2-3 hours
  - Use posts/_template.html as starting point

- [ ] **Scientific Research Literacy Guide (Blog Version)** - Adapt existing 50+ page guide
  - Shows curriculum development and teaching ability
  - High-level overview with link to full guide
  - Estimated time: 3-4 hours

- [ ] **Meta-Analysis Methodology Post** - Write about thesis work
  - Explain 2.5x larger effect sizes in rodents vs humans finding
  - Demonstrates quantitative skills and systems thinking
  - Target audience: Translational researchers, CROs, pharma
  - Estimated time: 4-5 hours

- [ ] **Educational Software Project Post** - Document AI-resistant learning platform
  - Shows software development skills
  - Target audience: EdTech companies, data science roles
  - Estimated time: 2-3 hours

## GitHub Repositories (Medium Priority)

Create these repos and link from website:

- [ ] **meta-analysis-enrichment** - Thesis work (R code, data, documentation)
- [ ] **research-literacy-guide** - 50+ page curriculum as open-source resource
- [ ] **learning-platform** - AI-resistant educational software code
- [ ] **lab-inventory-manager** - If project exists, create repo

Each repo should include:
- Comprehensive README.md
- LICENSE file (choose appropriate open-source license)
- Link back to blog post on website
- Clear documentation

## Optional Enhancements (Low Priority)

- [ ] **Interactive Blog Post: Effect Size Calculator** - JavaScript-based meta-analysis tool
- [ ] **Interactive Blog Post: Power Analysis Tool** - For experimental design
- [ ] **Interactive Blog Post: Statistical Test Selector** - Decision tree interface
- [ ] **Dark Mode Toggle** - Mentioned in requirements but not critical
- [ ] **Blog Search Functionality** - Filter/search posts
- [ ] **RSS Feed** - For blog subscribers
- [ ] **Google Analytics** - Track visitor stats (if desired)
- [ ] **Professional Headshot** - Add to About page

## Content Enhancement Ideas

Based on Master Career Chronology, consider enriching:

### About Page
- Emphasize 15 years research experience
- Highlight Cell publication (2017, Impact Factor 64.5)
- Detail CSHL lab manager experience (500+ mouse colony, budgets)
- Mention industry exposure through Certerra
- Emphasize 7 semesters teaching statistics

### Projects Page
- Add more detail about Exerkine Performance startup (2021)
- Expand meta-analysis finding explanation
- Discuss translational validity implications for drug development

### Homepage
- Add quick stats: "15 years research | Published in Cell | 7 semesters teaching"
- Brief mention of AI/Claude Code proficiency
- Consider adding professional headshot

## Quick Reference

### Files You Can Edit Directly
- `data/testimonials.json` - Add/modify testimonials
- `posts/_template.html` - Copy to create new blog posts
- `writing.html` - Add new post previews
- `sitemap.xml` - Update when adding new pages

### Helper Scripts
- `preview-website.bat` - Double-click to preview locally
- `deploy-website.bat` - Double-click to commit and deploy

### Reference Docs
- `LOCAL-DEVELOPMENT-WORKFLOW.md` - Complete local editing guide
- `GITHUB-PAGES-DEPLOYMENT-GUIDE.md` - DNS and deployment instructions
- `HOSTING-ALTERNATIVES-FOR-DYNAMIC-FEATURES.md` - Future migration options

## Workflow for Adding New Blog Post

1. Copy `posts/_template.html` to `posts/new-post-slug.html`
2. Edit new file: update title, meta tags, datetime, content
3. Add post preview to `writing.html` (copy existing post-preview block)
4. If one of 3 most recent, update `index.html` homepage
5. Update `sitemap.xml` with new post URL
6. Commit and push using `deploy-website.bat`

## Notes

- Site is ready for launch - only DNS configuration blocks going live
- All core functionality implemented and tested locally
- Content creation can happen at your own pace while site is live
- Interactive features (Phase 3) are nice-to-have enhancements

---

Generated: January 21, 2026
