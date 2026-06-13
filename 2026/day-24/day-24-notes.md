# Day 24 of #90DaysOfDevOps – Mastering Advanced Git: Merge, Rebase, Stash & Cherry Pick

## Introduction

After learning Git fundamentals, repositories, commits, branches, and GitHub workflows, the next step was understanding how developers manage changes across multiple branches in real-world projects.

Today's focus was on some of the most important Git concepts:

* Merge
* Rebase
* Squash Merge
* Stash
* Cherry Pick

These commands are used daily in software development and DevOps teams to keep repositories organized and collaboration efficient.

---

# Understanding Git Merge

Git Merge combines changes from one branch into another.

I started by creating a feature branch:

```bash
git switch -c feature-login
```

Made a few commits:

```bash
git add .
git commit -m "Add login page"

git commit -m "Add login validation"
```

Then switched back to main:

```bash
git switch main
```

Merged the branch:

```bash
git merge feature-login
```

---

## Fast-Forward Merge

When no new commits exist on the target branch, Git simply moves the branch pointer forward.

Example:

```text
main
  |
  A --- B --- C (feature-login)
```

After merge:

```text
main
         |
A --- B --- C
```

No extra merge commit is created.

This is called a **Fast-Forward Merge**.

---

# Merge Commit

Next, I created another feature branch:

```bash
git switch -c feature-signup
```

Made commits on the feature branch.

At the same time, I also created a commit on main.

Now both branches had diverged.

When I merged:

```bash
git merge feature-signup
```

Git created a special merge commit.

History looked like:

```text
A --- B --- C -------- M
      \              /
       D --- E --- F
```

This preserves the complete branch history.

---

# Understanding Merge Conflicts

A merge conflict occurs when Git cannot automatically decide which change should be kept.

To create one intentionally:

1. Edit the same line in main.
2. Edit the same line differently in a feature branch.
3. Merge both branches.

Git displayed:

```text
CONFLICT (content)
```

Inside the file:

```text
<<<<<<< HEAD
Main branch content
=======
Feature branch content
>>>>>>> feature-branch
```

After manually resolving the conflict:

```bash
git add .
git commit
```

the merge completed successfully.

---

# Understanding Git Rebase

Rebase is another way to combine branch history.

I created a new branch:

```bash
git switch -c feature-dashboard
```

Added several commits.

Meanwhile, main received new commits.

Instead of merging:

```bash
git rebase main
```

Git replayed my feature branch commits on top of the latest main branch.

---

## How Rebase Changes History

Before:

```text
A --- B --- C (main)
      \
       D --- E --- F
```

After:

```text
A --- B --- C --- D' --- E' --- F'
```

Notice the commit IDs change.

Git creates new versions of those commits.

---

## Merge vs Rebase

### Merge

Pros:

* Preserves complete history
* Safer for shared branches

Cons:

* Creates additional merge commits

### Rebase

Pros:

* Clean linear history
* Easier to read logs

Cons:

* Rewrites commit history

A key rule:

> Never rebase commits that have already been pushed and shared with other developers.

---

# Squash Merge

Sometimes feature branches contain many small commits:

```text
Fix typo
Update formatting
Add comments
Refactor variable
```

Instead of keeping every commit:

```bash
git merge --squash feature-profile
```

Git combines them into one commit.

Result:

```text
Add Profile Feature
```

rather than:

```text
Fix typo
Update formatting
Add comments
Refactor variable
```

---

## Why Use Squash Merge?

Benefits:

* Cleaner history
* Easier code reviews
* One feature = one commit

Trade-off:

* Individual commit history is lost

---

# Git Stash

One of the most practical Git features I learned today was Stash.

Imagine working on a feature:

```bash
vim app.py
```

You have uncommitted changes but need to switch branches immediately.

Instead of committing unfinished work:

```bash
git stash
```

Git stores changes temporarily.

Now I can switch branches:

```bash
git switch main
```

and work normally.

---

## Viewing Stashes

```bash
git stash list
```

Example:

```text
stash@{0}
stash@{1}
stash@{2}
```

---

## Applying Stashed Work

Restore latest stash:

```bash
git stash pop
```

Apply without removing:

```bash
git stash apply
```

Difference:

* `pop` = apply + remove
* `apply` = apply only

---

# Cherry Pick

Cherry Pick allows selecting a specific commit from another branch.

I created:

```bash
git switch -c feature-hotfix
```

Added three commits:

```text
A
B
C
```

Suppose only commit B is needed.

First find the commit hash:

```bash
git log --oneline
```

Then:

```bash
git cherry-pick <commit-id>
```

Only that specific commit is copied.

Result:

```text
main
 |
 B
```

without merging the entire branch.

---

## Real-World Use Case

Cherry Pick is useful when:

* A production hotfix is needed immediately
* Only one commit from a feature branch is required
* Emergency fixes must be moved between release branches

---

# Commands Learned Today

## Merge

```bash
git merge branch-name
```

## Rebase

```bash
git rebase main
```

## Squash Merge

```bash
git merge --squash branch-name
```

## Stash

```bash
git stash

git stash list

git stash pop

git stash apply
```

## Cherry Pick

```bash
git cherry-pick commit-id
```

## Visualizing History

```bash
git log --oneline --graph --all
```

---

# Key Learnings

### 1. Merge Preserves History

Merging combines branches while maintaining branch structure.

### 2. Rebase Creates Cleaner History

Rebasing rewrites commits to create a straight timeline.

### 3. Stash Helps During Context Switching

Temporary work can be safely stored without creating unnecessary commits.

### 4. Cherry Pick Provides Precision

Specific commits can be moved between branches without merging everything.

### 5. Git History Matters

Understanding commit history is just as important as creating commits.

---

# Conclusion

Day 24 was one of the most practical Git learning sessions so far.

I learned how branches come together using Merge and Rebase, how to temporarily save unfinished work with Stash, and how to selectively move changes using Cherry Pick.

These workflows are widely used in professional development teams and form the foundation for advanced collaboration in Git.

Every new Git concept makes version control feel less intimidating and more powerful.

Day 24 completed. 🚀
