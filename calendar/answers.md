# AI Calendar – Answers

**1. Demo** – Tasks are still there after logout because they're saved in PostgreSQL (Neon) with the user's id.

**2.** `preventDefault` → `api('/tasks', POST)` sends the form → server saves it, returns the task → `setTasks` adds it → React re-renders → plan updates.

**3.** So each user sees only their own tasks. `requireAuth` reads the token from the header, verifies it, sets `req.userId`.

**4.**
```js
const open = tasks.filter((t) => !t.done);
```
A `for` loop with `if` is fine too.

**5.**
```js
console.log(`${task.title} takes ${task.hours} hours`);
```
`task.title + ' takes ' + task.hours + ' hours'` is fine too.

**6.** React (browser) → `fetch` + JSON → Express API → SQL → PostgreSQL.

**7.** A JWT token. Saved in `localStorage`, sent as `Authorization: Bearer <token>`, checked by `requireAuth`.

**8.** In `planner.js`, skip Sundays when building `workDays` (or add the rule to the AI prompt).
