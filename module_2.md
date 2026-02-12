# MODULE 2 — CORE ARCHITECTURE & INTERNAL WORKING
We divide this into two major systems:
1. **Git Internal Architecture**
2. **Docker Internal Architecture**
---
# 🧠 PART 1 — GIT INTERNAL ARCHITECTURE
---
# 1️⃣ Git Is a Content-Addressable Database
When you run:
```bash
git init
```
What actually happens?
It creates a hidden folder:
```
.git/
```
This is the entire Git database.
Let’s inspect its structure:
```
.git/
 ├── HEAD
 ├── config
 ├── description
 ├── hooks/
 ├── info/
 ├── objects/
 └── refs/
```
Everything Git does is stored here.
---
# 2️⃣ The Three Trees of Git
Git internally manages 3 states:
```
1. Working Directory
2. Staging Area (Index)
3. Repository (Commit history)
```
Visual:
```
Working Directory  →  Staging Area  →  Repository
   (files)             (index)         (objects)
```
This is the most misunderstood part of Git.
Let’s go deep.
---
# 3️⃣ What Is the Working Directory?
This is your actual project folder:
```
project/
 ├── app.js
 ├── index.html
 └── style.css
```
These files exist in your filesystem.
They are just normal files.
No Git magic here.
---
# 4️⃣ What Is the Staging Area? (DETAILED)
The staging area is NOT a folder.
It is a file:
```
.git/index
```
It is a binary file.
The staging area is:
> A snapshot builder.
It stores:
* File path
* File mode (permissions)
* SHA hash of content
* Timestamp
* File size
It does NOT store the file content itself.
It stores reference to blob object via SHA.
---
## Why Does Staging Area Exist?
Because Git separates:
"Selecting changes"
from
"Committing changes"
This allows:
* Partial commits
* Fine-grained control
* Cleaner history
Without staging:
Every commit would include everything modified.
---
## What Happens When You Run:
```bash
git add file.txt
```
Internal Steps:
1. Git reads file.txt
2. Computes SHA hash of content
3. Stores content as a blob in:
```
.git/objects/
```
4. Updates `.git/index`
   * Adds entry mapping file → blob SHA
So staging area = pointer table.
---
# 5️⃣ Git Object Model
Git has 4 object types:
1. Blob
2. Tree
3. Commit
4. Tag
---
## 🔹 Blob (Binary Large Object)
Represents file content.
Important:
* Blob does NOT store filename.
* Only content.
Example:
```
"Hello World"
```
Stored as blob with SHA:
```
e59ff97941044f85df5297e1c302d260
```
Location:
```
.git/objects/e5/9ff97941044f85df5297e1c302d260
```
Notice:
SHA is split:
* First 2 characters → folder
* Remaining → filename
Why?
To avoid too many files in one folder.
---
## 🔹 Tree Object
Tree represents directory.
It stores:
* File names
* Blob SHAs
* Sub-tree SHAs
* File permissions
Example:
```
project/
 ├── app.js
 └── src/
      └── main.js
```
Git builds:
```
Tree (project)
 ├── blob(app.js)
 └── tree(src)
        └── blob(main.js)
```
Tree is snapshot of folder structure.
---
## 🔹 Commit Object
Commit contains:
* Pointer to tree
* Parent commit
* Author
* Timestamp
* Message
Visual:
```
Commit
  |
  → Tree
       |
       → Blob
```
This is a Directed Acyclic Graph (DAG).
---
# 6️⃣ What Is HEAD?
Inside `.git/HEAD`:
```
ref: refs/heads/main
```
HEAD is:
> Pointer to current branch.
Branch is just:
> A file containing commit SHA.
Example:
```
.git/refs/heads/main
```
Contains:
```
f23ab94...
```
That’s it.
Branch is just pointer.
---
# 7️⃣ How Branching Actually Works
When you run:
```bash
git branch feature
```
Git creates:
```
.git/refs/heads/feature
```
Containing same commit SHA as current branch.
No copy of files.
No duplication.
Branching is O(1) operation.
That’s why Git branching is fast.
---
# 8️⃣ How Merge Works Internally
Let’s say:
```
A → B → C (main)
     \
      D → E (feature)
```
Merge does:
1. Find common ancestor (B)
2. Compare:
   * B vs C
   * B vs E
3. Combine differences
This is called 3-way merge.
If both modified same line → conflict.
Git inserts:
```
<<<<<<< HEAD
your changes
=======
their changes
>>>>>>> feature
```
Internally:
* New tree object created
* New commit created with 2 parents
---
# 9️⃣ How Rebase Works Internally
Rebase rewrites history.
Original:
```
A → B → C (main)
     \
      D → E (feature)
```
After rebase:
```
A → B → C → D' → E'
```
Git:
* Takes each commit
* Applies patch on new base
* Creates new commit objects
Important:
Rebase creates NEW commits (new SHA).
---
# 🔟 Staging Area Deep Mechanics
Staging area file:
```
.git/index
```
Contains:
* Path
* Mode
* SHA
* Flags
When you modify file:
Git compares:
* Working directory
* Index
* HEAD commit
This comparison gives:
```
Modified
Staged
Untracked
```
---
# 🐳 PART 2 — DOCKER INTERNAL ARCHITECTURE
---
# 1️⃣ Docker High-Level Architecture
```
Docker CLI
    ↓
Docker Daemon (dockerd)
    ↓
containerd
    ↓
runc
    ↓
Linux Kernel
```
---
# 2️⃣ Docker Daemon (dockerd)
Daemon:
* Background process
* Listens on Unix socket
* Manages images, networks, volumes
Command:
```bash
docker run nginx
```
CLI sends API request to daemon.
Daemon does heavy lifting.
---
# 3️⃣ containerd
Lower-level runtime.
Responsible for:
* Managing containers lifecycle
* Pulling images
* Storage
---
# 4️⃣ runc
Very low-level tool.
Implements:
* OCI standard
* Creates namespaces
* Applies cgroups
* Calls kernel syscalls
runc is what actually starts container process.
---
# 5️⃣ Docker Image Internals
Images stored in:
```
/var/lib/docker/
```
Inside:
* OverlayFS layers
* Metadata
* Diff directories
---
# 6️⃣ What Is OverlayFS?
Union filesystem.
Combines multiple layers:
```
Layer 1 (base OS)
Layer 2 (node installed)
Layer 3 (app code)
-------------------------
Merged View
```
Each layer is read-only.
Container adds:
Writable layer on top.
---
# 7️⃣ Namespaces Deep Dive
Linux namespaces types:
* PID namespace
* NET namespace
* MNT namespace
* UTS namespace
* IPC namespace
* USER namespace
Each isolates specific resource.
Example:
PID namespace:
Container thinks it is PID 1.
But host sees:
```
PID 48392
```
---
# 8️⃣ cgroups Deep Dive
Stored in:
```
/sys/fs/cgroup/
```
Controls:
* Memory limit
* CPU shares
* I/O throttle
Without cgroups:
One container can crash system.
---
# 9️⃣ What Happens When You Run `docker run`
Internally:
1. CLI sends request to daemon
2. Daemon checks if image exists
3. If not:
   * Pull from registry
4. Create container config
5. Create writable layer
6. containerd prepares runtime spec
7. runc:
   * Creates namespaces
   * Applies cgroups
   * Mounts filesystem
   * Executes ENTRYPOINT
8. Kernel schedules process
Container is now running.
It is just a process.
---
# 🔟 ENTRYPOINT vs CMD (Internals)
Dockerfile example:
```
ENTRYPOINT ["node"]
CMD ["app.js"]
```
ENTRYPOINT:
* Main executable
* Cannot be overridden easily
CMD:
* Default arguments
* Can be overridden
Internally:
Final command:
```
node app.js
```
If you run:
```
docker run image server.js
```
CMD replaced:
```
node server.js
```
---
# 🔥 OS LEVEL VIEW
When container runs:
```
ps aux | grep nginx
```
You see it as normal process.
Because it is.
Isolation is illusion created by namespaces.
---
# 🎯 KEY CONNECTION
Git:
* Objects
* Hashes
* DAG
* Immutable snapshots
Docker:
* Layers
* Hashes
* Union FS
* Immutable images
Both are:
Content-addressable immutable systems.
---
# MODULE 2 — PRACTICE
1. Manually inspect `.git/objects`
2. Delete index file and observe behavior
3. Run `ps aux` while container running
4. Explore `/sys/fs/cgroup`
5. Inspect `/var/lib/docker/overlay2`
---
# INTERVIEW QUESTIONS
1. Explain Git staging area internally.
2. Difference between blob and tree?
3. How does merge find common ancestor?
4. Why is rebase dangerous?
5. Explain Docker architecture from CLI to kernel.
6. What are namespaces?
7. What are cgroups?
8. Why is container just a process?
