# Krem Chympe — Fixed Site

## What was broken, and what I changed

### 1. Double-charging / wrong totals in the booking form
For camping bookings, the form showed **two separate food selectors** listing the
same meal items ("Meal Options" and a second "Food: Veg / Non-Veg Split" panel).
Picking food in both added it to the total twice. I removed the duplicate panel and
simplified the price calculation so every selected item — package, guide, vehicle,
meals, camping gear, entry, parking — is counted exactly once, matching exactly what
the "Total Calculator" breakdown shows the visitor line-by-line. I tested the new
calculation logic against several booking scenarios to confirm the totals are correct.

### 2. The overlapping/garbled text you circled in the booking form
This was caused by the pencil-icon "Content Editor." It remembered *where an edited
element sat in the page* (its position among its siblings) rather than *which*
element it actually was. Because different pages (Home vs. Booking, etc.) have
different layouts, the same position can point at a totally different element on
another page — so an edit made on the Home page could get smeared onto an unrelated
element on the Booking page later. That's exactly what you circled.

Fixed by having the editor double-check, before applying any saved edit, that the
element still matches what was actually edited (its tag + style signature). If it
doesn't match, the editor now safely skips it instead of overwriting the wrong
element. Any old, unsafe edits already saved in your browser/database are
automatically cleared on first load so they can't keep causing this.

### 3. Save button not saving images for visitors
The editor's cloud-save step was failing silently — errors were caught and discarded,
so it could *look* saved to you while never actually reaching the database visitors
load from. It now shows a clear on-screen status ("Saving…", "Saved — visible to
every visitor", or a specific error) every time you save, so you'll always know
whether it worked.

## Files
- `index.html` — page shell, loads the other files
- `config.js` — your Firebase Realtime Database URL (only thing you'll usually touch here)
- `styles.css` — all styling
- `app.js` — the booking site itself (compiled)
- `editor.js` — the pencil-icon Content Editor (text/image tap-to-edit tool)
- `safelinks.js` — makes outbound links open safely in a new tab

Splitting these out (instead of one giant file) makes the repo easier to review and
edit on GitHub — you can now open `app.js` or `editor.js` directly instead of
scrolling through one huge file.

## Deploy to GitHub Pages
1. Create a new GitHub repository (or use your existing `DAVID` repo).
2. Upload **all files in this folder** to the repo root (keep the filenames as-is —
   `index.html`, `config.js`, `styles.css`, `app.js`, `editor.js`, `safelinks.js`).
3. Go to **Settings → Pages → Deploy from a branch**, pick `main` / `root`, save.
4. Your site will be live at the same GitHub Pages URL as before within a minute or two.

No build step, no backend — same as before, just split into normal files.

## One thing worth knowing
Your `KC_FIREBASE_URL` (in `config.js`) is what lets admin edits — prices, images,
text — show up for every visitor instead of just your own device. If it's ever unset
or the database is deleted, edits will only be saved locally on your phone, and the
Content Editor will now clearly tell you that when you hit Save.
