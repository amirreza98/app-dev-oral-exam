# 3D Map – Answers

**1. Demo** – Places stay after reload because they're saved in PostgreSQL (Neon) with the user's id.

**2.** Prompt for a name → `e.lngLat` gives coordinates → `api('/places', POST)` saves it → `setPlaces` → `useEffect([places])` draws the markers.

**3.** `[]` = run once (create the map). `[places]` = run every time places change (redraw markers).

**4.**
```js
const names = places.map((p) => p.name);
```
A `for` loop with `push` is fine too.

**5.**
```js
function isBerlin(place) {
  return place.city === 'Berlin';
}
```

**6.** GET = list, POST = create, DELETE = remove. `201` = created, `401` = not logged in.

**7.** Vercel serves the built React files; `/api` runs Express as a serverless function. Secrets are in Vercel Environment Variables, not in GitHub.

**8.** Backend: `PATCH /api/places/:id` with SQL `UPDATE`. Frontend: call it, then update `places` state.
