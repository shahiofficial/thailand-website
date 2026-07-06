# Reviews & Video testimonials — Supabase setup (one-time, ~7 minutes)

The site is fully built. It shows **sample data** until you connect Supabase.
Project: `https://btulhnrguotqjkhyeazk.supabase.co`

> **Activation is a single edit:** once the tables/buckets exist, paste your anon key **once** into `assets/kt-config.js` and the whole site (home, every package page, the reviews page, and the submission form) goes live together.

---

## 1. Create the tables — SQL Editor
```sql
create table if not exists public.reviews (
  id uuid primary key default gen_random_uuid(),
  name text not null, place text,
  rating int not null check (rating between 1 and 5),
  text text not null,
  photos jsonb default '[]'::jsonb,
  approved boolean not null default false,
  created_at timestamptz not null default now()
);
create table if not exists public.testimonials (
  id uuid primary key default gen_random_uuid(),
  name text not null, place text,
  video_url text not null,
  created_at timestamptz not null default now()
);
```

## 2. Row Level Security (safety layer)
```sql
alter table public.reviews enable row level security;
alter table public.testimonials enable row level security;
create policy "reviews insert" on public.reviews for insert to anon with check (approved = false);
create policy "reviews read approved" on public.reviews for select to anon using (approved = true);
create policy "testimonials read" on public.testimonials for select to anon using (true);
```

## 3. Storage buckets
Storage → New bucket (Public): make **`reviews`** and **`testimonials`**. Then:
```sql
create policy "review photo upload" on storage.objects for insert to anon with check (bucket_id = 'reviews');
create policy "review photo read"   on storage.objects for select to anon using (bucket_id = 'reviews');
create policy "testimonial read"     on storage.objects for select to anon using (bucket_id = 'testimonials');
```

## 4. Flip it on — one edit
Project Settings → API → copy the **anon / public** key. Open **`assets/kt-config.js`**:
```js
window.KT_SB_KEY = 'eyJ....your-anon-key....';
```
Redeploy. The anon key is safe in the site — RLS controls everything.

---

## Add video testimonials
Upload each MP4 to the **`testimonials`** bucket → copy its public URL → add a row in Table Editor → testimonials (name, place, video_url). It shows in the auto-marquee on home + every package page.
Clips autoplay muted & loop; tap the speaker to unmute (one at a time); videos pause when scrolled off-screen.

## Moderate customer reviews
Every submission arrives **approved = false** (hidden). Publish: Table Editor → reviews → tick `approved`.

## Customer link
`https://thailand.kairalitrails.com/review-submit`

## Where things appear
- Home → videos + rating summary + "Read all reviews"
- Every package page → videos + latest reviews (with photos) + "Write a review" / "See all"
- /reviews → full archive
