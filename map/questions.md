# 3D Map – Questions

**1. Demo** – Open your Vercel link, log in, fly to a city, save 2 places, reload the page.

**2. Your code** – In `MapPage.jsx`: what happens step by step when I click on the map?

**3. Your code** – Why does one `useEffect` have `[]` and another `[places]`?

**4. JS problem** – Solve live:

```js
const places = [
  { name: 'Brandenburg Gate',  city: 'Berlin'  },
  { name: 'Cologne Cathedral', city: 'Cologne' },
  { name: 'Museum Island',     city: 'Berlin'  },
];
```
a) Return an array with only the names.
b) Write `countInCity(places, city)` → `countInCity(places, 'Berlin')` returns `2`.

**5. Concept** – What do `GET`, `POST` and `DELETE /api/places` do? What do `201` and `401` mean?

**6. Concept** – What happens on Vercel when someone opens your link? Where are your secrets?

**7. What if** – A user wants to rename a place. What do you add?
