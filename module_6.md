# 🚀 MODULE 6 — Professional Git + GitHub + DevOps Workflow (In-Depth)
---
# 🔥 1️⃣ Pull Requests (PR) — Deep Understanding
A Pull Request is NOT just "request to merge".
It is:
* Code review mechanism
* Discussion platform
* Quality gate
* Audit trail
* Integration checkpoint
---
## 🧠 What Happens Internally When You Create PR?
1. You push feature branch
2. GitHub compares:
   ```
   base branch (main)
   vs
   your feature branch
   ```
3. It calculates:
   * Diff
   * Commits ahead/behind
4. Shows:
   * Files changed
   * Line-level diff
   * Conflict detection
---
## 🧠 PR Lifecycle
1. Create branch
2. Push branch
3. Open PR
4. CI runs tests
5. Code review
6. Approvals
7. Merge
In companies, merging without review = 🚫 not allowed.
---
## 🎯 Interview Question:
Why use PR instead of directly merging?
Answer:
* Code quality control
* Peer review
* Automated testing
* Prevent production bugs
* Documentation of decisions
---
# 🔥 2️⃣ Merge Strategies in GitHub
When merging PR, you see options:
---
## 🧠 1. Merge Commit
Creates merge commit.
History:
```
A---B---C
     \   \
      D---E
```
Preserves branch history.
Good for:
* Team collaboration
* Feature isolation
---
## 🧠 2. Squash and Merge
All commits → 1 commit.
Before:
```
D
E
F
```
After:
```
Single commit
```
Good for:
* Clean main branch
* Small features
Most startups use this.
---
## 🧠 3. Rebase and Merge
Rewrites commit history.
Linear history.
Good for:
* Clean git log
* Advanced teams
---
🎯 Interview:
Which merge strategy would you use?
Answer:
* Squash for small feature branches
* Merge commit for complex features
* Rebase if team agrees and understands history rewriting
---
# 🔥 3️⃣ CI/CD (Continuous Integration / Deployment)
This is VERY important for interviews.
---
## 🧠 What is CI?
Every time you push code:
* Automated tests run
* Build runs
* Lint runs
If tests fail → PR blocked
Example tools:
* GitHub Actions
* Jenkins
* GitLab CI
* CircleCI
---
## 🧠 What is CD?
After merge to main:
* App automatically deployed
* To staging / production
---
## 🧠 Example GitHub Actions Flow
When PR created:
* Run:
  * npm install
  * npm test
  * build project
If success → allow merge
---
## 🎯 Interview Question:
Why is CI important?
Answer:
* Detect bugs early
* Prevent broken main branch
* Enforce code quality
* Faster feedback loop
---
# 🔥 4️⃣ Protecting Main Branch (Very Important)
In companies:
main branch is protected.
Settings include:
* Require PR review
* Require status checks (CI pass)
* Require 2 approvals
* No direct push allowed
* No force push
---
## 🎯 Interview:
Why protect main branch?
Answer:
* Prevent accidental push
* Avoid breaking production
* Maintain stability
---
# 🔥 5️⃣ Conflict Resolution at Scale
In small projects:
You manually resolve conflicts.
In big teams:
Conflicts happen daily.
Best practices:
1. Pull latest main before PR:
   ```
   git pull origin main
   ```
2. Rebase frequently:
   ```
   git fetch
   git rebase origin/main
   ```
3. Keep PR small.
---
# 🔥 6️⃣ Semantic Versioning (Very Important)
Version format:
```
MAJOR.MINOR.PATCH
```
Example:
```
1.4.2
```
Meaning:
* MAJOR → Breaking changes
* MINOR → New features (backward compatible)
* PATCH → Bug fixes
---
## 🎯 Interview:
When should you increment MAJOR?
Answer:
When backward compatibility breaks.
---
# 🔥 7️⃣ Git Submodules (Advanced)
Used when:
Project A depends on Project B repository.
Instead of copying code:
You link repository.
Command:
```
git submodule add <repo-url>
```
Used in:
* Large enterprise systems
* Shared libraries
---
# 🔥 8️⃣ Monorepo vs Polyrepo
---
## 🧠 Monorepo
One repo:
* frontend
* backend
* mobile
* shared code
Pros:
* Easy dependency management
* Single versioning
Cons:
* Large repo
* Complex CI
Used by:
Google, Meta
---
## 🧠 Polyrepo
Separate repos:
* frontend repo
* backend repo
* mobile repo
Pros:
* Independent deployment
* Smaller repos
Cons:
* Version sync issues
---
🎯 Interview:
Which is better?
Answer:
Depends on scale, team size, architecture.
---
# 🔥 9️⃣ Git Bisect (Very Powerful Debug Tool)
Used to find commit that introduced bug.
Command:
```
git bisect start
git bisect bad
git bisect good <commit>
```
Git performs binary search on commits.
This is senior-level knowledge.
---
# 🔥 🔟 Detached HEAD (Deep Explanation)
HEAD normally points to branch.
Detached HEAD happens when:
```
git checkout <commit-id>
```
Now:
HEAD points to commit, not branch.
If you commit here:
Commit can be lost unless you create branch.
Fix:
```
git checkout -b new-branch
```
---
# 🔥 1️⃣1️⃣ Git Worktrees (Advanced)
Allows multiple branches checked out simultaneously in different folders.
Example:
```
git worktree add ../feature-branch feature
```
Used in:
* Large codebases
* Parallel feature development
---
# 🔥 1️⃣2️⃣ Large File Handling (Git LFS)
Git not good for:
* Large binaries
* Videos
* ML models
Solution:
Git LFS (Large File Storage)
Stores pointer in repo
Actual file stored externally.
---
# 🔥 1️⃣3️⃣ Security in Git
Very important in real world.
Never commit:
* API keys
* Passwords
* .env files
* Private certificates
Use:
* .gitignore
* Environment variables
* Secret managers
---
# 🔥 1️⃣4️⃣ Code Review Best Practices
Good PR:
* Small
* Clear description
* Linked issue
* Tested
* No unnecessary commits
Bad PR:
* 2000 lines
* No description
* Mixed features
* Untested
---
# 🔥 1️⃣5️⃣ Real Production Incident Example
Imagine:
You merge code.
Production crashes.
What do you do?
1. Revert commit:
   ```
   git revert <commit>
   ```
2. Deploy again
3. Investigate in separate branch
Never:
```
git reset --hard
git push --force
```
on main.
---
# 🧠 Module 6 Interview Master Questions
Be ready for:
1. Explain CI/CD pipeline.
2. What is branch protection?
3. Merge vs squash vs rebase merge?
4. What is semantic versioning?
5. How to find commit that broke code?
6. What is detached HEAD?
7. How to secure secrets in Git?
8. What is Git LFS?
9. What happens during PR?
10. How do large teams manage Git workflow?
---
# 🏁 After Module 6
You now understand:
✅ Git (core)
✅ Collaboration
✅ History rewriting
✅ Recovery
✅ Debugging
✅ CI/CD
✅ Production safety
✅ Enterprise workflows
This is beyond most college-level knowledge.
You are now at **junior developer industry level** in Git & GitHub.
