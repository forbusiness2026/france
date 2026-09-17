# abonnement-iptv-stable.fr — template clean-up & edit list

**Inspected:** 2026-09-17 · **Stack:** WordPress 7.1 + Elementor 4.2.4 + Rank Math + Site Kit, hosted on OVH
**Scope:** all 12 URLs in `page-sitemap.xml`, crawled and parsed (HTML, JSON-LD, canonicals, robots, sitemaps, REST root)

This is the "what the template left behind" list. Items are ordered by how much damage they
do, not by how hard they are to fix. The first three are 10-minute settings fixes with
outsized impact.

---

## 1. 🔴 Every page except the homepage canonicalises to `http://`

| Page | `<link rel="canonical">` |
|---|---|
| `/` | `https://abonnement-iptv-stable.fr/` ✅ |
| `/iptv-premium/` | `http://abonnement-iptv-stable.fr/iptv-premium/` ❌ |
| `/iptv-suisse/` | `http://…/iptv-suisse/` ❌ |
| `/abonnement-iptv-12-mois-smart-tv/` | `http://…` ❌ |
| `/abonnement-iptv-28/contact/` | `http://…` ❌ |

`http://` URLs 301-redirect to `https://`, so each page is telling Google "the real version
of me is a URL that doesn't serve content." Same problem in the sitemap: 11 of 12 `<loc>`
entries are `http://`.

WordPress itself is configured correctly — the REST root reports
`url: https://…` and `home: https://…`. So the stale `http://` is **Rank Math's cached
metadata**, saved back when the site had no SSL.

**Fix:** Rank Math → Status & Tools → Database Tools → *Clear Rank Math Cache*, then
*Rebuild Sitemap*. Re-check one page's canonical afterwards; if it's still `http://`, the
per-page canonical was hard-typed in the page's Rank Math sidebar and needs clearing there.

## 2. 🔴 Fabricated review & rating structured data

This is the highest-risk thing on the site — it is the one item here that carries
**manual-action** risk rather than just ranking suppression, because it's a Google
structured-data policy violation (self-serving reviews + review markup that doesn't match
anything visible on the page).

| Page | Markup | Claimed |
|---|---|---|
| `/` | `Product` → `AggregateRating` | 5.0 from **8** reviews, 8 named `Review` blocks, all 5★ |
| `/iptv-premium/` | `Product` → `AggregateRating` | 4.9 from **1250** reviews |
| `/iptv-suisse/` | `Product` → `AggregateRating` | 4.9 from **1450** reviews |
| `/abonnement-iptv-12-mois-smart-tv/` | **two** `Product` blocks | 4.9/**2840** *and* 4.9/**128** |

Two separate problems:

- **The counts aren't real.** 1250, 1450 and 2840 reviews with no review system on the site
  and no third-party profile to back them. Google's policy forbids review markup a site
  writes about its own products, and forbids ratings with no corresponding on-page content.
- **The same authors appear on multiple pages.** "Lucas M." and "Camille B." are 5★ reviewers
  on both `/` and `/abonnement-iptv-12-mois-smart-tv/`; the homepage's 8 reviewers are the
  template's placeholder set.

**Fix:** delete the `AggregateRating` and `Review` nodes. Keep `Product`, `Offer` and
`FAQPage` — those are legitimate and useful. If you want star ratings back later, they have
to come from a real review system (a WP review plugin, or a Trustpilot/Google Business
profile) where the count matches reality.

## 3. 🔴 `/abonnement-iptv-28/` — seven pages nested under a 404

Every legal/support page lives under a parent slug that doesn't exist:

```
/abonnement-iptv-28/                            → 404  ("This page doesn't seem to exist.")
/abonnement-iptv-28/mentions-legales/           → 200
/abonnement-iptv-28/politique-de-confidentialite/ → 200
/abonnement-iptv-28/politique-de-remboursement/ → 200
/abonnement-iptv-28/conditions-generales-dutilisation/ → 200
/abonnement-iptv-28/liste-des-chaines/          → 200
/abonnement-iptv-28/instalation-iptv/           → 200
/abonnement-iptv-28/contact/                    → 200
/abonnement-iptv-28/merci-contact/              → 200
```

`abonnement-iptv-28` is the template author's own page slug — it came with the import and
the parent page was deleted, leaving the children orphaned. Two things are wrong: the
breadcrumb trail on those pages passes through a 404, and the URLs advertise somebody
else's naming.

Also note `instalation-iptv` — the template's typo, single `l`. It should be
`installation-iptv`.

**Fix:** in Pages → Quick Edit, set each page's *Parent* to "(no parent)" so they become
`/mentions-legales/`, `/contact/`, `/liste-des-chaines/`, `/installation-iptv/` etc. Then
add 301 redirects from the old paths (Rank Math → General Settings → Redirections) so no
existing links break.

## 4. 🟠 Four different phone numbers and three different emails

The contact details were never unified after the import.

| Number | Where it appears |
|---|---|
| `+33 7 48 66 23 75` (`wa.me/33748662375`) | every page — the real one, in the floating WhatsApp widget |
| `+33 7455 46243` | sitewide in body text — **and it's malformed**, a French mobile is 9 digits after `+33`, this has 9 in the wrong grouping |
| `+33 6 44 65 70 45` (`wa.me/33644657045`) | homepage + `/contact/` |
| `+44 7782 260559` (`wa.me/447782260559`) | `/contact/`, `/mentions-legales/`, `/politique-de-remboursement/` — **a UK number on a French site** |
| `wa.me/447877418908` | homepage — a second UK number |

Emails: `support@`, `contact@`, `dmca@`, `admin@` — all four used, inconsistently, for
overlapping purposes.

**Fix:** pick one WhatsApp number and one support email, then replace everywhere. The UK
numbers are almost certainly the template author's and should go first — they're on your
legal pages, which is the worst place for a contradiction.

## 5. 🟠 Keyword stuffing — same pattern the iiptv.se audit flagged

Healthy density for a primary term is roughly 0.5–2.0%.

| Page | Body words | "iptv" | Density |
|---|---:|---:|---:|
| `/iptv-suisse/` | 2,896 | 341 | **11.77%** |
| `/iptv-premium/` | 2,659 | 238 | **8.95%** |
| `/abonnement-iptv-12-mois-smart-tv/` | 2,834 | 137 | **4.83%** |
| `/` | 1,978 | 99 | **5.01%** |
| `/mentions-legales/` | 1,425 | 26 | 1.82% ✅ |

The mechanism is the same as on iiptv.se: hashtag/pill blocks that are pure keyword lists,
e.g. on `/iptv-premium/`:

> `# Abonnement IPTV  # Premium IPTV France  # IPTV Premium Stable  # Acheter IPTV 4K  # IPTV Pas Cher`
> `# Abonnement IPTV  # IPTV 4K France  # Premium IPTV stable  # Acheter IPTV Premium`

and on `/iptv-suisse/`, where "Abonnement IPTV Suisse" is repeated as a standalone heading or
label **198 times** in under 3,000 words.

**Fix:** delete the hashtag/pill widgets entirely — they rank for nothing and they're the
clearest stuffing signal on the site. Then on `/iptv-suisse/` and `/iptv-premium/`, replace
repeated "Abonnement IPTV Suisse" labels with pronouns and short forms ("le service",
"l'abonnement", "nos offres"). Target under 2%.

## 6. 🟠 Prices contradict each other across meta, schema and page

| Source | Price for 12 months |
|---|---|
| Homepage meta description | "12 mois + 6 mois offerts à **59,95€**" |
| `/abonnement-iptv-12-mois-smart-tv/` page text | **59,99€** (struck through: 149,99€) |
| Same page, JSON-LD `Offer` | **49.00 EUR** |

Three numbers for one product. The 49.00 in schema is what Google may show in a rich result,
so a visitor can arrive expecting €49 and see €59.99 — that's the kind of mismatch that gets
rich results suppressed, and it's a consumer-protection problem in France regardless of SEO.

`/iptv-suisse/` has the same drift: the schema advertises 24.90 / 44.90 / 64.90 / 94.90 CHF,
but the page also displays 59.90, 69.90, 104.90, 159.90 and 169.90 CHF.

**Fix:** decide the real price list, then update in three places per product — page text,
Rank Math meta description, and the JSON-LD `Offer`.

## 7. 🟡 Brand name is inconsistent in four variants

Schema and page copy disagree on what the business is called:

- `Organization.name` on `/` → **"Abonnement IPTV"**
- `Organization.name` on `/contact/` → **"Abonnement IPTV Illimité"**
- `Product.brand` on `/abonnement-iptv-12-mois-smart-tv/` → **"IPTV France Pro"** and **"IPTV Premium France"**
- `Product.brand` on `/iptv-premium/` → **"IPTV Premium"**

"IPTV France Pro" in particular reads like the template author's brand. Pick one name and use
it in every `Organization` and `brand` field.

## 8. 🟡 Locale is declared as US English

All 8 pages carry `<meta property="og:locale" content="en_US">` on French content, and there
are no `hreflang` tags. Set Rank Math's social locale to `fr_FR`. If you intend
`/iptv-suisse/` to target Switzerland, add `hreflang="fr-CH"` on it and `hreflang="fr-FR"`
elsewhere — otherwise the two French-language pages compete with each other.

## 9. 🟡 Site tagline holds an SEO title

The WP REST root reports the site *description* as
`"Abonnement IPTV – Meilleur IPTV France | 4K, Sport & Séries"` — identical to the title.
Somebody pasted the SEO title into Settings → General → Tagline. It leaks into feeds and
theme markup. Replace it with a real one-line tagline, or empty it.

## 10. 🟡 robots.txt points at the wrong sitemap

robots.txt advertises `wp-sitemap.xml` (WordPress core). That URL 301s to Rank Math's
`sitemap_index.xml`, so crawlers do get there — but declare the real one directly:

```
Sitemap: https://abonnement-iptv-stable.fr/sitemap_index.xml
```

Also worth noting: `sitemap_index.xml` lists only `page-sitemap.xml`. There is no post
sitemap, so the site currently has **zero blog content** — 12 pages total.

---

## Suggested order of work

1. Clear Rank Math cache + rebuild sitemap → fixes §1 (canonicals, sitemap protocol)
2. Strip `AggregateRating` / `Review` from all four pages → removes §2, the manual-action risk
3. Unparent the `/abonnement-iptv-28/` pages + add redirects, fix the `instalation` typo → §3
4. Unify phone/email, deleting the two UK numbers first → §4
5. Delete the hashtag/pill widgets, then thin the repeated labels on the two worst pages → §5
6. Reconcile the price list across page text, meta and schema → §6
7. Settings pass: brand name, `fr_FR` locale + hreflang, tagline, robots.txt → §7–10

## A note on §2 and §6

Items 1, 3, 4, 5, 7, 8, 9 and 10 are pure clean-up — nothing lost. Items 2 and 6 trade a
visible number for accuracy: removing the star ratings loses the rich-result stars, and
reconciling prices means committing to one figure. Both are worth doing anyway. Invented
review counts and a schema price that undercuts the checkout price are the two things on this
site most likely to cost you the whole domain rather than a few positions, and in France a
displayed price that differs from the price charged is an advertising problem on its own.
