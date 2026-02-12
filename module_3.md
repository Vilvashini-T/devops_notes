# 🚀 MODULE 3 — COMMANDS (Deep, Symbol-by-Symbol, Internals Explained)
---
# 🔹 SECTION 1 — COMMAND LINE FOUNDATIONS
Before Git & Docker, you must understand CLI syntax.
---
## 1️⃣ What is `.` (dot)?
`.` means:
> Current directory
Example:
```bash
ls .
```
Means: list files in current directory.
In Git:
```bash
git add .
```
Means:
Add all files in current directory.
Internally:
* Git recursively scans working directory
* Computes SHA for all modified files
* Updates index
---
## 2️⃣ What is `..` (double dot)?
`..` means:
> Parent directory
Example:
```bash
cd ..
```
Move one level up.
If you're in:
```
/home/user/project/src
```
`cd ..` moves to:
```
/home/user/project
```
---
## 3️⃣ What is `~` (tilde)?
`~` means:
> Home directory
On Linux/macOS:
```
~ = /home/username
```
On Windows Git Bash:
```
~ = C:/Users/YourName
```
Example:
```bash
cd ~
```
Goes to home folder.
---
## 4️⃣ What is `/` (forward slash)?
Represents:
> Root directory (Linux/Mac)
> Folder separator
Example:
```
/etc
/usr
/home
```
On Windows, internally uses `\`
But Git Bash converts `/`.
---
## 5️⃣ What is `-` (single dash)?
Single dash introduces:
> Short option / flag
Example:
```bash
git commit -m "message"
```
`-m` = short form of `--message`
---
## 6️⃣ What is `--` (double dash)?
Double dash introduces:
> Long option name
Example:
```bash
docker run --name mycontainer
```
Readable version of:
```bash
docker run -n mycontainer
```
(Not all commands have short forms.)
---
# 🔹 SECTION 2 — GIT COMMANDS (STEP BY STEP)
We build a full project.
---
# 🧱 Step 1 — Create Project
```bash
mkdir git-demo
cd git-demo
```
`mkdir` = make directory
`cd` = change directory
---
# 🧱 Step 2 — Initialize Git
```bash
git init
```
Breakdown:
* `git` → Git CLI program
* `init` → initialize new repository
What happens internally:
1. Creates `.git/` folder
2. Creates object database
3. Sets default branch (main/master)
4. Creates HEAD file
---
Check structure:
```
git-demo/
 └── .git/
      ├── HEAD
      ├── objects/
      └── refs/
```
---
# 🧱 Step 3 — Create File
Create:
```
app.js
```
Content:
```js
console.log("Hello Git");
```
---
# 🧱 Step 4 — Check Status
```bash
git status
```
What happens internally:
* Git compares:
  * Working directory
  * Index (staging)
  * HEAD commit
Output:
```
Untracked files:
  app.js
```
Untracked = file exists but not in index.
---
# 🧱 Step 5 — Add File
```bash
git add app.js
```
Breakdown:
* `add` = add file to staging area
* `app.js` = target file
Internal steps:
1. Read file content
2. Compute SHA
3. Store blob in `.git/objects`
4. Update `.git/index`
Now file is staged.
---
# 🧱 Step 6 — Commit
```bash
git commit -m "Initial commit"
```
Breakdown:
* `commit` = create snapshot
* `-m` = message
* `"Initial commit"` = commit message
Internal steps:
1. Read index
2. Create tree object
3. Create commit object
4. Store commit SHA
5. Update branch pointer
Now history created.
---
# 🧱 Step 7 — View Log
```bash
git log
```
Shows:
* Commit SHA
* Author
* Date
* Message
Internally:
Reads commit objects from `.git/objects`.
---
# 🧱 Step 8 — Create Branch
```bash
git branch feature
```
Breakdown:
* `branch` = create pointer
* `feature` = branch name
Internally:
Creates file:
```
.git/refs/heads/feature
```
Containing current commit SHA.
---
Switch branch:
```bash
git checkout feature
```
OR modern:
```bash
git switch feature
```
What happens:
* HEAD updated
* Working directory updated to match commit
---
# 🧱 Step 9 — Merge
```bash
git merge feature
```
Internally:
1. Find common ancestor
2. Perform 3-way merge
3. Create new commit
4. Update branch pointer
---
# 🔹 SECTION 3 — GITHUB COMMANDS
---
## Clone Repository
```bash
git clone https://github.com/user/repo.git
```
Breakdown:
* `clone` = copy remote repo
* URL = remote location
Internally:
1. Create directory
2. Initialize Git
3. Fetch objects
4. Set origin remote
5. Checkout default branch
---
## Add Remote
```bash
git remote add origin https://github.com/user/repo.git
```
* `remote` = manage remotes
* `add` = add new remote
* `origin` = name
* URL = location
Stored in:
```
.git/config
```
---
## Push
```bash
git push origin main
```
Breakdown:
* `push` = send commits
* `origin` = remote name
* `main` = branch
Internally:
1. Find commits remote doesn't have
2. Send objects
3. Update remote branch pointer
---
## Pull
```bash
git pull origin main
```
Internally:
= `git fetch` + `git merge`
---
# 🔹 SECTION 4 — DOCKER COMMANDS (DEEP BREAKDOWN)
---
## 1️⃣ Check Version
```bash
docker --version
```
* `docker` = CLI
* `--version` = long flag
Returns client version.
---
## 2️⃣ Pull Image
```bash
docker pull nginx
```
Breakdown:
* `pull` = download image
* `nginx` = image name
Internally:
1. Contact registry
2. Fetch manifest
3. Download layers
4. Store in `/var/lib/docker`
---
## 3️⃣ Run Container
```bash
docker run -d -p 8080:80 --name mynginx nginx
```
Now we break EVERYTHING:
* `run` = create + start container
* `-d` = detached mode (background)
* `-p` = publish port
* `8080:80` = host:container
* `--name` = container name
* `mynginx` = name value
* `nginx` = image
Internally:
1. Create container config
2. Create writable layer
3. Create namespaces
4. Apply cgroups
5. Setup network bridge
6. Map port 8080 → container 80
7. Execute ENTRYPOINT
---
## 4️⃣ Interactive Mode
```bash
docker run -it ubuntu
```
Breakdown:
* `-i` = interactive (keep STDIN open)
* `-t` = allocate TTY
* `ubuntu` = image
TTY = pseudo terminal
Without `-it`, no shell interaction.
---
## 5️⃣ Stop Container
```bash
docker stop mynginx
```
Sends:
SIGTERM → wait → SIGKILL
---
## 6️⃣ Remove Container
```bash
docker rm mynginx
```
Deletes container metadata + writable layer.
---
## 7️⃣ Build Image
```bash
docker build -t myapp .
```
Breakdown:
* `build` = build image
* `-t` = tag
* `myapp` = image name
* `.` = current directory (build context)
Internally:
1. Send context to daemon
2. Read Dockerfile
3. Execute instructions
4. Create layers
5. Assign tag
---
# 🔹 ENTRYPOINT vs CMD (Interview Ready Explanation)
ENTRYPOINT:
* Main executable
* Cannot easily override
CMD:
* Default arguments
* Overridden by CLI
Best practice:
Use ENTRYPOINT for fixed executable.
Use CMD for parameters.
---
# 🔹 COMMON INTERVIEW QUESTIONS (MODULE 3)
1. Difference between git add and git commit?
2. What does `git pull` actually do?
3. Why do we use `-m` in commit?
4. What does `docker run -it` do internally?
5. What does `-p 8080:80` mean?
6. What does `.` mean in docker build?
7. Difference between `docker run` and `docker start`?
---
# 🔹 DEBUGGING SCENARIOS
1. Port already in use → why?
2. Container exits immediately → why?
3. Git says “detached HEAD” → what happened?
4. Git push rejected → why?
---
# SUMMARY
Now you understand:
* CLI symbols
* Git workflow
* GitHub flow
* Docker run flags
* Docker build
* What happens internally
