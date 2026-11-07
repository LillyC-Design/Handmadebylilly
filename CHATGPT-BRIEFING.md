# Briefing for ChatGPT: the Handmade by Lilly website

*Lilly: upload this file at the start of any ChatGPT chat about your website, then ask your question.*

---

**ChatGPT, please read this before answering.**

You are helping Lilly, a crochet maker with no coding background, run her shop website. She edits files directly on the GitHub website (pencil icon → Commit changes). She cannot run commands on her computer. When she commits, GitHub Actions runs `node build.mjs`, which turns the content files into the finished website and publishes it to GitHub Pages. If the build fails, the live site keeps its last good version, and the error shows in the repository's **Actions** tab.

## How to help her

- Give **complete, ready-to-paste file contents**, or very exact "find this line, replace with this" instructions. Never give fragments she has to merge.
- **Keep JSON valid:** double quotes, commas between items, no comma after the last item, no comments. When she pastes a JSON file, check it before she commits.
- Use **British English**, warm and simple, in Lilly's voice (first person: "I", "my").
- Don't change `build.mjs`, `static/js/` or `.github/` unless she asks for a change to the design or behaviour. Most jobs only touch `content/` and `static/images/`.
- If she pastes a build error, explain it in one or two plain sentences and give the exact fix.

## Repository layout

```
.github/workflows/deploy.yml  builds and publishes the site on every commit (don't edit)
build.mjs                     the generator: page templates, page text, legal pages
serve.mjs                     local preview server (not needed on GitHub)
README.md                     technical notes
content/site.json             business name, email, Instagram, address of the site, GA4 ID
content/products.json         every product (array)
content/stock.json            "available" or "sold" for each product
content/events.json           craft fairs/markets (array, past events hide automatically)
content/blog/*.md             journal posts
content/pattern-guides/*.md   pattern guides
content/technical-details/*.md  stitch library entries
static/images/                all photos and the logo
static/css/site.css           design
static/js/site.js             small interactive extras (menu, shop filter, animations)
```

## content/site.json

```json
{
  "name": "Handmade by Lilly",
  "tagline": "From my hands to your heart",
  "description": "…used for Google on the homepage…",
  "siteUrl": "https://USERNAME.github.io",
  "basePath": "",
  "email": "Handmadebylilly@outlook.com",
  "instagram": "https://www.instagram.com/handmade_by_lilly_crochet/",
  "instagramHandle": "@handmade_by_lilly_crochet",
  "ga4Id": "",
  "dispatch": "within 3 working days",
  "dataRetention": "1 year",
  "policyUpdated": "2026-09-25",
  "productSignOff": "Handmade by Lilly – Made uniquely, just like you."
}
```

- `siteUrl` is the full address with no trailing slash. `basePath` is `""` if the repo is named `USERNAME.github.io`, otherwise `"/repo-name"`.
- Putting a Google Analytics ID in `ga4Id` switches on a cookie-consent banner and updates the privacy and cookie policies automatically. Analytics only loads after a visitor clicks Accept, as UK law requires.
- If `policyUpdated` changes, the "Last updated" date on the legal pages changes too.

## content/products.json: one product

```json
{
  "slug": "sand-dune",
  "name": "Sand Dune",
  "subtitle": "Handmade Crochet Fan Stitch Shawl",
  "category": "Shawls",
  "price": 135,
  "stripe": "https://buy.stripe.com/…",
  "cardLine": "Natural elegance, delicate texture and timeless comfort.",
  "cardFacts": "50% wool · 200 cm wingspan",
  "metaDescription": "A unique 1–2 sentence description for Google, at least 50 characters.",
  "images": [
    { "file": "sand-dune-crochet-shawl-flat-lay", "alt": "What the photo shows, in plain words." },
    { "file": "sand-dune-crochet-shawl-styled", "alt": "…" }
  ],
  "description": ["Paragraph one.", "Paragraph two."],
  "details": [["Colour", "Natural beige"], ["Material", "50% wool, 50% acrylic"], ["Wingspan", "200 cm"]],
  "care": "Hand wash gently in cool water…",
  "closing": "An italic closing line, or \"\" for none."
}
```

- `slug` becomes the web address (`/shop/sand-dune/`): lower case, dashes, never change it once live.
- `price` is a plain number in pounds (no £ sign, no quotes). UK delivery is included and there is no VAT.
- `stripe` is optional. Without it, every Buy button opens an email to Lilly. With it, Buy goes to Stripe checkout.
- `category` can be any word, for example `Shawls`, `Wraps` or `Scarves`. New categories appear in the shop filter automatically.
- The first image is the shop-card photo. Photos are always shown whole, never cropped.
- Optional: `"instagram": "https://www.instagram.com/reel/…"` adds a "Watch it move on Instagram" link.
- Optional: `"draft": true` keeps a product in the file but off the website.
- Every product also needs a line in `content/stock.json`: `"sand-dune": "available"`.
- The order in the file is the order in the shop. Sold pieces move to the end automatically and show "In the archive".

## Photos (static/images/)

- In a product's `images[].file` (or a post's `image`), write the file name **without** the extension.
- Either upload one photo named `name.jpg` (also `.jpeg`, `.png` or `.webp`),
- or, for faster loading, two sizes named `name-1170.jpg` and `name-768.jpg` (the number is the width in pixels). The site picks the right size automatically.
- Use lower case and dashes, no spaces: `rosehip-crochet-shawl-flat-lay.jpg`.
- Keep photos under about 500 KB. 1200 pixels wide is plenty.
- If a referenced photo is missing, the build fails with the message `Missing image: "name"`.

## Journal posts (content/blog/my-post-title.md)

```markdown
---
title: The post title
description: One or two sentences for Google and the post list, at least 50 characters, unique.
date: 2026-11-20
image: photo-name-without-extension
alt: What the photo shows
---

Opening paragraph.

## A subheading

Text with **bold**, *italic* and [a link](https://example.com).

- A bullet point
- Another
```

The file name becomes the address (`/resources/blog/my-post-title/`). Pattern guides (`content/pattern-guides/`) use the same format plus `level: Beginner`. Stitch-library entries (`content/technical-details/`) add `stitch:`, `difficulty:`, `fabric:` and `uses:`, and don't need an image.

## content/events.json

```json
[
  {
    "title": "Christmas Makers' Market",
    "date": "2026-12-05",
    "time": "10am to 4pm",
    "location": "Town Hall, High Street, Anytown",
    "agenda": "New winter pieces to see and try on."
  }
]
```

Dates use the format YYYY-MM-DD. Past events disappear on the next update. With no upcoming events, the page shows a friendly "no dates yet" message.

## Rules the build enforces

The build stops on purpose if:

- two pages share the same title or meta description
- any description is under 50 characters
- a photo is missing

Fix the content and commit again.

## Text that lives in build.mjs

The homepage text, About page story, FAQs, Terms of sale, Privacy policy and Cookie policy are written inside `build.mjs`: in the functions `home()`, `about()` and `legal()`, and in a list called `faqs` (the questions on the Contact page). To change them, find the exact sentence and replace only the words, keeping the surrounding quotes and `${…}` parts untouched. The homepage and About photos are referenced there too (`img("lilly-hero", …)` and `img("plum-orchard-shawl", …)`): to swap one, upload the new photo and change that name.

## Legal points to keep

- UK only, delivery included in the price, not VAT-registered.
- Every piece is ready-made, so the 14-day right to cancel applies. Lilly pays return postage, and refunds are made within 14 days of the item coming back. This is shown on every product page before purchase, which UK law requires.
- The privacy policy names Stripe (payments), Microsoft/Outlook (email) and GitHub (hosting). If she adds another service, such as a newsletter tool, add it there.
- The repository is **public**: never put private or financial data, passwords or secret keys in it.
