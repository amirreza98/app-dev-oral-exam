# AI Calendar – Answers

**1. Demo** – Tasks are still there after logout because they're saved in PostgreSQL (Neon) with the user's id.

**2.** `preventDefault` → `api('/tasks', POST)` sends the form → server saves it, returns the task → `setTasks` adds it → React re-renders → plan updates.

**3.** So each user sees only their own tasks. `requireAuth` reads the token from the header, verifies it, sets `req.userId`.

**4.**
```js
const open = tasks.filter((t) => !t.done);
const hours = open.reduce((sum, t) => sum + t.hours, 0); // 7
```
A `for` loop is fine too.

**5.** React (browser) → `fetch` + JSON → Express API → SQL → PostgreSQL.

**6.** A JWT token. Saved in `localStorage`, sent as `Authorization: Bearer <token>`, checked by `requireAuth`.

**7.** In `planner.js`, skip Sundays when building `workDays` (or add the rule to the AI prompt).
