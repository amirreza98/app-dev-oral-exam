# Oral Exam – AI Calendar 📅

> 🔒 Examiner only. ✅ = expected answer · 💡 = hint if stuck · 🚩 = red flag (didn't understand)

---

## Part 1 – Demo (5 min · 20 pts)

Ask the student to open **their deployed Vercel link** (not localhost) and:

1. Register a new account (or log in).
2. Add 2 tasks with different deadlines, hours and priorities.
3. Show the calendar: where are the work sessions? where are the deadlines?
4. Mark one task as done → what changes in the plan?
5. Log out and log in again → are the tasks still there?

| Check | Points |
|---|---|
| App is live on Vercel and loads | 5 |
| Register / login works | 5 |
| Tasks are created and appear in the calendar plan | 5 |
| Data survives logout → login (database works) and student explains why | 5 |

✅ For the last point: *"The tasks are saved in the PostgreSQL database on Neon, not in the browser, and they are linked to my user id."*

---

## Part 2 – Own code (4 min · 20 pts, 10 each)

### Q2.1 – Open `client/src/pages/CalendarPage.jsx`, point at `addTask`
> "Walk me through what happens, step by step, when I click **Add task**."

✅
1. `e.preventDefault()` stops the form from reloading the page.
2. If the title is empty, it stops.
3. `api('/tasks', { method: 'POST', body: form })` sends the form to the backend.
4. The backend saves it in the database and sends back the new task (with an `id`).
5. `setTasks((prev) => [...prev, task])` adds it to the state → React re-renders the list.
6. `setForm(emptyForm())` clears the form.
7. *(Bonus)* Because `tasks` changed, the `useEffect` runs `fetchPlan()` again → the plan updates.

💡 Hint: "What does `api(...)` talk to? And what does `setTasks` cause?"
🚩 Can't say that the data goes to the server / thinks it is only stored in React.

### Q2.2 – Open `server/src/routes/tasks.js`, point at `router.get('/', ...)`
> "Why do we write `WHERE user_id = $1` here? And where does `req.userId` come from?"

✅
- So every user only sees **their own** tasks.
- `req.userId` is set by the `requireAuth` middleware: it reads the token from the `Authorization: Bearer ...` header, checks it with `jwt.verify`, and puts the user id on the request.
- *(Bonus)* `$1` is a **parameter**, not string concatenation → protects against SQL injection.

💡 Hint: "Look at `app.js`, what runs before `tasksRouter`?"
🚩 "Without it, it doesn't work" with no idea what it filters.

---

## Part 3 – JS fundamentals: solve live (5 min · 20 pts)

> Paper or browser console. Tell them: *"Think out loud. Syntax doesn't need to be perfect."*

```js
const tasks = [
  { title: 'Gym',     hours: 2, done: true  },
  { title: 'Study',   hours: 3, done: false },
  { title: 'Report',  hours: 4, done: false },
];
```

**(a)** Return only the tasks that are **not done**.
**(b)** How many **hours** of open work are left in total? *(expected: 7)*

✅ Solution

```js
// (a)
const open = tasks.filter((t) => !t.done);
// [{ title: 'Study', ... }, { title: 'Report', ... }]

// (b)
const hoursLeft = open.reduce((sum, t) => sum + t.hours, 0); // 7

// also fine: a for...of loop
let total = 0;
for (const t of tasks) {
  if (!t.done) total += t.hours;
}
```

| | Points |
|---|---|
| (a) correct idea (`filter` or loop + `if`) | 8 |
| (b) correct idea (`reduce` or loop + sum), result 7 | 8 |
| Explains clearly while coding | 4 |

💡 Hint: "This exact line is in your `CalendarPage.jsx`, look at the **Stats** part." (costs ~5 pts)
⭐ Bonus follow-up: "What does `filter` return if nothing matches?" → ✅ an empty array `[]`.

---

## Part 4 – Concepts (4 min · 25 pts)

### Q4.1 – Frontend, backend, database (12 pts)
> "Your app has three parts. Name them, what technology each uses, and how they talk to each other."

✅
- **Frontend**: React (+ Vite), runs in the browser, shows the UI.
- **Backend**: Node.js + Express, an API under `/api/...`.
- **Database**: PostgreSQL on Neon.
- Frontend → backend: **HTTP requests with `fetch`** and **JSON** (GET / POST / PATCH / DELETE).
- Backend → database: **SQL queries** with the `pg` pool.

🚩 Thinks React talks directly to the database.

### Q4.2 – Login & security (13 pts)
> "When I log in, what does the server give me, and how does the server know who I am on the next request? Why don't we store the password itself?"

✅
- Server checks the password with `bcrypt.compare` and returns a **JWT token**.
- The frontend saves it in `localStorage` and sends it with every request: `Authorization: Bearer <token>`.
- `requireAuth` checks the token with the **JWT secret**; if fake or expired → `401`.
- Passwords are stored as a **hash** (`bcrypt`), so even if the database leaks, nobody can read the passwords.
- *(Bonus)* Login gives the same error for "wrong email" and "wrong password" so attackers can't find out which emails exist.

---

## Part 5 – "What if" (2 min · 15 pts)

> "A user says: *I don't want to work on Sundays.* Where would you change the code, and roughly how?"

✅ Any reasonable plan:
- Change the **planner** in `server/src/planner.js`, in `ruleBasedPlan`: when building `workDays`, **skip days where `getUTCDay() === 0`** (Sunday).
- Or: add the rule to the **AI prompt** in `llmPlan`.
- *(Great)* Make it a **setting**: a checkbox in the frontend → send `skipSundays` in the `/tasks/plan` body → use it in the planner.

💡 Hint: "Which file decides on which days you work?"
🚩 Wants to hide Sundays in the calendar UI only (the plan would still book hours there).

**Alternative "what if" (if you want a debugging one):**
> "The app works on your laptop but on Vercel every request says *Server is not configured: DATABASE_URL missing*. Why?"
✅ `.env` is not uploaded to Vercel (it's in `.gitignore`). You must add `DATABASE_URL` and `JWT_SECRET` in **Vercel → Settings → Environment Variables** and redeploy.

---

## ⭐ Bonus (only if time is left)

- "What is the difference between the **rule-based** plan and the **AI** plan? Why do we fall back to rules?" → ✅ Rules: always works, free, predictable. AI: needs an API key, costs money, slower, can fail/return bad JSON → `try/catch` falls back to rules.
- "Why does the planner finish one day **before** the deadline?" → ✅ Buffer day for procrastinators / unexpected problems.
