# SEO & Spam-Risk Audit — iiptv.se

**Audited:** 2026-09-17 · **Scope:** all 66 URLs in `sitemap_index.xml` (41 posts, 20 pages, 5 categories), crawled and parsed in full.
**Benchmarks used:** [Google Search spam policies](https://developers.google.com/search/docs/essentials/spam-policies) · [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)

---

## Verdict

Your instinct was right on three of four counts, but the severity is not where you'd expect it.

| Your concern | Finding | Severity |
|---|---|---|
| Keyword stuffing | **Confirmed** — but concentrated in ~8 pages, not site-wide | 🔴 Critical |
| Keyword density bad | **Confirmed** — 5.12% site-wide, 13.2% worst page | 🔴 Critical |
| Content looks AI-generated | **Confirmed** — strong marker density | 🟠 High |
| Weak SEO | **Confirmed** — plus issues you didn't mention that are worse | 🔴 Critical |

The single most dangerous thing on the site is **not** the keyword stuffing. It's the **fabricated review / rating structured data** (§3) and the **paid-guest-post link footprint** (§4). Those two carry manual-action risk; stuffing mostly carries algorithmic-suppression risk.

---

## 1. Keyword density — confirmed, and quantified

Measured on extracted visible text, then re-measured with site boilerplate (nav/footer) removed to prove it isn't just template noise. **Removing boilerplate made density go up, not down** — the repetition is in the body copy.

Healthy density for a primary term is roughly **0.5–2.0%**. Site-wide you are at **5.12%** (8,579 occurrences of "iptv" in 167,669 words).

### Worst offenders (boilerplate removed)

| Page | Body words | "iptv" | Density |
|---|---:|---:|---:|
| `/iptv-kanallista/` | 1,190 | 158 | **13.28%** |
| `/iptv-norway-2/` | 3,491 | 461 | **13.21%** |
| `/category/basta-iptv/` | 520 | 65 | **12.50%** |
| `/iptv-suomi/` | 3,690 | 434 | **11.76%** |
| `/category/nordisk-iptv/` | 293 | 31 | **10.58%** |
| `/category/iptv-nordic/` | 536 | 54 | **10.07%** |
| `/category/iptv-abonnemang/` | 222 | 22 | **9.91%** |
| `/iptv-danmark/` | 2,333 | 220 | **9.43%** |
| `/iptv-norge-pro/` | 3,550 | 329 | **9.27%** |
| `/iptv/` | 2,438 | 205 | **8.41%** |

**Your homepage is fine** (0.84%). The problem is the country landing pages and the category archives.

### The smoking gun

`/iptv-norway-2/` contains the bigram **"iptv norway" 217 times** and **"iptv norge" 132 times** in 3,508 words. The trigram `"norge iptv norway"` appears 21 times — that only happens when keyword variants are chained next to each other with no sentence between them.

Actual rendered text from the page:

> IPTV Norway anmeldelse IPTV Norge erfaringer IPTV Norge omtale IPTV Norway test 2026 beste IPTV Norge 2026 IPTV Norway kunder IPTV anmeldelse Norge IPTV Norway vurdering IPTV Norge Trustpilot…

> IPTV Norge IPTV abonnement Norge IPTV Norway pris norsk IPTV abonnement IPTV Norge HD 4K IPTV Smart TV Norge IPTV Norway sport billig IPTV Norge nordic IPTV abonnement IPTV uten…

This is not prose. It is a keyword list rendered as "Relaterte søk" (related searches) pill tags — 124 such tags on that one page, 42 on `/iptv-suomi/`, 32 on `/iptv/`.

Google's spam policy on **keyword stuffing** defines this exactly:

> "Keyword stuffing refers to the practice of filling a web page with keywords or numbers in an attempt to manipulate rankings in Google Search results… Examples include: blocks of text listing cities and regions a page targets for ranking; repeating words or phrases unnaturally."

Your blocks are the *search-query* version of the "cities and regions" example. Same violation.

**One point in your favour:** the blocks are rendered visibly (`.no-rv-tags` has `display:flex`, normal colours, no `display:none`, no off-screen positioning). So this is *overt* stuffing, **not cloaking or hidden text**. That matters — hidden text is treated far more harshly.

### Important correction to the "5% is all stuffing" reading

On the ~50 ordinary blog posts (density 3.5–5.5%), I checked the densest 40-word windows and the prose is **largely natural**. The elevated number there comes from three benign causes:

1. "IPTV" is the unavoidable topic noun.
2. The nav/footer repeats `IPTV Nordic · IPTV Norge · IPTV Suomi · IPTV Danmark` on every page.
3. Competitor brand names all contain the word (Monster IPTV, Svea IPTV, Viking IPTV, Rapid IPTV…).

**So: do not rewrite all 66 pages.** Fix the 8 pages in the table above. That's where the actual violation is.

---

## 2. AI-content signals — confirmed

Scanned against the markers in *Signs of AI writing*, adapted to Swedish/Norwegian/Danish/Finnish.

| Marker | Hits | Pages (of 66) |
|---|---:|---:|
| Em dash `—` | 763 | 66 / 66 |
| Emoji used as formatting | 2,210 | 66 / 66 |
| Decorative emoji (🚀🔥⭐✨💡🎯📺⚡) | 513 | 66 / 66 |
| "Oavsett om du…" (*whether you're…*) | 54 | 28 |
| Curly quotation marks | 438 | 48 |
| "optimal / optimera" | 125 | 41 |
| Checkmarks ✅✔ | 126 | 36 |
| "avgörande" (*crucial*) | 32 | 21 |
| "Dessutom" sentence-initial (*Additionally*) | 27 | 16 |
| "sömlös/sömlöst" (*seamless*) | 24 | 15 |
| "Kort sagt / Sammanfattningsvis" | 20 | 16 |
| Negative parallelism "inte bara… utan också" | 17 | 12 |

Every one of these is on the Wikipedia list: promotional register ("seamless", "optimal"), `Additionally` at sentence start, negative parallelism ("not just X but Y"), emoji-as-formatting, em-dash overuse, curly quotes. The **100% page coverage** on em dashes and emoji is the tell — human writers are not that consistent across 66 documents.

### Where this actually hurts you

AI-generated content is **not** itself against Google policy. What *is* against policy is **scaled content abuse**:

> "Using generative AI tools or other similar tools to generate many pages without adding value… Pages where content makes little sense but contains search keywords."

So the AI authorship only becomes a violation when combined with §5 below (many near-identical-intent pages). Fix the intent duplication and the AI authorship stops mattering. Polishing the prose alone will not help.

---

## 3. 🔴 Fabricated review & rating structured data — highest-risk finding

This is the item most likely to earn a manual action, and you didn't ask about it.

### Aggregate ratings with no visible source

| Page | Product | Rating | Claimed reviews |
|---|---|---:|---:|
| `/iptv-suomi/` | IPTV Suomi Abonnemang | 4.9 | **1,840** |
| `/iptv-danmark/` | Bedste IPTV Danmark | 4.9 | **1,432** |
| `/iptv/` | IPTV Nordic Sverige | 4.9 | **1,250** |
| `/iptv-norge-pro/` | IPTV Norge Abonnement | 4.9 | **1,240** |
| `/iptv-norge-pro/` | IPTV Norge Abonnement | 4.9 | **2,400** ⚠️ |

`/iptv-norge-pro/` emits **two contradictory `aggregateRating` blocks for the same product on the same page** — 1,240 reviews and 2,400 reviews. That self-contradiction is provable evidence of fabrication, and it is trivially machine-detectable.

### Reviews falsely attributed to Trustpilot

`/iptv-norway-2/` embeds 9 `schema.org/Review` items marked up as if from Trustpilot:

```html
<div class=no-rv-platform>⭐ Trustpilot · <strong>IPTV Norway</strong></div>
<p itemprop=reviewBody>«Support hjalp meg med alt via WhatsApp på norsk…»</p>
<div itemprop=name>Henrik Sundby</div><div>Ålesund, Norge 🇳🇴</div>
```

Two separate problems:

1. **Self-serving reviews.** Google's structured-data policy forbids review snippets for reviews about your own business, written by your own site. These are disallowed outright regardless of whether they're genuine.
2. **False attribution to a third-party platform.** Presenting site-authored testimonials as Trustpilot reviews is misrepresentation, and Trustpilot actively pursues this separately from Google.

**Fix this first.** Remove every `aggregateRating` you cannot back with a real, auditable review source, and remove the Trustpilot branding from the testimonial carousel entirely. If you want star snippets, collect real reviews through a licensed provider that feeds Google directly.

---

## 4. 🔴 Paid-guest-post link footprint

Your pages link **out** with exact-match commercial anchors to articles on networks widely known for paid placement:

| Destination | Anchor text used |
|---|---|
| `techbullion.com` | "stabil iptv sverige" |
| `timebusinessnews.com` | "IPTV-teknologi" |
| `ventsmagazine.com` | "iptv norden 2026" |
| `usawire.com` | "IPTV Nordic-tjänst", "iptv 2026" |
| `nerdbot.com` | "IPTV Sverige" |
| `bignewsnetwork.com` | "iptv nordic world cup 2026" |
| `kulfiy.com` | "IPTV i Norden" |
| `itechfy.com` | "Lagliga aspekter av IPTV" |
| `techager.com` | "IPTV revolutionerar" |
| `medium.com/@realstreamtv` | "bästa iptv sverige 2026", "IPTV i Sverige", ×8 |

The Medium account `@realstreamtv` is self-evidently yours. The pattern — commercial-anchor outbound links to placement networks, pointing at articles that near-certainly link back — is the standard footprint of a reciprocal link scheme. Google's **link spam** policy covers both buying links and excessive cross-linking for ranking purposes.

**Fix:** add `rel="sponsored"` or `rel="nofollow"` to every one of these, or remove them. They are providing you no user value — none of them are cited as sources in context.

---

## 5. 🟠 Doorway pages / keyword cannibalization

Here I need to correct a common assumption, because the data contradicts it.

I ran 5-word shingle comparison across all 1,431 page pairs. **The pages are not duplicates.** Median pairwise overlap is **10.1%**; only one pair exceeds 30%, and that's `/` vs `/iptv-nordic2/`, which is the same document. **Every page is genuinely uniquely worded.**

So this is *not* a duplicate-content problem. It's a **search-intent duplication** problem — many distinct articles all chasing the same query:

| Head term in `<title>` | Pages competing |
|---|---:|
| "iptv nordic" | **16** |
| "bästa iptv" | **14** |
| "iptv sverige" | **9** |
| "iptv abonnemang" | **7** |
| "iptv norge" | 4 |
| "nordisk iptv" | 4 |
| "nordic iptv" | 4 |

Fourteen pages targeting *bästa IPTV*: `basta-iptv-2026`, `basta-iptv-appen`, `basta-iptv-norden-guide`, `basta-iptv-nordic`, `basta-iptv-nordic-2026`, `best-iptv`, `nordisk-iptv-basta`, plus more. The slugs themselves read as keyword permutations — `iptv-scandinavia-2026` and `iptv-scandinavia-2026-4k`; `nordic-iptv-2026-4k` and `nordisk-iptv-2026-4k`.

Against Google's **doorway abuse** definition — *"sites or pages created to rank for specific, similar search queries… substantially similar pages"* — the intent-level similarity qualifies even though the wording doesn't.

**Consequence right now, independent of any penalty:** these pages split each other's link equity and relevance signals. Google picks one and suppresses the rest. Consolidating 14 pages into 2–3 genuinely strong ones will likely *raise* rankings on its own.

---

## 6. 🔴 Technical SEO defects

### Every page declares the wrong language — 66/66

```html
<html lang="en-GB">  <!-- content is Swedish -->
<meta property="og:locale" content="en_GB">
```

All 66 pages claim British English. The content is Swedish, Norwegian, Danish and Finnish. This misdirects language targeting for your entire site and is a genuine ranking handicap in Nordic SERPs. **Highest-value, lowest-effort fix on this list.**

### hreflang essentially absent

You target 4 countries in 4 languages. Only **2 of 66** pages carry any `hreflang`. Your `/iptv-suomi/`, `/iptv-danmark/`, `/iptv-norge-pro/` and Swedish pages should be a reciprocal hreflang cluster. They aren't.

### Homepage canonical

`https://iiptv.se/` **301-redirects** to `/iptv-nordic2/`, and the sitemap lists **both**. Your root domain is not your homepage. The `2` suffix indicates the original `/iptv-nordic/` page was orphaned and recreated. Serve the homepage at `/` directly.

### Structured-data URLs point at redirects

`Product.url` → `https://iiptv.se/iptv-nordic/` (301) and `https://iiptv.se/iptv-norway/` (301). Schema should reference canonical URLs.

### Titles and headings

- **No H1:** `/iptv-installation/`, `/iptv-nordic2/checkout/`, `/iptv-nordic/blog/`
- **Two H1s:** `/iptv-smart-tv-installation-sverige/`
- **Placeholder titles:** `/iptv-installation/` → "iptv installation"; `/iptv-kanallista/` → "iptv kanallista" (lowercase, unwritten)
- **7 pages** have no meta description
- **Stale year in live titles:** `basta-iptv-appen` → "…fungerar perfekt **2025**"; `iptv-abonnemang` → "Bästa IPTV **2025**…" — while the rest of the site markets 2026

### Page weight

Average **375 KB of HTML alone** (max 684 KB), before CSS/JS/images. That is roughly 10× a normal page and will be hurting LCP/INP on mobile. Much of the bulk is inlined CSS and the stuffing blocks.

### Image alt text
813 images, only 8 missing alt (1.0%) — **this is genuinely good.** Though `alt="iptv nordic"` repeated 73× is over-optimized; describe the image instead.

---

## 7. 🟠 Contradictory factual claims (E-E-A-T)

Channel counts across the site do not agree with each other:

| Claim | Occurrences |
|---|---:|
| 150 000 kanaler | 141 |
| 100 000 kanaler | 114 |
| 22 000 kanaler | 2 |
| 30 000 kanaler | 2 |
| **1 800 kanaler** | 3 |

`/iptv-kanallista/` — your actual *channel list* page, the most authoritative page on this subject — has the H1 **"Över 1800+ Kanaler"**, while 141 other mentions claim 150,000. An 83× discrepancy on your core product spec.

Pricing is similarly inconsistent: the homepage title advertises **"39 kr/mån"** (3 mentions site-wide) while the dominant price on the site is **249 kr/mån** (44 mentions). A title-tag price that appears almost nowhere in the content reads as bait-and-switch to both users and quality raters.

Also: footer copyright reads **© 2025**.

Pick one channel number, one price structure, and propagate them.

---

## 8. The risk factor underneath all of this

Worth stating plainly, since it shapes how much the rest will pay off: the site sells unlicensed access to Viaplay, sports and premium broadcast content, and carries pages like `/iptv-polisen/` and `/iptv-nordic-fullstandig-guide-2026/` ("Olagligt? Straffbart?") that address exactly that. Google applies demotion signals to sites subject to repeated DMCA removals, and rightsholders in the Nordics actively file them.

This means the ceiling on organic performance here is set by something other than on-page SEO. I've given you the full technical audit you asked for, and every fix below is worth doing — but I'd be misleading you if I implied that fixing density and schema unlocks stable rankings for this content category.

---

## Priority action list

### Do this week
1. **Remove all fabricated `aggregateRating` markup** and the Trustpilot-attributed review carousel (§3). Highest manual-action risk.
2. **Change `lang="en-GB"` → `lang="sv-SE"`** (and `nb-NO`/`da-DK`/`fi-FI` per page), fix `og:locale` (§6). Biggest win per minute spent.
3. **Delete the "Relaterade sök" keyword-pill blocks** from all 8 affected pages (§1). Target <2.5% density.
4. **Add `rel="sponsored"`** to the 10 guest-post outbound domains (§4).

### Next 2–4 weeks
5. **Consolidate the "bästa IPTV" cluster** — 14 pages → 2–3. 301 the rest into the survivors (§5).
6. Same for "iptv sverige" (9) and "iptv nordic" (16).
7. Serve the homepage at `/` instead of 301→`/iptv-nordic2/` (§6).
8. Reconcile channel count and pricing sitewide (§7).
9. Fix missing/duplicate H1s, placeholder titles, 7 missing meta descriptions, 2025 dates, footer year (§6).

### Ongoing
10. Build a real hreflang cluster across the SE/NO/DK/FI pages (§6).
11. Cut HTML weight from 375 KB average — most is inlined CSS (§6).
12. Rewrite the retained pages so each answers a distinct question. Uniqueness of *wording* is already fine; uniqueness of *purpose* is what's missing (§2, §5).

---

## Method note

All figures are measured, not estimated. 66 URLs fetched live with a browser user-agent; HTML parsed with BeautifulSoup/lxml; `<script>`, `<style>`, `<noscript>`, `<svg>`, `<iframe>` stripped before text extraction. Density computed on Unicode word tokens including `åäöæø`. Boilerplate identified as text segments appearing on ≥50% of pages and subtracted for the second density pass. Near-duplicate detection used 5-word shingles with Jaccard and containment coefficients over all 1,431 page pairs.
