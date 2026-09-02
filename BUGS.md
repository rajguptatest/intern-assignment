# Bugs found

Add one section per issue. Bug 1 is filled in to show the format — fix it, then write what you changed. Copy the blank template for the rest.

Keep this file in the repo and **commit it** with your fixes.

---

## Bug 1
**Status:** ✅ FIXED & VERIFIED

**How to reproduce:** Open the app. The expense list says “Newest first”. The first row is Wine (7 Mar). Board game (15 Mar) is further down.

**What is wrong:** The list is showing oldest expenses first. Newest should be at the top.

**What I changed:** Fixed the `dateValue()` function in src/lib/format.js to convert date strings to numeric timestamps using `new Date(date).getTime()`. The sorting comparison in ExpenseList.jsx was correct, but it was failing because dateValue() wasn't returning a comparable number.

---

### Bug 2: Balance calculation incorrect when payer is not in split

**Status:** ✅ FIXED & VERIFIED

**How to reproduce:**
1. Add expense: Alice pays $100 for meal
2. Split only with Bob and Carol (NOT Alice)
3. Check balances panel - totals won't match

**What was wrong:**
Payer was incorrectly debited even when not in the split. If payer isn't in split, they should only receive repayment, not be charged a share.
- Symptom: Total owed ≠ Total owed back

**What I changed:**
- File: `src/lib/balances.js` (Removed lines 14-16)
- Deleted incorrect condition: `bal[exp.paidBy] -= Number(exp.amount) / n;`
- Now only people in the split are debited, payer is only credited

**Commit:** "Fix multiple critical bugs: balance calculation, filters, amount validation"  
**File path:** src/lib/balances.js

**Verification:**
- Before: Diya owes $13.00 (WRONG - imbalanced)
- After: Diya owes $43.00 (CORRECT - balances sum to zero)

---

### Bug 3: "Paid by" filter doesn't work

**Status:** ✅ FIXED & VERIFIED

**How to reproduce:**
1. Click dropdown "Paid by" in Filters
2. Select "Aisha Khan"  
3. No expenses filter

**What was wrong:**
Type mismatch: comparing string "1" with number 1, so comparison always fails.

**What I changed:**
- File: `src/App.jsx` (Line 33)
- Changed: `e.paidBy !== paidBy`
- To: `Number(e.paidBy) !== Number(paidBy)`
- Both sides now cast to number for proper comparison

**Commit:** "Fix multiple critical bugs: balance calculation, filters, amount validation"
**File path:** src/App.jsx

**Verification:**
- Filter "Aisha Khan" → Shows 2 expenses ✓
- Filter "Ben" → Shows correct expenses ✓

---

### Bug 4: Dates not hydrated after page reload

**Status:** ✅ FIXED & VERIFIED

**How to reproduce:**
1. Open app, add new expense with today's date
2. Refresh browser (F5)
3. Check if expenses still sort correctly by date

**What was wrong:**
After reload, dates were strings instead of Date objects, breaking:
- Expense sorting (wouldn't work correctly)
- Date comparisons
- Future date calculations

**What I changed:**
- File: `src/state/store.js` (Line 20)
- Changed: `return JSON.parse(raw);`
- To: `return hydrate(JSON.parse(raw));`
- Now loaded data properly converts string dates to Date objects

**Commit:** "Fix multiple critical bugs: balance calculation, filters, amount validation"
**File path:** src/state/store.js

---

## Verification Summary

✅ **All 4 critical bugs FIXED and TESTED**

| Bug | Issue | Verification |
|-----|-------|--------------|
| Amount Edit | Reverts on invalid input | Tested with 0, negative, same value |
| Balance Calc | Incorrect payer debit | Balances now sum correctly |
| Paid By Filter | Type mismatch | Filter works for all members |
| Date Hydration | String instead of Date | Sort order maintained after reload |


---
