# Fandom Suggestions Demo (suggestions + ratings)

> https://github.com/tay-lor-AI/WDProjStrontiumTancincoYu  
> https://tay-lor-ai.github.io/WDProjStrontiumTancincoYu/home.html



**What you'll learn:** saving user suggestions and ratings as an array in localStorage, rendering a list, adding and removing items, and persisting ratings across reloads.




### Fandom Suggestions Demo — 3–5 Minute Challenges

**Task A — Add Favorite Genre Field**

-   **Objective:** Add a short text field for “Genre” and save/load it with each suggestion.
    
-   **Steps:** add an `<input id="genre">`; when creating a suggestion push `{ name, rating, genre }`; update `renderList` to show `genre`.
    
-   **Success criteria:** New genre appears in the card and persists after reload.
    
-   **Hint:** Update both `loadSuggestions()` and `saveSuggestions()` usage; stringify the whole array as before.
    
-   **Extension (optional):** Show genre next to the fandom name in italics.
    

**Task B — Quick Remove Single Item Button**

-   **Objective:** Add a small “X” button on each card that removes only that suggestion.
    
-   **Steps:** add a button with `data-idx`, attach click listener to splice the array and re-save.
    
-   **Success criteria:** Clicking X removes that card and the change persists after reload.
    
-   **Hint:** Use `localStorage.removeItem(STORAGE_KEY)` only when clearing all; for single items modify the array and `saveSuggestions(arr)`.
    

**Task C — Show Count of Suggestions**

-   **Objective:** Display the number of saved suggestions in the status area.
    
-   **Steps:** after `renderList()` compute `loadSuggestions().length` and set `#status` or a new element.
    
-   **Success criteria:** Count updates when adding/removing and remains correct after reload.
    
-   **Hint:** Call the count update inside `renderList()` so it always reflects current state.

---

<details>

<summary> <b>Break glass in case of emergency!! (Solutions) </b></summary>

###   

### Fandom Suggestions Demo

### 

**Task A — Add Favorite Genre Field** **Objective:** Save a `genre` with each suggestion. **Paste this HTML inside the form area (near fandom input):**


```html
<div class="field">
  <label class="label">Genre</label>
  <div class="control">
    <input id="genreInput" class="input" placeholder="e.g., sci-fi">
  </div>
</div>
```

**Paste this JS into the Add Suggestion handler (where name and rating are read):**



```js
const genre = document.getElementById('genreInput').value.trim();
arr.push({ name, rating, genre, createdAt: Date.now() });
saveSuggestions(arr);
document.getElementById('genreInput').value = '';
```

**Success criteria:** Genre appears on each card and persists after reload. **Hint:** Update `renderList()` to show `s.genre` (e.g., add `<em>${escapeHtml(s.genre)}</em>`).


---

###   

**Task B — Quick Remove Single Item Button** **Objective:** Add an X button to remove only that suggestion. **Paste this snippet into the card HTML inside** `renderList()` **where buttons are created:**


```html
<button data-idx="${idx}" class="button is-small is-danger removeBtn">X</button>
```

**Ensure the existing remove listener uses** `arr.splice(i,1); saveSuggestions(arr); renderList();` **Success criteria:** Clicking X removes that item and change persists. **Hint:** Do not call `localStorage.clear()`; modify the array and re-save.

**Task C — Show Count of Suggestions** **Objective:** Display number of saved suggestions. **Paste this JS at the end of** `renderList()` **to update a status element:**


```js
const count = loadSuggestions().length;
document.getElementById('status').textContent = `Suggestions: ${count}`;
```

**Success criteria:** Count updates after add/remove and after reload. **Hint:** Put a `<p id="status"></p>` in the page if missing.

</details>