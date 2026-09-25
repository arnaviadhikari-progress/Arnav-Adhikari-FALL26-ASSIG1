# Mission 0: Get the code, the professional way

**Name:**
**GitHub username:**
arnaviadhikari-progress

## Evidence

### `git remote -v`
```
paste here
origin  https://github.com/arnaviadhikari-progress/Arnav-Adhikari-FALL26-ASSIG1.git (fetch)
origin  https://github.com/arnaviadhikari-progress/Arnav-Adhikari-FALL26-ASSIG1.git (push)

```

### `git branch`
```
paste here
* Assignment1
  main

```

### `git status` before the `.gitignore` fix
```
paste here
On branch Assignment1
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        node_modules/
        package-lock.json

nothing added to commit but untracked files present (use "git add" to track)

```

### `git status` after the `.gitignore` fix
```
paste here
On branch Assignment1
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")

-- AFTER COMMIT --

On branch Assignment1
nothing to commit, working tree clean

```

## Questions

1. Which folder should not be committed, and why? Give one practical reason and one security-related reason.

   > the folder node_modules should not be committed. This is because it contains an extremely large amount of data that would take a long amount of time, as well as make the git response time incredibly slow. Additionally, this folder contains third party code, which significantly increases the attack surface an attacker can access through.

2. What line or lines did you add to `.gitignore`? What does a trailing `/` mean in a `.gitignore` pattern?

   > In .gitignore, I added the lines: "node_modules/" and "package-lock.json". The trailing '/' in a .gitignore pattern means to ignore the ENTIRE folder, rather than a specific file.

3. **Connections:** in one or two sentences, what is the difference between a **fork** and a **clone**? Which one lives on GitHub and which one lives on your machine?

   > The difference between a fork and a clone is where the repository is stored. Fork creates a personal copy of a repository to the github cloud, whereas clone creates a copy of the repository on my local device.
