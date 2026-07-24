# Zayna's Kitchen 🌺

A mobile-first food ordering website for Zayna's Kitchen — a home-cook business in Lagún, Curaçao, serving Kuminda Kriyoyo (local dishes), Pasapalu (snacks), and Refresko (local drinks).

**Live site:** [zaynaskitchen.com](https://zaynaskitchen.com)


---

## What this is

A single-page ordering site plus a separate password-protected admin panel, both built as standalone HTML/CSS/JS files with no build step or framework — just open and deploy. Orders are placed via WhatsApp; menu data, order history, and photos are stored in a real backend (Supabase).

## Structure

```
├── index.html          # Customer-facing ordering site
└── owner/
    └── index.html       # Owner panel (menu, prices, photos, orders, settings)
```

Deploying the whole folder to Netlify (or any static host) gives you:
- `zaynaskitchen.com/` → customer site


## Features

**Customer site**
- Browse menu by category (Kuminda Kriyoyo, Pasapalu, Refresko — fully owner-editable)
- 4 languages: English, Papiamentu, Español, Nederlands — switchable via a dropdown, with dish names/descriptions translatable per language
- Dual currency display (USD / Caribbean Guilder), both entered manually by the owner
- Cart with add/remove/quantity controls, grouped by category
- Sunday-only pickup logic for Kuminda Kriyoyo (with same-day allowed if it's actually Sunday), same-day pickup for other categories
- Bulk-order threshold (15+ Pasapalu requires direct WhatsApp confirmation)
- Tap-to-enlarge photos on dishes and the logo
- Auto-rotating homepage photo slideshow
- Live "Open now / Closed now" badge, with owner override
- "Order again" shortcut (remembers last order in-browser)
- Checkout via WhatsApp deep link, with order also logged to the database

**Owner panel**
- Real login via Supabase Auth (email + password)
- Add/edit/delete/duplicate menu items, with photo upload
- Per-language editing for category names, subtitles, dish names, and descriptions
- Publish/unpublish toggle, 4-item cap enforcement for Kuminda Kriyoyo
- "Sold out today" and "Closed today" overrides (auto-reset next day)
- Manual Open/Closed status override
- Editable homepage tagline and pickup/delivery note
- Recent orders log
- Manual "Save changes" button (nothing writes until you tap it)

## Tech stack

- Plain HTML/CSS/JavaScript — no framework, no build step
- [Supabase](https://supabase.com) — Postgres database (`menu_data`, `orders` tables), Storage (`menu-images` bucket), and Auth
- [Netlify](https://netlify.com) — static hosting
- WhatsApp deep links (`wa.me`) for order submission

## Setup (for a new deployment)

1. Create a Supabase project
2. Run the SQL in this repo's setup notes to create the `menu_data` and `orders` tables, storage bucket policies, and enable Row Level Security
3. Create an owner user under Supabase Authentication → Users
4. Update `SUPABASE_URL` and `SUPABASE_KEY` (the publishable anon key) at the top of both `index.html` and `owner/index.html`
5. Deploy the folder to Netlify (drag-and-drop or connect this repo for auto-deploy)

## Notes

- Both pages are self-contained — no external dependencies beyond Google Fonts and the Supabase REST API
- The Supabase publishable key is not a secret (it's meant to be public), but access to write data is restricted by Row Level Security policies and real user authentication, not by hiding the key
- Menu photos are compressed client-side before upload to keep the site fast
