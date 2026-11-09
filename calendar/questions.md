# AI Calendar – Questions

**1. Demo** – Open your Vercel link, log in, add 2 tasks, show the plan, log out and in again.

**2. Your code** – In `CalendarPage.jsx`: what happens step by step when I click **Add task**?

**3. Your code** – In `routes/tasks.js`: why `WHERE user_id = $1`? Where does `req.userId` come from?

**4. JS problem** – Solve live:

```js
const tasks = [
  { title: 'Gym',    hours: 2, done: true  },
  { title: 'Study',  hours: 3, done: false },
  { title: 'Report', hours: 4, done: false },
];
```
a) Return only the tasks that are not done.
b) How many hours of open work are left in total?

**5. Concept** – Name the 3 parts of your app (frontend, backend, database). How do they talk to each other?

**6. Concept** – When I log in, what does the server give me? How does it know who I am on the next request?

**7. What if** – A user doesn't want to work on Sundays. What would you change?
