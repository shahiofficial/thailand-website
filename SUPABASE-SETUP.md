# Reviews & Testimonials — Supabase setup (one-time, ~5 minutes)

The reviews pages are already built. They show **sample data** until you connect Supabase.
Do these 4 steps once to make customer reviews live.

Your project: `https://btulhnrguotqjkhyeazk.supabase.co`

---

## 1. Create the `reviews` table

Supabase dashboard → **SQL Editor** → paste and run:

```sql
create table if not exists public.reviews (
  id          uuid primary key default gen_random_uuid(),
  name        text not null,
  place       text,
  rating      int  not null check (rating between 1 and 5),
  text        text not null,
  photos      jsonb default '[]'::jsonb,
  approved    boolean not null default false,
  created_at  timestamptz not null default now()
);
```

## 2. Turn on Row Level Security + policies

This is what keeps it safe: the public can **submit** and can **read only approved** reviews. Nobody can edit, delete, or read pending ones.

```sql
alter table public.reviews enable row level security;

-- anyone may submit a review (it starts unapproved)
create policy "public can insert" on public.reviews
  for insert to anon with check (approved = false);

-- anyone may read ONLY approved reviews
create policy "public can read approved" on public.reviews
  for select to anon using (approved = true);
```

## 3. Create the photo storage bucket

Dashboard → **Storage** → **New bucket** → name it exactly `reviews` → set **Public** → create.
Then allow public uploads into it (SQL Editor):

```sql
create policy "public can upload review photos" on storage.objects
  for insert to anon with check (bucket_id = 'reviews');

create policy "public can read review photos" on storage.objects
  for select to anon using (bucket_id = 'reviews');
```

## 4. Paste your anon key into the two pages

Dashboard → **Project Settings → API** → copy the **anon / public** key (a long `eyJ...` string — safe to put in the website; RLS protects everything).

Paste it into `SUPABASE_ANON_KEY` at the top of the `<script>` in **both** files:
- `reviews/index.html`
- `review-submit/index.html`

That's it — redeploy and it's live.

---

## How moderation works (important)

Every submitted review is saved with **`approved = false`** and will **not** appear on the site.
To publish one: dashboard → **Table Editor → reviews** → tick `approved` = true for that row. It appears instantly.

*(If you'd rather approve from your phone without opening Supabase, I can build a small password-protected admin page — just ask.)*

## Video testimonials

Open `reviews/index.html` and edit the `VIDEOS = [ ... ]` list near the top of the script:
- **YouTube (recommended):** upload each clip as an *unlisted* YouTube video and put its ID (the part after `watch?v=`) as `src`, with `type:'youtube'`. Free, streams great, plays with sound.
- **Self-hosted MP4:** upload the file to a public Supabase bucket and put its URL as `src`, with `type:'mp4'`.
Add a `poster` image URL for a nice thumbnail (optional).

## The link to send happy customers

`https://thailand.kairalitrails.com/review-submit`

They open it, add photos, write a review, submit → it lands in your Supabase as pending → you approve → it shows on `/reviews`.
