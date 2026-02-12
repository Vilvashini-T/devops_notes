# 🚀 MODULE 5 — Advanced Git & Professional Workflow (In-Depth)
---
# 🔥 1️⃣ Rewriting History (Very Important for Interviews)
Git allows you to modify commit history.
But ⚠️ with great power comes great responsibility.
---
## 🧠 A. `git amend`
Used to modify the **last commit**.
### Example:
You forgot to add one file.
```
git add missing.txt
git commit --amend
```
What happens?
Instead of creating a new commit:
* Git **replaces** the previous commit
* Creates a new commit with new SHA
Interview question:
> Why does SHA change when you amend?
Because:
* SHA is calculated using:
  * Commit message
  * Timestamp
  * Parent commit
  * Content
* Changing anything → new hash
---
## 🧠 B. Interactive Rebase (Very Important)
```
git rebase -i HEAD~3
```
This opens last 3 commits.
You can:
* squash (combine commits)
* reword (edit message)
* drop (delete commit)
* reorder commits
---
### Example:
Before:
```
A - B - C - D (HEAD)
```
Run:
```
git rebase -i HEAD~3
```
You see:
```
pick B
pick C
pick D
```
You can change to:
```
pick B
squash C
squash D
```
Result:
```
A - BCD
```
Now 3 commits → 1 clean commit.
---
### ⚠️ VERY IMPORTANT RULE
Never rebase shared public branches.
Why?
Because:
* Rebase rewrites history
* Other developers' history breaks
Interview question:
> When should you use rebase instead of merge?
Answer:
* To keep linear history
* Before pushing feature branch
* For cleaning commit history
---
# 🔥 2️⃣ Merge vs Rebase (Deep Comparison)
---
## Merge
```
git merge feature
```
Creates merge commit.
History:
```
      B
     /
A ——— C
     \
      D
```
Keeps full history.
---
## Rebase
```
git rebase main
```
Moves your commits on top.
History becomes:
```
A — B — C — D
```
Linear.
---
## Interview Comparison
| Merge                    | Rebase                 |
| ------------------------ | ---------------------- |
| Preserves history        | Rewrites history       |
| Safe for public branches | Dangerous if pushed    |
| Creates merge commit     | No merge commit        |
| Good for team work       | Good for clean history |
---
# 🔥 3️⃣ Git Reset (Most Confusing Topic)
You MUST master this.
There are 3 levels:
---
## 🧠 A. Soft Reset
```
git reset --soft HEAD~1
```
What it does:
* Moves HEAD
* Keeps staging
* Keeps working directory
Meaning:
Commit removed, but files are staged.
---
## 🧠 B. Mixed Reset (default)
```
git reset HEAD~1
```
What it does:
* Moves HEAD
* Clears staging
* Keeps working directory
Files become modified (unstaged).
---
## 🧠 C. Hard Reset (Dangerous)
```
git reset --hard HEAD~1
```
What it does:
* Moves HEAD
* Clears staging
* Deletes working directory changes
Data lost.
---
## 🧠 Visual Understanding
Think in 3 layers:
```
Commit history (HEAD)
↓
Staging area
↓
Working directory
```
Soft → affects only history
Mixed → affects history + staging
Hard → affects all three
---
### Interview Question:
> What is difference between reset and revert?
Answer:
| Reset                        | Revert                 |
| ---------------------------- | ---------------------- |
| Rewrites history             | Creates new commit     |
| Dangerous in shared branches | Safe for public branch |
| Deletes commits              | Adds inverse commit    |
---
# 🔥 4️⃣ Git Revert
Safe undo.
```
git revert <commit-id>
```
It:
* Creates new commit
* Undoes changes of old commit
History remains intact.
Used in:
* Production
* Shared branches
---
# 🔥 5️⃣ Git Cherry-Pick
Used to apply specific commit from one branch to another.
```
git cherry-pick <commit-id>
```
Example:
Feature branch has 5 commits.
You only want 1 bug fix.
Cherry-pick copies that commit.
Interview question:
> When would you use cherry-pick?
Answer:
* Urgent bug fix
* Apply specific patch
* Backport changes
---
# 🔥 6️⃣ Git Stash (Very Important in Real Projects)
Temporarily saves uncommitted changes.
---
## Save changes
```
git stash
```
Your working directory becomes clean.
---
## View stash list
```
git stash list
```
---
## Apply stash
```
git stash apply
```
---
## Apply and remove
```
git stash pop
```
---
## Real Scenario
You're working on feature A.
Suddenly urgent bug fix required.
You:
```
git stash
git checkout main
git checkout -b hotfix
```
After finishing:
```
git checkout featureA
git stash pop
```
Work continues.
---
# 🔥 7️⃣ Git Tags
Used to mark versions.
Example:
```
git tag v1.0
```
Annotated tag:
```
git tag -a v1.0 -m "Version 1 release"
```
Push tags:
```
git push origin v1.0
```
Used for:
* Releases
* Versioning
* Production deployments
---
# 🔥 8️⃣ Git Hooks (Interview Advanced)
Scripts that run automatically.
Examples:
* pre-commit
* pre-push
* post-merge
Used for:
* Running tests
* Checking formatting
* Preventing bad commits
Location:
```
.git/hooks/
```
---
# 🔥 9️⃣ Git Internals (Advanced Interview)
Git is:
* Distributed
* Content-addressable storage
Objects stored in:
```
.git/objects/
```
Types:
* Blob (file content)
* Tree (directory)
* Commit (snapshot metadata)
* Tag
---
# 🔥 🔟 Real Industry Workflow (How Companies Work)
---
## 🧠 Git Flow Model
Branches:
* main (production)
* develop
* feature/*
* release/*
* hotfix/*
---
## 🧠 GitHub Flow (Modern)
Simpler:
* main
* feature branches
* Pull Request
* Merge
Most companies use this now.
---
# 🎯 Important Interview Questions From Module 5
Be ready to answer:
1. Difference between merge and rebase?
2. Explain reset types?
3. What is cherry-pick?
4. When to use revert?
5. How does stash work?
6. What is HEAD?
7. What is detached HEAD?
8. Explain Git object model?
9. How do you recover deleted commit?
10. What happens internally when you commit?
---
# 🧠 BONUS — Recover Lost Commits (Very Important)
If you accidentally reset hard:
```
git reflog
```
Shows history of HEAD movements.
Then:
```
git checkout <commit-id>
```
You can recover.
Interview golden answer ⭐
