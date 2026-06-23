# Thailand Travel Brochure — Kairali Trails

Static brochure site cloned from the Vietnam build. Same "Island Rush" design system and features.

## Structure
- `index.html` — home / listing page (one card per package; grid is empty until packages are added)
- `<package-slug>/index.html` — one folder per package, folder name = URL slug = Supabase folder name
- `assets/` — `kt-dark.png` (nav/light) and `kt-white.png` (cover/footer) logos
- `vercel.json` — clean URLs

## Before going live — one find & replace
Every image URL points at a placeholder Supabase project. Replace `btulhnrguotqjkhyeazk` with your
real Thailand project ref everywhere (it appears in `index.html` and in each package page),
and create a public Storage bucket named `thailand`.

Base URL pattern:
`https://<btulhnrguotqjkhyeazk>.supabase.co/storage/v1/object/public/thailand/<slug>/<file>`

## Deploy
Download zip → unzip → GitHub "Add file → Upload files" → drag contents in → Commit →
Vercel rebuilds (~30s) → check in an incognito window.

## Day photos (auto-detect)
Each day shows however many photos you upload, side by side in a 3-wide grid (3 = one row, 6 = two rows, 9 = three rows). Just name them in sequence and the page shows them all:
`d1-1.jpg, d1-2.jpg, d1-3.jpg, d1-4.jpg ...` then `d2-1.jpg ...`
Tap any photo to open it large and swipe/arrow through that day's set. No code changes needed when counts change.
