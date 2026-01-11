# LocalStorage worksheet for grade 9: interactive, step‑by‑step

> **Remember**: localStorage is like a tiny dictionary in the browser. It stores key → value pairs, but only as strings.



## Getting started



**Goal**: Understand what localStorage does and practice saving, loading, and removing data.

**Setup**: Use the provided [index.html](https://github.com/roycan/localstorage-demo/blob/main/index.html) template. Open it in a browser. Keep DevTools open (Console) to observe behavior.

> https://github.com/roycan/localstorage-demo/blob/main/index.html



> **Tip**: Reload the page after saving to see persistence.


## Warm‑up: read and predict

### Find the save logic:

```js
document.getElementById("save").onclick = function() {
  localStorage.setItem("name", document.getElementById("name").value);
  localStorage.setItem("theme", document.getElementById("theme").value);
  document.getElementById("status").textContent = "Data saved! Reload the page to see persistence.";
};
```

**Question**: localStorage works with key/value pairs. What keys are being saved?

**Prediction**: If you type “Alex” and choose “Dark”, what will localStorage.getItem("name") return after reload?


### Find the load logic:

```js
document.addEventListener("DOMContentLoaded", function() {
  const savedName = localStorage.getItem("name");
  const savedTheme = localStorage.getItem("theme");

  if (savedName) document.getElementById("name").value = savedName;
  if (savedTheme) document.getElementById("theme").value = savedTheme;

  if (savedName || savedTheme) {
    document.getElementById("status").textContent = "Loaded saved data from localStorage!";
  }
});
```


**Question**: When does this code run?

**Prediction**: What happens if there’s no saved data?


---


## Exercise 1: add a new field (favorite band/ singer)

Add HTML input:

```js
<div class="field">
  <label class="label">Favorite Band/ Singer</label>
  <div class="control">
    <input id="favoriteBand" class="input" type="text" placeholder="e.g., Itchyworms">
  </div>
</div>
```

Save the new field:

```js
localStorage.setItem("favoriteBand", document.getElementById("favoriteBand").value);
```

Load the new field: 

```js
const savedFavoriteBand = localStorage.getItem("favoriteBand");
if (savedFavoriteColor) document.getElementById("favoriteBand").value = savedFavoriteBand;
```

**Checkpoint**: Type a Band/ Singer, click Save, reload—does it persist?


---


## Exercise 2: use removeItem instead of clear

Replace the Clear button logic:

```js
document.getElementById("clear").onclick = function() {
  localStorage.removeItem("name");
  localStorage.removeItem("theme");
  localStorage.removeItem("favoriteBand");
  document.getElementById("status").textContent = "Removed specific items from localStorage!";
};
```

Reset the UI (optional, but recommended): 

```js
document.getElementById("name").value = "";
document.getElementById("theme").value = "light";
document.getElementById("favoriteBand").value = "";
```


**Checkpoint**: Save data, then click Clear—only the listed keys should be removed. Confirm in DevTools: `localStorage.length`.


---

## Exercise 3: show all saved keys and values

Add a display area:

```html
<div class="box mt-4">
  <h3 class="title is-5">Saved data</h3>
  <pre id="dump"></pre>
</div>
```

Write a function to render localStorage:

```js
function renderDump() {
  const dump = {};
  for (let i = 0; i < localStorage.length; i++) {
    const key = localStorage.key(i);
    dump[key] = localStorage.getItem(key);
  }
  document.getElementById("dump").textContent = JSON.stringify(dump, null, 2);
}
```


<details>
  <summary> <b>Explain JSON.stringify()</b> </summary> 


**TLDR**: localStorage can only save strings. To save a whole object or list, we use JSON.stringify to convert it into a string. Later, we use JSON.parse to turn it back into an object. Think of it like packing your data into a box (stringify) and then unpacking it (parse).


    Converts a JavaScript object into a JSON string. The second parameter is a replacer (null means no filtering), and the third parameter specifies indentation for readability.

* localStorage only stores strings.  
That means if you try to save an object or array directly, it won’t work — it will just save `[object Object]`.
* `JSON.stringify(object)` turns an object/array into a string.  

Example:
```js
const profile = { name: "Alex", theme: "dark" };
const str = JSON.stringify(profile);
console.log(str); // '{"name":"Alex","theme":"dark"}'
localStorage.setItem("profile", str);
```

* `JSON.parse(string)` turns the string back into an object/array.  

Example:

```js
const saved = localStorage.getItem("profile");
const obj = JSON.parse(saved);
console.log(obj.name); // "Alex"
```

* Why this matters:
  * Lets us store multiple values together in one key.
  * Keeps the structure (like arrays of suggestions or quiz results).
  * Makes it easy to reload and use the data later.

</details>




Call it after load/save/clear:

```js
document.addEventListener("DOMContentLoaded", renderDump);
document.getElementById("save").onclick = () => {
  // ...existing save code...
  renderDump();
};
document.getElementById("clear").onclick = () => {
  // ...existing removeItem code...
  renderDump();
};
```

**Checkpoint**: Save some data, and see it appear in the dump area.
 Do you see a JSON view of all saved items?
  Clear data and see it update.

---


## Exercise 4: store an object with JSON

* Add a “profile” object save:

```js
const profile = {
  name: document.getElementById("name").value,
  theme: document.getElementById("theme").value,
  favoriteColor: document.getElementById("favoriteBand").value
};
localStorage.setItem("profile", JSON.stringify(profile));
```

* Load the “profile” object:

```js
const savedProfile = localStorage.getItem("profile");
if (savedProfile) {
  const p = JSON.parse(savedProfile);
  document.getElementById("name").value = p.name || "";
  document.getElementById("theme").value = p.theme || "light";
  document.getElementById("favoriteBand").value = p.favoriteBand || "";
}
```

* **Checkpoint**: Confirm localStorage.getItem("profile") is a JSON string. Reload—does the form populate from the object?


---


## Exercise 5: handle missing or empty values

* Add simple validation before saving:

```js
const nameValue = document.getElementById("name").value.trim();
if (!nameValue) {
  document.getElementById("status").textContent = "Please enter your name before saving.";
  return;
}
localStorage.setItem("name", nameValue);
```

* **Checkpoint**: Try saving with an empty name—does the status message guide you?


---

## Challenge: incognito and quota awareness

* Incognito test: Open the page in incognito/private mode. Save data, close the tab, reopen—does it persist? Discuss why behavior may differ.

* Quota test (optional): Try saving a very large string (e.g., repeat "x" many times) and catch errors:

```js
try {
  localStorage.setItem("big", "x".repeat(5_000_000));
} catch (e) {
  document.getElementById("status").textContent = "Storage limit reached.";
}
```

---



# Reflection and quick quiz

You may search, study, and give short answers in your own words:

1. Definition: What is localStorage used for?

2. Security: Why shouldn’t you store passwords or tokens in localStorage?

3. Scope: What does “scoped to origin” mean?


---

# Demos 

1. https://github.com/roycan/localstorage-demo/tree/main/fandom
2. https://github.com/roycan/localstorage-demo/tree/main/zb1_zone
3. https://github.com/roycan/localstorage-demo/tree/main/albee
   