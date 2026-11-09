# Oral Exam – 3D Map 🗺️

> 🔒 Examiner only. ✅ = expected answer · 💡 = hint if stuck · 🚩 = red flag (didn't understand)

---

## Part 1 – Demo (5 min · 20 pts)

Ask the student to open **their deployed Vercel link** (not localhost) and:

1. Register a new account (or log in).
2. Fly to another city with the buttons, tilt/rotate the map.
3. Click on the map to save 2 places → red markers appear.
4. Reload the page → are the markers still there?
5. Log in as a **different** user → do they see the first user's places?

| Check | Points |
|---|---|
| App is live on Vercel and loads | 5 |
| Register / login works | 5 |
| 3D map + saving places works | 5 |
| Places survive reload and are private per user, and student explains why | 5 |

✅ For the last point: *"Places are saved in PostgreSQL on Neon with my `user_id`, and the server only returns places `WHERE user_id = me`."*

---

## Part 2 – Own code (4 min · 20 pts, 10 each)

### Q2.1 – Open `client/src/pages/MapPage.jsx`, point at `map.on('click', ...)`
> "Walk me through what happens, step by step, when I click on the map."

✅
1. `window.prompt` asks for a name; if empty → stop.
2. `e.lngLat` gives the **longitude/latitude** of the click.
3. `api('/places', { method: 'POST', body: { name, lng, lat } })` sends it to the backend.
4. The backend saves it in the database and returns the new place (with `id`).
5. `setPlaces((prev) => [...prev, place])` adds it to the state.
6. Because `places` changed, the `useEffect` with `[places]` runs → removes old markers and draws a marker for every place.

💡 Hint: "What does `setPlaces` trigger? Look at the `useEffect`s below."
🚩 Thinks the marker is saved in the map itself / doesn't know a server is involved.

### Q2.2 – Point at the three `useEffect`s in `MapPage.jsx`
> "Why does the first `useEffect` have `[]` and the last one `[places]`? What does `return () => map.remove()` do?"

✅
- `[]` = run **once**, when the page appears → create the map one time only.
- `[places]` = run **again every time `places` changes** → redraw the markers.
- `return () => map.remove()` is the **cleanup**: when you leave the page, the map is destroyed so it doesn't leak memory / exist twice.
- *(Bonus)* `useRef` keeps the map object between renders **without** causing a re-render.

💡 Hint: "What happens if the array is empty vs. has a variable inside?"

---

## Part 3 – JS fundamentals: solve live (5 min · 20 pts)

> Paper or browser console. Tell them: *"Think out loud. Syntax doesn't need to be perfect."*

```js
const places = [
  { name: 'Brandenburg Gate', city: 'Berlin',  lng: 13.37 },
  { name: 'Cologne Cathedral', city: 'Cologne', lng: 6.95 },
  { name: 'Museum Island',     city: 'Berlin',  lng: 13.40 },
];
```

**(a)** Return an array with **only the names**: `['Brandenburg Gate', 'Cologne Cathedral', 'Museum Island']`
**(b)** Write a function `countInCity(places, city)` that returns how many places are in that city. `countInCity(places, 'Berlin')` → `2`

✅ Solution

```js
// (a)
const names = places.map((p) => p.name);

// (b)
function countInCity(places, city) {
  return places.filter((p) => p.city === city).length;
}

// also fine: a loop
function countInCity(places, city) {
  let count = 0;
  for (const p of places) {
    if (p.city === city) count++;
  }
  return count;
}
```

| | Points |
|---|---|
| (a) correct idea (`map` or loop + `push`) | 8 |
| (b) correct function with parameter + `return`, result 2 | 8 |
| Explains clearly while coding | 4 |

💡 Hint: "`map` changes every item, `filter` keeps some items." (costs ~5 pts)
⭐ Bonus follow-up: "What's the difference between `=` and `===`?" → ✅ `=` assigns a value, `===` compares (value **and** type).

---

## Part 4 – Concepts (4 min · 25 pts)

### Q4.1 – What is an API / REST? (12 pts)
> "Your backend has `GET /api/places`, `POST /api/places` and `DELETE /api/places/:id`. What does each one do, and what do the status codes `201`, `400` and `401` mean?"

✅
- `GET` = read/list my places · `POST` = create a new place · `DELETE /:id` = remove the place with that id.
- An **API** = the URLs the frontend can call; data is sent as **JSON**.
- `201` = created · `400` = bad request (e.g. name missing) · `401` = not logged in / token invalid.
- *(Bonus)* `204` = success, no content (used after delete). `500` = server error.

🚩 Can't name what GET vs POST does.

### Q4.2 – Deployment (13 pts)
> "Your app is on Vercel. What happens on Vercel when someone opens your link, and when they call `/api/places`? Where are your secrets?"

✅
- Frontend: Vercel serves the **built** React files (`npm run build` → `client/dist`).
- `/api/...` is sent (rewrite in `vercel.json`) to `api/index.js` → the Express app runs as a **serverless function**: it starts when a request comes in, then may stop.
- That's why `app.js` has **no `app.listen()`**: Vercel does the listening.
- Secrets (`DATABASE_URL`, `JWT_SECRET`) are in **Vercel → Environment Variables**, not in GitHub (`.env` is in `.gitignore`).
- *(Bonus)* GitHub Actions (CI) builds and tests the app on every push → green check / red cross.

---

## Part 5 – "What if" (2 min · 15 pts)

> "A user wants to **rename** a saved place. What do you need to add in the backend and the frontend?"

✅
- **Backend**: a new route `PATCH /api/places/:id` in `server/src/routes/places.js` → SQL `UPDATE places SET name = $1 WHERE id = $2 AND user_id = $3 RETURNING *`.
- **Frontend**: a button/prompt → `api('/places/' + id, { method: 'PATCH', body: { name } })` → update state with `setPlaces(prev => prev.map(p => p.id === id ? updated : p))`.
- 🌟 Mentions `AND user_id = ...` so users can't rename other people's places.

💡 Hint: "Look at how tasks are updated in the calendar, which HTTP method?"
🚩 Only changes the popup text in the browser (not saved → gone after reload).

**Alternative "what if" (debugging):**
> "After 7 days a user suddenly gets sent back to the login page. Bug or feature?"
✅ Feature: the JWT token has `expiresIn: '7d'`. After that `jwt.verify` fails → `401` → `api.js` deletes the token and fires the `logout` event → login page.

---

## ⭐ Bonus (only if time is left)

- "How does the map look 3D?" → ✅ `pitch: 60` tilts the camera; a `fill-extrusion` layer raises buildings using their `render_height`.
- "Why do we store `lng` and `lat` as numbers and not as text?" → ✅ Correct type for calculation/sorting; `DOUBLE PRECISION` in SQL; `Number(lng)` in the route.
