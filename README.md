# BASRA STYLE

### MOVE. GLOW. OWN IT. ✨

A premium women's activewear & fitness lifestyle frontend prototype for the Iraqi market — mobile‑first, bilingual (English/Arabic, full RTL), with IQD/USD pricing.

This is a **client‑demo frontend prototype**. There is no backend, real payment, real authentication, or real order processing yet — it's built so those can be added later without reworking the UI.

---

## 1. Project overview

BASRA STYLE is a single self‑contained web app covering:

- Home (hero, categories, trending, Build Your Fit, Mystery Box, 7‑Day Challenge, offers, Instagram-style grid)
- Shop (search, filters, sort, product grid)
- Product quick view & full product detail modal
- Cart (add/remove/qty, subtotal, estimated delivery, demo checkout)
- Wishlist
- Profile
- Language switch (English / Arabic with true RTL layout)
- Currency switch (IQD / USD)
- WhatsApp contact CTA

The whole app — markup, styles, and logic — lives in one file, `index.html`, by design (see "Main files" below).

## 2. How to run it

No build step, no server, no dependencies to install.

1. Download/unzip the project.
2. Double‑click `index.html`, or open it in any browser (`File → Open`).
3. That's it — the app runs entirely client‑side.

It also works fine served from any static host (Netlify, Vercel, GitHub Pages, a plain Apache/Nginx folder, etc.) — just deploy the whole `BASRA-STYLE/` folder.

## 3. Main files

```
BASRA-STYLE/
├── index.html          ← the entire application (HTML + CSS + JS)
├── assets/
│   ├── images/          ← put real product/campaign photography here
│   ├── icons/            ← put a real favicon / app icons here if desired
│   └── fonts/             ← put self-hosted font files here if you want to stop relying on Google Fonts
└── README.md
```

Inside `index.html`, everything is organized top‑to‑bottom and clearly commented:

- `CONTACT_CONFIG` — WhatsApp number & message, Instagram, email
- `BRAND_CONFIG` — brand name, tagline, currency exchange rate, shipping rules
- `translations` — the English/Arabic dictionary
- `categories` — the 6 shop categories
- `products` — the full product catalog
- CSS `:root` variables at the top of `<style>` — all brand colors, fonts, radii, shadows
- Everything else below is rendering functions and app state (cart, wishlist, filters, language, currency)

## 4. How to change products

Edit the `products` array in `index.html` (search for `DATA — Products`). Each product is a plain object:

```js
{ id:1, name:{en:"Sculpt Leggings",ar:"ليجنز سكالبت"}, priceIQD:39000, oldPriceIQD:49000,
  category:"gym", sizes:["S","M","L","XL"], colors:["Black","Pink"],
  badge:"trending", rating:4.8, reviews:126, inStock:true, seed:2 }
```

- `category` must match one of the ids in the `categories` array (`gym`, `running`, `yoga`, `training`, `lifestyle`, `accessories`).
- `badge` can be `"trending"`, `"new"`, `"best_seller"`, `"limited"`, or `""`.
- `seed` only controls which abstract placeholder graphic is generated — see next section.
- Add or remove objects from the array freely; the grid, carousels, filters and search all read from this one list.

## 5. How to change images

The prototype currently uses generated abstract color‑block graphics (no external photos) so the file works fully offline with zero broken links, **except for the products/photos listed below, which now use real photography/video.**

### Real photo — Flow Yoga Set (id 5)
`assets/images/products/product-set-brown-flow.jpg` — cropped from a supplied studio shot (matching bra + leggings set), wired into `PRODUCT_IMAGES[5]`.

### Real product videos (`PRODUCT_VIDEOS`)
Seven supplied lifestyle/stock clips were compressed (480px wide, ~8s, ~700–850 KB each, down from up to 70 MB) and placed in `assets/videos/products/`. They're wired into the new `PRODUCT_VIDEOS` map (search `PRODUCT_VIDEOS` in `index.html`) and play automatically in the quick‑view / product‑detail modal for any product that has no still photo, via the existing `productVideoSrc()` fallback chain (product video → category video → shop video):

| Product | Video |
|---|---|
| Windbreak Running Jacket (id 4) | `product-run-jacket-trophy.mp4` |
| Power Training Shorts (id 7) | `product-training-shorts-squat.mp4` |
| Core Training Tee (id 8) | `product-training-tee-basketball.mp4` |
| Everyday Oversized Hoodie (id 9) | `product-hoodie-lifestyle-walk.mp4` |
| Weekend Jogger Pants (id 10) | `product-jogger-lifestyle-dock.mp4` |
| Grip Training Socks (id 12) | `product-training-socks-kneepads.mp4` |
| Reflect Night Run Vest (id 14) | `product-run-vest-urban-jog.mp4` |

These are curated lifestyle/motion clips, not literal single‑product shoots — swap any of them out any time by replacing the file path in `PRODUCT_VIDEOS` (or the file itself, keeping the same name). To swap in more real photography for the remaining products:

1. Put your image files in `assets/images/` (e.g. `assets/images/sculpt-leggings.jpg`).
2. Find the `campaignTile(seed, tone)` calls for the section you want to change (product cards, category cards, hero, offers, Instagram grid).
3. Replace the generated `<svg>` output with a real `<img src="assets/images/your-file.jpg" alt="...">`, keeping the same wrapping `<div class="tile">…</div>` so the rounded corners, aspect ratio and overlay styling stay intact.

Doing this per‑product is more editing than a CMS-driven site, but it's a direct, predictable swap — no data structure needs to change.

## 5b. Settings page — managing products yourself

A small gear icon in the footer (bottom of every page, next to the copyright line) opens a **Settings** login. This is deliberately low-key — it's not part of the main customer navigation, so regular shoppers browsing the live site won't notice it, but it's there for the store team to use any time.

- **Login:** username `هدى`, password `12345` (see `ADMIN_CONFIG` near the top of the `<script>` block to change either one).
- **Note:** this is a client-side demo gate, not real authentication/security — anyone who opens the browser's dev tools could read the password in the page source. It's meant to keep casual visitors out, not to be a production login system. A real backend (see README section 10) would replace this with proper auth.
- A correct login takes you straight to a full **Settings page** listing every product, **sorted by price**. Each product has its own card with:
  - **Image upload** — pick a file from your device; it's resized and saved immediately (no Save click needed for the image itself), replacing that product's photo everywhere on the site (grid, quick view, full product page). A ✕ button appears to remove it and fall back to the original photo/placeholder.
  - **Offer label** — a dropdown (No offer / New / Trending / Best seller / Limited) that sets the badge shown on the product card.
  - **Discount pricing** — a "current price" field and an optional "price before discount" field; fill the second one to show a struck‑through original price next to the new price, exactly like the built‑in sale items.
  - **Description (Arabic / English)** — two text boxes to override the product's default detail‑page description; leave either blank to keep the default auto‑generated text for that language.
  - A **Save** button per product commits the offer/price/description fields together (the image saves on upload, independently).
- All of this — uploaded images and saved offer/price/description edits — is stored in the browser's `localStorage` (the same mechanism already used for cart/wishlist), so it persists across reloads **on that browser/device**. It is not synced to a server and won't be visible to a shopper on a different device — for that, wire up a real backend per README section 10.
- The login itself is **not** remembered — logging out, or reloading the page, sends you back to the normal storefront and the Settings gear will ask for the username/password again.

## 6. How to change the WhatsApp number

Edit `CONTACT_CONFIG` near the top of the `<script>` block:

```js
const CONTACT_CONFIG = {
  whatsappNumber: "9647700000000", // ← change this
  whatsappDefaultMessage: "Hi BASRA STYLE! I need help choosing my size.",
  instagram: "@basrastyle",
  email: "hello@basrastyle.example",
};
```

The number must be in international format with no `+`, `00`, spaces or dashes (e.g. `9647701234567`). This single value updates the floating WhatsApp button everywhere.

## 7. How to change brand colors

Edit the CSS variables at the very top of the `<style>` block:

```css
:root{
  --color-primary:#FF5C7A;
  --color-dark:#111111;
  --color-cream:#FFF8F5;
  --color-peach:#FFB199;
  --color-lime:#C8F23D;
  --color-white:#FFFFFF;
  --color-muted:#777777;
}
```

Because every component references these variables (not hard‑coded hex values), changing a value here updates the whole app consistently.

## 8. How to change Arabic/English text

Edit the `translations` object (search for `TRANSLATIONS`). It has two blocks, `en` and `ar`, with matching keys:

```js
en: { nav_home:"Home", hero_line1:"MOVE.", ... }
ar: { nav_home:"الرئيسية", hero_line1:"تحرّكي.", ... }
```

Change the string value on either side — the key names must stay the same since the app looks text up by key. Product names are translated separately, inline on each product (`name:{en:"...", ar:"..."}`).

## 9. How to change prices

Prices are stored once, in IQD, on each product (`priceIQD`, and optional `oldPriceIQD`). USD is calculated automatically using the exchange rate in `BRAND_CONFIG.fxUsdToIqd`:

```js
const BRAND_CONFIG = {
  fxUsdToIqd: 1310, // update this to change the IQD → USD conversion everywhere
  ...
};
```

To reprice a single product, just change its `priceIQD` (and `oldPriceIQD` if it's on sale). Both currencies update automatically.

## 10. Future integration possibilities

The app is intentionally structured so a real backend can be dropped in without rebuilding the UI:

- **Catalog** — replace the static `products` / `categories` arrays with a `fetch()` call to Shopify, WooCommerce, Supabase, Firebase, or a custom REST API. Keep the same object shape (or map to it) and the grid/filters/search keep working unchanged.
- **Cart & Wishlist** — currently held in `state.cart` / `state.wishlist` and mirrored to `localStorage`. These can be swapped for server-side calls (e.g. on `addToCart`, `toggleWishlist`) once accounts exist.
- **Checkout** — the `Checkout` button currently shows a demo toast (`checkout_demo`). This is the single hook point to wire up a real payment/COD flow.
- **Authentication** — the Profile page is a static/guest view today; it's ready to be replaced with real login state.
- **Images/CDN** — swap the placeholder tiles for a real image host (Cloudinary, S3, a CDN, or local files in `assets/images/`) as described above.
- **Admin/analytics** — none are implemented yet by design; the data layer (`products`, `categories`, `translations`, `CONTACT_CONFIG`, `BRAND_CONFIG`) is centralized specifically so an admin panel could edit these same values later.

---

© BASRA STYLE — client‑demo prototype.
