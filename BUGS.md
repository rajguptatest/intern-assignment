# Bugs found

Add one section per issue. Bug 1 is filled in to show the format — fix it, then write what you changed. Copy the blank template for the rest.

Keep this file in the repo and **commit it** with your fixes.

---

## Bug 1

**How to reproduce:** Open the app. The expense list says “Newest first”. The first row is Wine (7 Mar). Board game (15 Mar) is further down.

**What is wrong:** The list is showing oldest expenses first. Newest should be at the top.

**What I changed:** Fixed the `dateValue()` function in src/lib/format.js to convert date strings to numeric timestamps using `new Date(date).getTime()`. The sorting comparison in ExpenseList.jsx was correct, but it was failing because dateValue() wasn't returning a comparable number.

---

## Bug 2

**How to reproduce:**

**What is wrong:**

**What I changed:**

---
