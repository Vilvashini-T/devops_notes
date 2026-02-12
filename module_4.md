# 🚀 MODULE 4 — REAL WORLD PROJECT IMPLEMENTATION (FULL DEPTH)
We will build **2 complete simulations**:
---
# 🔥 PROJECT 1 — Git Branching + Merge Conflict + GitHub Collaboration Simulation
---
# 🐳 PROJECT 2 — Dockerizing a Node.js App (Production-style)
* Dockerfile
* .dockerignore
* Multi-stage build
* Volume persistence
* Container networking
* Docker Compose
Everything step-by-step.
Everything explained internally.
---
# ===============================
# 🔥 PROJECT 1 — GIT SIMULATION
# ===============================
We simulate:
* 2 developers
* Feature branch
* Merge conflict
* Pull request flow
* Push rejection
* Conflict resolution
---
# 🧱 STEP 1 — Create Project
```bash
mkdir git-masterclass
cd git-masterclass
```
Create file:
```
index.js
```
Content:
```js
function greet() {
    console.log("Hello World");
}
greet();
```
---
# 🧱 STEP 2 — Initialize Git
```bash
git init
```
Internally:
* Creates `.git`
* Creates object store
* Sets HEAD
---
# 🧱 STEP 3 — Add and Commit
```bash
git add .
git commit -m "Initial commit"
```
What `.` means:
Current directory.
Internally:
* Blob created
* Tree created
* Commit object created
* Branch pointer updated
---
# 🧱 STEP 4 — Create Feature Branch
```bash
git branch feature-login
git switch feature-login
```
Now we simulate Developer 1 working on feature.
Modify `index.js`:
```js
function greet() {
    console.log("Hello from Feature Login");
}
greet();
```
Commit:
```bash
git add index.js
git commit -m "Updated greeting for login feature"
```
---
# 🧱 STEP 5 — Switch Back to Main
```bash
git switch main
```
Simulate Developer 2 modifying same file.
Modify `index.js`:
```js
function greet() {
    console.log("Hello from Main Branch");
}
greet();
```
Commit:
```bash
git add index.js
git commit -m "Main branch greeting update"
```
---
# 🧱 STEP 6 — Merge Feature (Conflict Simulation)
```bash
git merge feature-login
```
Git sees:
Same line changed in both branches.
Conflict appears:
```js
function greet() {
<<<<<<< HEAD
    console.log("Hello from Main Branch");
=======
    console.log("Hello from Feature Login");
>>>>>>> feature-login
}
```
---
# 🔎 WHAT HAPPENED INTERNALLY?
Git did:
1. Found common ancestor
2. Compared differences
3. Could not auto-merge
4. Inserted conflict markers
5. Paused merge
No commit created yet.
---
# 🧱 STEP 7 — Resolve Conflict
Manually edit:
```js
function greet() {
    console.log("Hello from Main + Feature");
}
greet();
```
Now:
```bash
git add index.js
git commit -m "Resolved merge conflict"
```
Now Git creates:
* New tree
* New commit with 2 parents
---
# 🎯 Interview Question:
Why does merge create a commit with 2 parents?
Because it combines two histories.
---
# 🧱 STEP 8 — Simulate GitHub Remote
Create GitHub repo.
Add remote:
```bash
git remote add origin https://github.com/username/git-masterclass.git
```
Push:
```bash
git push -u origin main
```
`-u` means:
Set upstream branch.
Now future pushes don't need branch name.
---
# 🧱 STEP 9 — Simulate Push Rejection
Clone repo into another folder:
```bash
cd ..
git clone https://github.com/username/git-masterclass.git dev2
```
Make change in dev2.
Push from dev2.
Now go back to original folder.
Make another commit.
Try:
```bash
git push
```
You get:
```
rejected non-fast-forward
```
Why?
Remote has new commits.
Your branch is behind.
Fix:
```bash
git pull --rebase
git push
```
---
# ===============================
# 🐳 PROJECT 2 — DOCKER NODE APP
# ===============================
---
# 🧱 STEP 1 — Create App Structure
```
docker-app/
 ├── package.json
 ├── index.js
 └── .dockerignore
```
---
# 🧾 package.json
```json
{
  "name": "docker-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
---
# 🧾 index.js
```js
const express = require("express");
const app = express();
app.get("/", (req, res) => {
    res.send("Hello from Dockerized App!");
});
app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```
---
# 🧾 .dockerignore
```
node_modules
.git
```
Why?
Reduce build context size.
---
# 🧱 STEP 2 — Create Dockerfile
```
FROM node:18-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```
---
# 🔍 BREAK DOWN DOCKERFILE
---
## FROM node:18-alpine
* Base image
* Alpine = lightweight Linux
Internally:
Pulls base image layers.
---
## WORKDIR /app
Sets working directory inside container.
Creates folder if not exists.
---
## COPY package.json .
Copy file from:
Host → container
`.` here means current WORKDIR.
---
## RUN npm install
Executes command inside container during build.
Creates new layer.
---
## COPY . .
Copies entire project.
---
## EXPOSE 3000
Documentation only.
Does not publish port.
---
## CMD ["npm", "start"]
Default command when container starts.
---
# 🧱 STEP 3 — Build Image
```bash
docker build -t docker-app .
```
* `-t` tag
* `.` build context
Internally:
1. Sends folder to daemon
2. Processes Dockerfile line-by-line
3. Creates layers
4. Stores image
---
# 🧱 STEP 4 — Run Container
```bash
docker run -d -p 5000:3000 --name myapp docker-app
```
Now:
Open browser:
```
http://localhost:5000
```
---
# 🧠 WHAT HAPPENED INTERNALLY?
1. Namespace created
2. cgroup created
3. Writable layer created
4. Port 5000 mapped to 3000
5. Node process started
Container is just process.
---
# 🧱 STEP 5 — Volume Persistence
Modify app to log visits.
Add:
```js
const fs = require("fs");
app.get("/", (req, res) => {
    fs.appendFileSync("visits.txt", "Visited\n");
    res.send("Visit logged!");
});
```
Run container:
```bash
docker run -d -p 5000:3000 -v myvolume:/app --name myapp docker-app
```
`-v` = volume mount.
Now file persists even if container removed.
---
# 🧱 STEP 6 — Docker Networking (Two Containers)
Run Redis:
```bash
docker run -d --name redis redis
```
Run app connected to Redis:
```bash
docker network create mynetwork
docker network connect mynetwork myapp
docker network connect mynetwork redis
```
Now containers communicate via name.
---
# 🧱 STEP 7 — Docker Compose
Create:
```
docker-compose.yml
```
```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "5000:3000"
  redis:
    image: redis
```
Run:
```bash
docker compose up
```
Now both containers start together.
---
# 🎯 Interview Questions (Module 4)
1. Why does Docker build create layers?
2. Why is .dockerignore important?
3. Difference between COPY and ADD?
4. Why does push get rejected?
5. What is fast-forward merge?
6. What happens when you remove container but not volume?
7. Why does container exit immediately?
---
# 🧠 Debugging Scenarios
1. Port already in use → another process running.
2. Container exits → CMD process crashed.
3. npm install fails → missing package.json.
4. Push rejected → remote ahead.
5. Merge conflict → same line modified.
---
# 🏁 What You Now Know
You can:
* Simulate branching
* Resolve conflicts
* Push/pull properly
* Dockerize Node app
* Use volumes
* Use networking
* Use compose
* Explain internals
