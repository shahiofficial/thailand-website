# Thailand Travel Brochure — Kairali Trails

Static brochure site cloned from the Vietnam build. Same "Island Rush" design system and features.

## Structure
- `index.html` — home / listing page (one card per package; grid is empty until packages are added)
- `<package-slug>/index.html` — one folder per package, folder name = URL slug = Supabase folder name
- `assets/` — `kt-dark.png` (nav/light) and `kt-white.png` (cover/footer) logos
- `vercel.json` — clean URLs

## Before going live — one find & replace
Every image URL points at a placeholder Supabase project. Replace `YOURPROJECT` with your
real Thailand project ref everywhere (it appears in `index.html` and in each package page),
and create a public Storage bucket named `thailand`.

Base URL pattern:
`https://<YOURPROJECT>.supabase.co/storage/v1/object/public/thailand/<slug>/<file>`

## Deploy
Download zip → unzip → GitHub "Add file → Upload files" → drag contents in → Commit →
Vercel rebuilds (~30s) → check in an incognito window.
