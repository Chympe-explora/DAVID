# Krem Chympe — Updated Site

## What changed in this round

### 1. Removed Firebase — you manage images yourself now
The site no longer relies on Firebase for images. Every image on the site is
now just a file name, listed in one place at the top of `index.html`
(`window.KC_IMAGES = {...}`). Upload your photo into the `images` folder and
point the matching line at its file name — nothing else needs to change.

This zip already includes the site's current photos in `images/`, renamed
clearly (`hero-bg-1.jpg`, `trek-card.jpg`, `gallery-3-camping.jpg`, etc.), so
the site works immediately. Swap any of them out whenever you like.

Two images were never bundled in the first place and still need you to add
them: `images/guide.jpg` (guide photo) and `images/logo.png` (site logo).
Until you add those, the site falls back to a simple icon.

The old `config.js` (Firebase Realtime Database URL) has been removed along
with its `<script>` tag in `index.html`. Admin edits (the Secret Admin
Dashboard's price/text fields) now save to this device only, via
`localStorage` — the same safe fallback the previous version already had, so
nothing is broken, there's just no cloud step anymore.

### 2. Fixed: text you type disappearing / the on-screen keyboard closing
The real cause: two small pieces of the page (the glass "card" wrapper used
everywhere, and the +/− quantity stepper used for meals, camping gear and
people count) were being **recreated from scratch on every single keystroke**.
React saw a "new" component each time and threw away and rebuilt the input
box you were typing into — which is exactly what closes the mobile keyboard
and can make typed text behave oddly.

Fixed by moving those two pieces out so they're created once, not on every
keystroke. Typing in Full Name, WhatsApp Number, Date, and the quantity
steppers should now behave normally and the mobile keyboard should stay open.

### 3. Fixed the pricing calculation
Previously, the flat "Trek" (₹1500) or "Camping" (₹3500) package price was
always added to every booking automatically, on top of whatever add-ons were
picked — that's the overcharge. It's now removed. The total charges only for
what's actually applicable:
- **Guide** — mandatory on every booking (as labeled), always included
- **Vehicle** — only if Rainy or Winter is picked (Free/Walk adds nothing)
- **Meals** — only the specific meals with quantity > 0, each counted once
- **Camping gear** — only for camping bookings, only items with quantity > 0
- **Entry** (per person) and **Parking** — the standard site facility fees

Nothing is double-counted, and nothing is added for an item that wasn't
selected.

### 4. Total Calculator now lists exactly what's selected
The breakdown panel on the Pricing page no longer shows a flat trek/camping
line. It now lists only the items actually chosen — Guide, Vehicle (if any),
each selected meal by name with its quantity, each selected camping item by
name with its quantity, Entry, and Parking — with full price detail for each,
and nothing shown for anything not selected.

## Files
- `index.html` — page shell; **your image file names go here** (`KC_IMAGES`)
- `styles.css` — all styling
- `app.js` — the booking site itself (compiled)
- `editor.js` — the pencil-icon Content Editor (text/image tap-to-edit tool)
- `safelinks.js` — makes outbound links open safely in a new tab
- `images/` — the site's photos; add/replace files here to match `index.html`

## Deploy to GitHub Pages
1. Create a new GitHub repository (or use your existing repo).
2. Upload **everything in this folder** to the repo root, including the
   `images` folder — keep the file names as-is.
3. Go to **Settings → Pages → Deploy from a branch**, pick `main` / `root`,
   save.
4. Your site will be live at the same GitHub Pages URL as before within a
   minute or two.

No build step, no backend, no Firebase — just static files.
