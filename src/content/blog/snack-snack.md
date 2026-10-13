---
title: "How Building a Snack Database Taught Me SQL (...And More!)"
description: "How I built an active database to track the world's snacks, and what I learned about schema design and PostgreSQL along the way."
pubDate: 2026-09-20
tags: ["postgresql", "supabase", "web-dev", "databases"]
---

Ever since I was a kid, I’ve been a massive snack lover. Chips, cookies, snack cakes—you name it, I wanted to try it. For a while, I could've just kept my ratings and thoughts tucked away in a private notepad, but I started wondering: what if I shared this with other people? What if other snack heads wanted to log their favorites too? 

To make that happen, I needed a real system behind it. Here is how I brought the idea to life and what I learned while putting the infrastructure together.

---

## Infrastructure Overview

I wanted a setup that felt fast, lightweight, and didn't drown me in server maintenance. The backend breaks down into four core pieces:

1. **Supabase & PostgreSQL**: The backbone of the project. I used Supabase's free tier to manage the relational tables, run queries, and handle authentication.
2. **Storage Optimization with Sharp**: High-res snack photos look great, but raw camera uploads can destroy bandwidth and eat through storage limits. Running uploads through `sharp` cuts image sizes by ~80% before storage, keeping the site fast and saving egress costs.
3. **Static Delivery & Client Layer**: Built with plain Vanilla JavaScript, HTML, and CSS. Skipping bulky JavaScript frameworks keeps the bundle tiny, loads pages instantly, and lets the browser do what it does best.
4. **PostgREST API Layer**: Instead of writing a full boilerplate backend in Node or Python just to handle basic CRUD, Supabase automatically spins up secure REST endpoints based directly on my database schema.

---

### Diving into the Data Layer: PostgreSQL & Schema Design

When I first started sketching this out, the easiest path seemed to be dumping everything into a single flat file or a giant JSON document. But as soon as you think about how people actually interact with snacks, you realize the data is completely relational:

* A **Snack** belongs to a specific **Brand** and fits into multiple **Categories** (sweet, savory, spicy, crunchy).
* A single snack can collect dozens or hundreds of **Reviews** and **Ratings**.
* Every single review has to tie back to a verified **User Profile**.

Going with PostgreSQL pushed me to think about proper relational design right out of the gate. Setting up primary keys, foreign keys, and strict data types saved me countless headaches. Instead of writing messy frontend validation to make sure an orphaned review didn't slip through, the database engine enforces data integrity automatically.

---

### The Game Changer: Row Level Security (RLS)

Because I wasn't running a traditional custom server sitting between the client and the database, I immediately ran into a big question: *How do I stop random visitors from wiping out the catalog or editing someone else's review?*

That’s where **Row Level Security (RLS)** blew my mind.

Instead of writing custom API middleware to check authorization tokens, RLS pushes those security rules directly down to the database engine:

* **Public Read Access**: Anyone browsing the catalog—logged in or not—can run `SELECT` queries to see snacks, categories, and approved reviews.
* **Locked-Down User Submissions**: When an authenticated user submits a review, a simple policy ensures the user ID matches the token attached to the request:

```sql
CREATE POLICY "Users can only create their own reviews"
ON reviews FOR INSERT
TO authenticated
WITH CHECK (auth.uid() = user_id);

