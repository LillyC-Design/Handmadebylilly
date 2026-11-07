# Handmade by Lilly: your website, start here

Hi Lilly,

This zip contains your whole website. Follow these steps once to get it live on your own GitHub account. After that, changing the site is just editing a text file on GitHub.

What's inside:

- **`website/`**: the website itself. Everything in this folder goes onto GitHub.
- **`CHATGPT-BRIEFING.md`**: a description of how the site works, written for ChatGPT. Upload it at the start of any ChatGPT chat about the website, so ChatGPT gives you correct answers.
- **`START-HERE.md`**: this guide.

The site is free to run. GitHub hosts it, and it rebuilds itself automatically every time you save a change.

---

## Part 1: Put the site on your GitHub (one time only)

### Step 1. Create a GitHub account

Go to <https://github.com> and sign up. Then turn on two-factor authentication: **Settings → Password and authentication → Enable two-factor authentication**. This matters: your GitHub login is the only key to your website.

### Step 2. Create a repository

A "repository" (or "repo") is the folder on GitHub that holds your website.

1. Click **+** (top right) → **New repository**.
2. **Repository name:** choose one of these:
   - **`YOUR-USERNAME.github.io`** (recommended), using your exact GitHub username. Your site will live at `https://YOUR-USERNAME.github.io/`.
   - Or any name, for example **`handmade-by-lilly`**. Your site will then live at `https://YOUR-USERNAME.github.io/handmade-by-lilly/`.
3. Set it to **Public**. Free GitHub accounts can only publish websites from public repositories.
4. Leave everything else unticked and click **Create repository**.

### Step 3. Tell the website its new address

Before uploading, open `website/content/site.json` in Notepad (Windows) or TextEdit (Mac) and change these two lines:

If you named the repo `YOUR-USERNAME.github.io`:

```json
  "siteUrl": "https://YOUR-USERNAME.github.io",
  "basePath": "",
```

If you named it something else, for example `handmade-by-lilly`:

```json
  "siteUrl": "https://YOUR-USERNAME.github.io/handmade-by-lilly",
  "basePath": "/handmade-by-lilly",
```

Use your real username. Keep the quotes and the comma at the end of each line, and don't put a `/` at the end. Save the file.

### Step 4. Upload the files

1. On your new, empty repository page, click the link **"uploading an existing file"**.
2. Open the `website` folder on your computer, select **everything inside it** (not the `website` folder itself), and drag it all onto the GitHub page.
3. Wait for the uploads to finish, then click **Commit changes**.

**Check:** your repository should now show the folders `.github`, `content` and `static`, plus files like `build.mjs` and `README.md`.

> **If you don't see `.github`:** Macs hide folders whose names start with a dot, so it may not have been dragged in. Fix it like this: click **Add file → Create new file**, type the name `.github/workflows/deploy.yml`, then copy everything from the file `website/.github/workflows/deploy.yml` into the box and click **Commit changes**. On a Mac, press **Cmd + Shift + .** in Finder to show hidden folders.

### Step 5. Switch on the website

1. In your repository, go to **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **GitHub Actions**.
3. Go to the **Actions** tab. Click **Build and deploy** on the left, then **Run workflow → Run workflow**.
4. Wait about a minute until the run shows a **green tick**.
5. Visit your address from Step 2. Your site is live.

If the run shows a **red cross**, click it, click **deploy**, copy the red error text and paste it into ChatGPT along with `CHATGPT-BRIEFING.md`.

---

## Part 2: Changing the site day to day

You never need to install anything. All changes are made on the GitHub website:

1. Open the file you want to change in your repository.
2. Click the **pencil icon** (Edit).
3. Make the change, then click **Commit changes**.
4. Within 1 to 2 minutes the live site updates. Check the **Actions** tab for the green tick.

**Before you commit, ask ChatGPT to check the file.** Paste the whole file and ask "Is this valid JSON?". A single missing comma or quote stops the site updating. Nothing breaks on the live site: it just keeps the last good version until the error is fixed.

### The files you'll actually use

| To change… | Edit this file |
| --- | --- |
| Mark a piece **sold** or **available** | `content/stock.json` |
| Product details, prices, photos, **Stripe links** | `content/products.json` |
| Email address, Instagram, dispatch time | `content/site.json` |
| Craft fairs and markets | `content/events.json` |
| Journal posts | `content/blog/` (one file per post) |
| Photos | `static/images/` (use **Add file → Upload files**) |

### Common jobs

**A piece has sold.** In `content/stock.json`, change its `"available"` to `"sold"`. Then, in Stripe, **deactivate that piece's Payment Link**. Otherwise someone with the link could still pay for it.

**Add a Stripe Payment Link.** In `content/products.json`, find the product and add a line like this after its `"price"` line:

```json
    "stripe": "https://buy.stripe.com/your-link-here",
```

Every "Buy" button for that piece then goes straight to Stripe checkout. Until a piece has a link, its Buy buttons open an email to you.

**Add a new product.** Ask ChatGPT: *"Using the briefing, write a new products.json entry for [details]."* Upload its photos to `static/images/` with simple names: lower case, words joined by dashes, no spaces (for example `rosehip-crochet-shawl-flat-lay.jpg`). Also add the piece to `content/stock.json`.

**Add a journal post.** Ask ChatGPT to write it as a blog file (the briefing explains the format), then create it in `content/blog/` with **Add file → Create new file**.

---

## Part 3: Still to do

- [ ] **Stripe Payment Links** for all 9 products.
- [ ] **Misty Woodland**: price, material, wingspan, drop, hours and care. It's hidden until done. In `products.json`, fill in the details, then delete the line `"draft": true,`.
- [ ] **Check these product details:** Sapphire Waves (are the stripes silver-grey or pale blue?), Shamrock Grace (cream or light grey?), Lavender (is the styled photo a fair picture of 100 × 40 cm?), Autumn (are its care instructions right?).
- [ ] **Dispatch time:** the site says "dispatched within 3 working days". Change `"dispatch"` in `content/site.json` if that's not right.
- [ ] **About page:** the story there is a placeholder written in your voice. Replace it with your own words. Ask ChatGPT to help, and tell it the text lives in `build.mjs` in the `about()` section.
- [ ] **Homepage and About photos:** the large photo at the top of the homepage, the About page photo and the plum photo halfway down the homepage are placeholders, not your pieces. Replace them with your own photos.
- [ ] **Starter articles:** the journal posts, pattern guides and stitch library were written to fill the pages. Read them and replace or add your own.
- [ ] **Google Search Console:** register your new address at <https://search.google.com/search-console> and submit `sitemap.xml`. This helps Google find the site.
- [ ] **Google Analytics (optional):** if you set it up, put the ID (`G-XXXXXXXXXX`) in `"ga4Id"` in `content/site.json`. A cookie banner then appears automatically, and the privacy and cookie pages update themselves.

## Rules to keep the site safe

- **Your repository is public:** anyone can read every file in it. Never upload spreadsheets with costs or suppliers, passwords, or Stripe secret keys. Stripe *Payment Links* (`buy.stripe.com/…`) are fine: they're meant to be public.
- **Every page needs its own description.** If two products end up with the same `metaDescription`, the update stops with an error, on purpose, because Google treats duplicate pages badly. Make each one different.
- **Every photo needs a description (`alt`)** saying what it shows. Screen readers and Google use it.

Made with care. The site was built by otobrothers.
