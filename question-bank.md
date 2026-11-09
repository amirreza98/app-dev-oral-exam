# Question Bank (backup)

> 🔒 Examiner only. Use these to swap a question, or if a student finishes early.

## JS fundamentals – extra live problems

| # | Problem | ✅ Solution |
|---|---|---|
| 1 | `isWeekend(day)` → `true` for `'Saturday'` / `'Sunday'` | `const isWeekend = (day) => day === 'Saturday' \|\| day === 'Sunday';` |
| 2 | Given `[10, 20, 30]`, return the prices **with 19 % tax** | `prices.map((p) => p * 1.19)` → prints `[11.899999999999999, ...]` (floating point!). Nice follow-up: round with `Math.round(p * 1.19 * 100) / 100` or `.toFixed(2)` |
| 3 | Count how often the letter `'a'` appears in a string | `[...str].filter((c) => c === 'a').length` or a `for` loop with a counter |
| 4 | `const user = { name: 'Ali', age: 22 }` → add `email`, then print every key and value | `user.email = 'ali@x.de'; for (const [k, v] of Object.entries(user)) console.log(k, v);` |
| 5 | Find the task with the **earliest deadline** in `[{title, deadline:'2026-12-01'}, ...]` | `[...tasks].sort((a, b) => a.deadline.localeCompare(b.deadline))[0]` or loop and keep the smallest |
| 6 | Given `[3, 7, 1, 9]`, return the **biggest** number without `Math.max` | loop: `let max = arr[0]; for (const n of arr) if (n > max) max = n;` |
| 7 | What is printed? `console.log(typeof [], typeof null, 0.1 + 0.2 === 0.3)` | `object object false` (arrays and `null` are "object"; floating-point rounding) |
| 8 | Difference between `let`, `const`, `var`? | `const` can't be reassigned, `let` can, both block-scoped; `var` is function-scoped (old, avoid) |
| 9 | What does `async`/`await` do? Rewrite `fetch(url).then(r => r.json()).then(console.log)` | `const r = await fetch(url); const data = await r.json(); console.log(data);` inside an `async` function |
| 10 | What does the spread `[...prev, task]` do and why not `prev.push(task)`? | Creates a **new** array. React only re-renders when state is a new object/array, never mutate state |

## React

- **What is state? What is a prop?** → State = data a component owns and can change (`useState`); changing it re-renders. Props = data passed in from the parent, read-only.
- **What is a component?** → A function that returns JSX (a piece of UI); reusable, e.g. `MonthCalendar`.
- **What is JSX?** → HTML-like syntax inside JS; Vite/Babel turns it into JS function calls.
- **Why does a list need `key`?** → So React can tell which item changed/moved and update only that one.
- **Controlled input?** → `value={form.title}` + `onChange` → React state is the single source of truth.

## Backend / Node / Express

- **What is middleware?** → A function that runs before the route (`req, res, next`), e.g. `cors`, `express.json()`, `requireAuth`.
- **What does `express.json()` do?** → Reads the JSON body of the request into `req.body`.
- **What is CORS?** → The browser blocks requests to another origin unless the server allows it; `cors()` allows it.
- **What is `npm install` / `package.json`?** → Lists dependencies + scripts; `npm install` downloads them into `node_modules`.
- **Why `try/catch` around `await`?** → `await` throws when the promise fails; catch it to answer with an error instead of crashing.

## Database

- **What is a primary key?** → Unique id for each row (`id SERIAL PRIMARY KEY`).
- **What is a foreign key?** → `user_id REFERENCES users(id)` links a row to a user; `ON DELETE CASCADE` deletes their tasks when the user is deleted.
- **What is SQL injection and how do you prevent it?** → User input becomes part of the SQL command; prevent with parameters `$1, $2` instead of string concatenation.
- **Why a database and not `localStorage`?** → Shared across devices, survives clearing the browser, server controls access.

## Process / SDLC (Miro board)

- **Name the SDLC phases.** → Planning → Requirements → Design → Development → Testing → Deployment & Maintenance.
- **Where did testing happen in our course?** → `server/test/*.test.js` + GitHub Actions CI on every push.
- **What is Git for? What's a branch?** → Version history / undo / teamwork; a branch is a separate line of work (`step-1-react`, ...).

## Bonus topics (steps 8–10)

- **React vs React Native?** → React Native renders real native mobile components (`View`, `Text`) instead of HTML; same React ideas (state, props, hooks).
- **Why can't the phone call `localhost:3001`?** → `localhost` on the phone is the phone itself; use the laptop's LAN IP or the deployed URL.
