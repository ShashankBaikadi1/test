# Usecase 1 : push existing code in github.
cd my-project
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO.git
git push -u origin main

# Uscase 2 : Push existing code in github not to main but to different branch.
If you’d like to push **your existing code to a brand-new branch** (without touching `main`), here’s the clean workflow:

---

### 1️⃣ Make sure your repo is set up and committed

If you haven’t already:

```bash
git init                   # only if not already initialized
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<username>/<repo>.git   # if not already added
```

---

### 2️⃣ Create and switch to a new branch

Pick a branch name—say `feature/start`:

```bash
git checkout -b feature/start
```

*This creates the branch and moves you onto it in one step.*

---

### 3️⃣ Push that branch to GitHub

```bash
git push -u origin feature/start
```

* `-u` sets `origin/feature/start` as the default upstream so later you can just run `git push` or `git pull` without extra arguments.

---


# Usecase 3 : if u want to make a baby branch from existing branch 
If you already have a branch called **`feature/start`** and you want to create a **new branch based on it**, here’s the typical workflow:

---

### 1️⃣ Make sure you have the latest `feature/start`

If the branch is on GitHub, fetch and check it out locally:

```bash
git fetch origin
git checkout feature/start
git pull origin feature/start   # optional but ensures you’re up to date
```

Now your working tree is exactly at the tip of `feature/start`.

---

### 2️⃣ Create and switch to the new branch

Pick a name for the new branch, e.g. `feature/experiment`:

```bash
git checkout -b feature/experiment
```

This creates `feature/experiment` **starting from the current commit of `feature/start`** and checks it out.

---

### 3️⃣ Push the new branch to GitHub

```bash
git push -u origin feature/experiment
```

* `-u` sets the upstream so future `git push`/`git pull` can be run without extra arguments.

---

### Quick Recap

```bash
git checkout feature/start
git pull origin feature/start
git checkout -b feature/experiment
git push -u origin feature/experiment
```

That’s it—`feature/experiment` is now a new branch whose starting point is whatever `feature/start` currently contains.

# Usecase 4 : how to reveert local commited files or changes on certain branch
There are a few **different “revert” situations**, so first decide *what* you need to undo.
Here’s a clear guide for **local changes on a specific branch** (not yet pushed) and what each command actually does.

---

## 1️⃣ Undo **uncommitted** changes

*(files you edited but never committed)*

While on the branch (e.g. `feature/start`):

```bash
git checkout -- <file>
```

*Throw away modifications to a single file.*

To discard *all* unstaged edits:

```bash
git restore .         # modern Git
# or
git checkout -- .     # older Git
```

If you staged files with `git add` but didn’t commit:

```bash
git restore --staged <file>    # unstage but keep the edits
```

---

## 2️⃣ Undo the **most recent commit**, keep changes as uncommitted

*(“I want my code back in the working tree, just not committed”)*

```bash
git reset --soft HEAD~1
```

* HEAD\~1 = one commit before the current HEAD.
* `--soft` keeps your edits staged.

---

## 3️⃣ Undo the last commit **and discard the changes entirely**

*(“That commit was a mistake, and I don’t want the edits either”)*

```bash
git reset --hard HEAD~1
```

⚠️ **Dangerous**: removes the commit *and* wipes the working files.

---

## 4️⃣ Revert one or more specific commits (safe & keeps history)

*(Good if you might have pushed later)*

Find the commit hash:

```bash
git log
```

Then create a new commit that undoes it:

```bash
git revert <commit-hash>
```

You can revert multiple:

```bash
git revert <old-hash>..<new-hash>
```

This is the safest if you’ve already shared the branch.

---

## 5️⃣ Reset branch to match another branch

*(e.g., “Make my branch identical to `origin/main`”)*

```bash
git fetch origin
git checkout my-branch
git reset --hard origin/main
```

---

### Key Points

* **reset** moves the branch pointer (rewrites history).
* **revert** adds a new commit that undoes previous changes (preserves history).
* Use **--hard** only when you’re certain you don’t need those local edits.

---

🔑 **Typical workflow** if you just want to throw away everything local on `feature/start`:

```bash
git checkout feature/start
git fetch origin
git reset --hard origin/feature/start
```

Now your local branch exactly matches the remote branch.

Choose the method based on whether you need to **keep**, **discard**, or **safely undo** the changes.
