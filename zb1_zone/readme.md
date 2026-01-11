# Quiz Result Saver Demo (store answers and show results)

> https://pyasantos.github.io/WDProjStrontiumSalvadorSantos/   
> https://github.com/PYASantos/WDProjStrontiumSalvadorSantos

**What you'll learn**: saving a student’s answers as an object, computing a score, persisting the result, and showing a “Your last result” card on load. This echoes the quiz/result vibe from the ZB1 Zone site.

### Quiz Result Saver Demo — 3–5 Minute Challenges

### 

**Task A — Save Timestamp Friendly Label**

-   **Objective:** When saving a result, store and display a human‑readable timestamp (e.g., “Saved 10:24 AM”).
    
-   **Steps:** add `savedAt: Date.now()` to the saved object; in `renderSavedResult()` use `new Date(savedAt).toLocaleTimeString()`.
    
-   **Success criteria:** The result card shows the readable time and persists after reload.
    
-   **Hint:** Use `toLocaleString()` or `toLocaleTimeString()` for quick formatting.
    

**Task B — Save Only Score Instead of Answers**

-   **Objective:** Change the saved object so it stores only `{ name, score, savedAt }` instead of full answers.
    
-   **Steps:** compute `score`, build the smaller object, `saveResult(obj)`. Update `renderSavedResult()` to expect the smaller shape.
    
-   **Success criteria:** Saved result still displays name and score; answers are not stored in localStorage.
    
-   **Hint:** Inspect localStorage in DevTools to confirm only the compact object is present.
    

**Task C — Show “Last Score” Badge on Form**

-   **Objective:** Add a small badge near the Submit button showing the last saved score (if any).
    
-   **Steps:** read `loadResult()` on load, set badge text like `Last: 2/3`. Update badge after saving.
    
-   **Success criteria:** Badge updates immediately after save and remains after reload.
    
-   **Hint:** Reuse `renderSavedResult()` logic to populate the badge.

---


<details>

<summary> <b>Break glass in case of emergency!! (Solutions) </b></summary>

###   

###  Quiz Result Saver Demo

### 

**Task A — Save Timestamp Friendly Label** **Objective:** Save and display a readable timestamp. **Paste this into the save logic before** `saveResult(resultObj)`**:**

js

```
resultObj.savedAt = Date.now();
saveResult(resultObj);
```

**Paste this into** `renderSavedResult()` **to show time:**

js

```
<p>Saved: ${new Date(saved.savedAt).toLocaleString()}</p>
```

**Success criteria:** Result card shows human‑readable saved time and persists. **Hint:** Use `toLocaleTimeString()` for time only.

**Task B — Save Only Score Instead of Answers** **Objective:** Store only `{ name, score, savedAt }`. **Replace the save code with:**

js

```
const score = computeScore(answers);
const compact = { name: answers.name, score, savedAt: Date.now() };
localStorage.setItem(QUIZ_KEY, JSON.stringify(compact));
```

**Update** `renderSavedResult()` **to read** `saved.score` **and** `saved.name`**.** **Success criteria:** localStorage contains only the compact object; UI shows name and score. **Hint:** Inspect Application → localStorage to confirm.

**Task C — Show Last Score Badge on Form** **Objective:** Add a small badge showing last saved score. **Add this HTML near the buttons:**

html

```
<span id="lastBadge" class="tag is-info">Last: —</span>
```

**Paste this JS to update the badge after load/save:**

js

```
const saved = loadResult();
document.getElementById('lastBadge').textContent = saved ? `Last: ${saved.score}/3` : 'Last: —';
```

**Success criteria:** Badge updates immediately after save and remains after reload. **Hint:** Call the badge update inside `renderSavedResult()`.

</details>