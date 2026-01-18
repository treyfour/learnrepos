# Git & GitHub Learning Plan for Cursor IDE

## Your Current Setup
- **Repo**: `learnrepos` (Next.js project)
- **Remote**: `https://github.com/treyfour/learnrepos.git`
- **IDE**: Cursor

---

## Module 1: Understanding the Git Mental Model

### The Three Areas of Git
```
Working Directory  →  Staging Area  →  Repository (Commits)
    (your files)       (git add)        (git commit)
```

**Key Insight**: Nothing is "saved" to version control until you commit. Files go through stages:
1. **Untracked/Modified** - Git sees changes but isn't tracking them yet
2. **Staged** - Marked for the next commit (`git add`)
3. **Committed** - Permanently saved in history (`git commit`)

### When Things Get Overwritten (Critical Knowledge)
| Action | What Happens | Safe? |
|--------|--------------|-------|
| `git checkout <file>` | Discards uncommitted changes | DESTRUCTIVE |
| `git reset --hard` | Discards ALL uncommitted changes | DESTRUCTIVE |
| `git pull` with conflicts | Requires manual merge | SAFE (asks first) |
| `git merge` | Combines branches | SAFE (creates merge commit) |
| `git stash` | Temporarily saves changes | SAFE (can recover) |

**Golden Rule**: Committed code is almost always recoverable. Uncommitted code can be lost.

---

## Module 2: Essential Git Commands in Cursor

### Exercise 2.1: Check Status (Do This Constantly)
```bash
git status
```
Cursor shortcut: Look at the Source Control panel (left sidebar, branch icon)

### Exercise 2.2: Your First Intentional Commit
1. Open `README.md` in Cursor
2. Add a line: `## Learning Git with Cursor`
3. Save the file
4. In terminal:
```bash
git status              # See the modified file (red)
git add README.md       # Stage it (now green in status)
git status              # Confirm it's staged
git commit -m "docs: add learning section to README"
```

### Exercise 2.3: Push to GitHub
```bash
git push origin main
```
Then check GitHub in your browser - your change should be there!

---

## Module 3: Branches - The Core of Git Workflow

### Why Branches?
- **main** = stable, production-ready code
- **feature branches** = experimental/work-in-progress code
- You work on branches, then merge to main when ready

### Exercise 3.1: Create and Switch Branches
```bash
# See all branches
git branch

# Create a new branch
git branch feature/add-about-page

# Switch to it
git checkout feature/add-about-page

# OR do both in one command (preferred)
git checkout -b feature/add-about-page
```

**Cursor Tip**: Click the branch name in the bottom-left corner to see/switch branches

### Exercise 3.2: Work on Your Branch
1. Create a new file: `app/about/page.tsx`
```tsx
export default function About() {
  return (
    <main>
      <h1>About Page</h1>
      <p>Learning Git branches!</p>
    </main>
  );
}
```
2. Commit it:
```bash
git add app/about/page.tsx
git commit -m "feat: add about page"
```

### Exercise 3.3: Push Your Branch to GitHub
```bash
git push -u origin feature/add-about-page
```
The `-u` sets up tracking so future pushes just need `git push`

---

## Module 4: Merging Branches

### Exercise 4.1: Merge Feature into Main
```bash
# First, switch to main
git checkout main

# Make sure main is up to date
git pull origin main

# Merge your feature branch
git merge feature/add-about-page

# Push the updated main
git push origin main
```

### Understanding Merge Outcomes
1. **Fast-forward**: Main hadn't changed, your commits slide in cleanly
2. **Merge commit**: Main had changes, Git creates a merge commit combining both
3. **Conflict**: Same lines changed in both branches - requires manual resolution

### Exercise 4.2: Simulate a Merge Conflict
1. On `main`, edit line 1 of README.md
2. Commit: `git commit -am "edit readme on main"`
3. Create new branch: `git checkout -b feature/conflict-demo`
4. Edit the SAME line 1 differently
5. Commit: `git commit -am "edit readme on feature"`
6. Switch back: `git checkout main`
7. Try to merge: `git merge feature/conflict-demo`
8. **Resolve the conflict** in Cursor (it highlights conflicts!)
9. `git add README.md && git commit -m "resolve merge conflict"`

---

## Module 5: Cloning Repositories

### What Cloning Does
Creates a complete copy of a repo including ALL history

```bash
# Clone any public repo
git clone https://github.com/username/repo-name.git

# Clone into a specific folder
git clone https://github.com/username/repo-name.git my-folder-name
```

### Exercise 5.1: Clone a Popular Repo to Study
```bash
cd ~/dev
git clone https://github.com/shadcn-ui/ui.git shadcn-study
cd shadcn-study
git log --oneline -20  # See recent commit history
```

---

## Module 6: Advanced Safety Techniques

### Stashing (Temporary Save)
When you need to switch branches but have uncommitted work:
```bash
git stash                    # Save changes temporarily
git checkout other-branch    # Do other work
git checkout original-branch
git stash pop                # Restore your changes
```

### Viewing History
```bash
git log --oneline           # Compact history
git log --graph --oneline   # Visual branch history
git show <commit-hash>      # See specific commit details
```

### Undoing Mistakes
```bash
# Unstage a file (keep changes)
git reset HEAD <file>

# Discard changes to a file (DESTRUCTIVE)
git checkout -- <file>

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes) - DESTRUCTIVE
git reset --hard HEAD~1
```

---

## Module 7: Daily Workflow Checklist

### Starting Work
```bash
git checkout main
git pull origin main
git checkout -b feature/your-feature-name
```

### During Work
```bash
git status                  # Check what's changed
git add <files>             # Stage specific files
git commit -m "message"     # Commit with clear message
```

### Finishing Work
```bash
git push -u origin feature/your-feature-name
# Create Pull Request on GitHub
# After review/approval, merge on GitHub
git checkout main
git pull origin main
git branch -d feature/your-feature-name  # Clean up local branch
```

---

## Commit Message Convention
```
type: short description

Types:
- feat: new feature
- fix: bug fix
- docs: documentation
- style: formatting
- refactor: code restructure
- test: adding tests
- chore: maintenance
```

---

## Cursor-Specific Tips

1. **Source Control Panel**: Shows changed files, lets you stage/commit with clicks
2. **GitLens Extension**: Shows who changed each line and when
3. **Bottom Status Bar**: Shows current branch, sync status
4. **Command Palette** (Cmd+Shift+P): Type "Git:" to see all Git commands
5. **Inline Blame**: Hover over lines to see last commit info

---

## Practice Exercises (Do These!)

1. [ ] Create 3 feature branches and merge them to main
2. [ ] Intentionally create and resolve a merge conflict
3. [ ] Use `git stash` to save work, switch branches, then restore
4. [ ] Clone an open-source repo and explore its history
5. [ ] Practice the daily workflow for a full week

---

## Quick Reference Card

| Want to... | Command |
|------------|---------|
| Check status | `git status` |
| See branches | `git branch` |
| Create branch | `git checkout -b name` |
| Switch branch | `git checkout name` |
| Stage files | `git add <file>` |
| Stage all | `git add .` |
| Commit | `git commit -m "msg"` |
| Push | `git push origin branch` |
| Pull | `git pull origin branch` |
| Merge | `git merge branch` |
| See history | `git log --oneline` |
| Stash | `git stash` / `git stash pop` |

---

## Next Steps with Claude Code

Ask me to help you:
1. "Walk me through creating a feature branch and merging it"
2. "Help me understand what this git status output means"
3. "Let's practice resolving a merge conflict together"
4. "Explain what would happen if I run [command]"
5. "Review my commit history and suggest improvements"
