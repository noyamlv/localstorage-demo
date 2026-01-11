# Local Leaderboard Demo (save name + score entries)

> https://github.com/AlBees-0511/CS3_1stQtrPortfolio.git
> https://albees-0511.github.io/CS3_1stQtrPortfolio/

**What you'll learn**: storing an array of score entries, sorting, limiting leaderboard length, and persisting across reloads. This mirrors a portfolio/leaderboard idea from the AlBees site.



### Local Leaderboard Demo — 3–5 Minute Challenges

### 

**Task A — Limit Name Length**

-   **Objective:** Prevent very long names by truncating to 12 characters before saving.
    
-   **Steps:** before pushing to board do `const short = name.slice(0,12)` and save `short`.
    
-   **Success criteria:** Leaderboard shows truncated names and persists after reload.
    
-   **Hint:** Add `…` when truncating for clarity: `name.length>12 ? name.slice(0,12)+'…' : name`.
    

**Task B — Remove Single Entry by Index**

-   **Objective:** Add a small “Remove” button next to each run to delete that entry only.
    
-   **Steps:** render a button with `data-idx`, on click splice the board array, `saveBoard(board)`, then `renderBoard()`.
    
-   **Success criteria:** Clicking Remove deletes that entry and change persists.
    
-   **Hint:** Re-sort after removal if needed; use `board.splice(idx,1)`.
    

**Task C — Highlight Personal Best**

-   **Objective:** If the current user submits a time better than their previous best, highlight that entry.
    
-   **Steps:** when saving, find existing entries with the same `name`, compare times, mark the best with a `best: true` flag, save and render with a special class.
    
-   **Success criteria:** Personal best entry is visually distinct and persists after reload.
    
-   **Hint:** Use `board.map()` to clear previous `best` flags for that name before setting the new one.


---

<details>

<summary> <b>Break glass in case of emergency!! (Solutions) </b></summary>


###   

### Local Leaderboard Demo

### 

**Task A — Limit Name Length** **Objective:** Truncate names to 12 characters before saving. **Replace name handling in submit handler with:**

js

```
const rawName = document.getElementById('playerName').value.trim() || 'Anonymous';
const name = rawName.length > 12 ? rawName.slice(0,12) + '…' : rawName;
```

**Success criteria:** Leaderboard shows truncated names and persists. **Hint:** Use `…` to indicate truncation.

**Task B — Remove Single Entry by Index** **Objective:** Add a Remove button for each run. **In** `renderBoard()` **include this button in each item HTML:**

html

```
<button data-idx="${i}" class="button is-small is-light removeRun">Remove</button>
```

**Add this listener after rendering:**

js

```
document.querySelectorAll('.removeRun').forEach(btn => {
  btn.addEventListener('click', e => {
    const i = Number(e.target.dataset.idx);
    const board = loadBoard();
    board.splice(i,1);
    saveBoard(board);
    renderBoard();
  });
});
```

**Success criteria:** Clicking Remove deletes that entry and persists. **Hint:** Re-render after saving.

**Task C — Highlight Personal Best** **Objective:** Mark a user’s best run visually. **In submit handler, after pushing and sorting, add:**

js

```
board.forEach(entry => entry.best = false);
const nameEntries = board.filter(e => e.name === name);
if (nameEntries.length) {
  const best = nameEntries.reduce((a,b) => a.time < b.time ? a : b);
  best.best = true;
}
saveBoard(board);
```

**In** `renderBoard()` **add a class when** `entry.best` **is true, e.g.:**

html

```
<div class="item ${entry.best ? 'has-background-success-light' : ''}">
```

**Success criteria:** Personal best is highlighted and persists after reload. **Hint:** Clear previous `best` flags for that name before setting the new one.

</details>