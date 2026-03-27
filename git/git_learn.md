# 🎥 Core Reference

[Git Will Finally Make Sense After This](https://www.youtube.com/watch?v=Ala6PHlYjmw&utm_source=chatgpt.com)

---

# 🧠 Git Second Brain

## 🧩 0. Mental Model (MOST IMPORTANT)

* Git = **graph of snapshots**
* You are just:

  * moving pointers
  * creating nodes
  * switching views

👉 If confused → ask:
**“What is HEAD pointing to?”**

---

## 📦 1. Git Internals (Real Truth)

* Git = **content-addressable DB** ([YouTube][1])
* Everything stored as objects:

  * **Blob** → file
  * **Tree** → folder
  * **Commit** → snapshot + metadata

👉 Key idea:

* Same content = same hash → no duplication

---

## 📸 2. Commit = Snapshot (Not diff)

* Full repo snapshot at that time

### Under the hood:

1. Files → blobs (hash)
2. Folder → tree (points to blobs)
3. Commit:

   * points to tree
   * points to parent(s)

👉 So commit =
**state + history link**

---

## 🌳 3. DAG (Graph)

* Commits linked via **parent pointers**
* Forms:

  * Directed ✔
  * Acyclic ✔

```text
c0 ← c1 ← c2 ← c3
        ↑
       c4 ← c5
```

👉 Merge = multiple parents
👉 No cycles = no time travel loops

---

## 🌿 4. Branch = Pointer

* Branch = **just a label to a commit**

```text
main → c3
feature → c5
```

### Why?

* Cheap (no copy)
* Moves automatically on commit

👉 Creating branch =
**just creating a pointer**

---

## 🎯 5. HEAD = Your View

* HEAD = **where you currently are**

### Case 1: On branch

```text
HEAD → main → c3
```

### Case 2: Detached

```text
HEAD → c2
```

👉 Detached = no branch tracking
👉 Think: **camera pointing at commit**

---

## 📥 6. Staging Area (Index)

* Middle layer before commit

```text
Working Dir → Staging → Commit
```

### Underhood:

* `git add` → updates index (snapshot)
* `git commit`:

  * index → tree
  * create commit

👉 Index =
**“what exactly will go into next snapshot”**

---

## 🔄 7. Checkout

* Moves **HEAD**
* Updates working directory

### Cases:

* Branch → HEAD follows branch
* Commit → detached HEAD

👉 Key:

* **Does NOT change history**
* Only changes **view**

---

## 🧨 8. Reset (Pointer Movement)

Moves **branch pointer**

### 🔹 `--soft`

* Move branch
* Keep staging + working

### 🔹 `--mixed` (default)

* Move branch
* Clear staging
* Keep working

### 🔹 `--hard`

* Move branch
* Reset staging
* Reset working

👉 Think:

* reset = **rewrite pointer + optionally files**

---

## 🔁 9. Revert (Safe Undo)

* Creates **new commit**
* Opposite of previous commit

👉 Used when:

* history is shared

---

## 🔀 10. Rebase (Clean History)

### What:

* Replays commits on new base

```text
Before:
main:    c1 ← c2
feature:      c3 ← c4

After:
main:    c1 ← c2 ← c3' ← c4'
```

### Why:

* Linear history
* Cleaner logs

### When:

✔ local branch
✔ before merge

### Avoid:

❌ shared branches

👉 Rule:
**Rebase = rewrite history**

---

## 🧯 11. Reflog (Undo Anything)

* Logs every HEAD movement

```bash
git reflog
```

👉 Even after:

* reset
* rebase
* deleted commits

You can recover:

```bash
git checkout <hash>
```

👉 Think:
**Git never forgets (temporarily)**

---

# 🔥 Ultra Short Summary

* Commit = snapshot
* Branch = pointer
* HEAD = current view
* Git = graph

---

# 🧠 Best Intuition from Video

* Git is NOT file-based
* It is **graph navigation system** ([YouTube][1])

---
