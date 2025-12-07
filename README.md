# ALXprodev-advanced_git

Git-Flow is a branching model for Git, proposed by Vincent Driessen, that helps developers manage features, releases, and hotfixes in a consistent and scalable way. It introduces well-defined roles for branches and helps teams coordinate code changes more effectively.

These hooks live inside `.git/hooks/` and are simple shell scripts that Git executes at specific events.

---

## ✅ Step 1: Pre‑Commit Hook

**File:** `.git/hooks/pre-commit`  
Purpose: Ensure every directory has a `README.md` before committing.

```bash
#!/bin/bash
# Pre-commit hook: check that each directory has a README.md

missing_readme=false

for dir in $(find . -type d ! -path "./.git*" ); do
    if [ ! -f "$dir/README.md" ]; then
        echo "❌ Missing README.md in directory: $dir"
        missing_readme=true
    fi
done

if [ "$missing_readme" = true ]; then
    echo "Commit aborted. Please add README.md files to all directories."
    exit 1
fi

exit 0
```

Make it executable:

```bash
chmod +x .git/hooks/pre-commit
```

---

## ✅ Step 2: Post‑Merge Hook

**File:** `.git/hooks/post-merge`  
Purpose: Log merges into the `main` branch.

```bash
#!/bin/bash
# Post-merge hook: log merges into main branch

branch=$(git rev-parse --abbrev-ref HEAD)

if [ "$branch" = "main" ]; then
    echo "✅ Merge completed into main on $(date)" >> merge.log
fi
```

Make it executable:

```bash
chmod +x .git/hooks/post-merge
```

---

## ✅ Step 3: Verify

- Try committing without a `README.md` in a directory → commit should be blocked.  
- Merge into `main` → a line is appended to `merge.log` with timestamp.  

---

## ✅ Commit Message Style

```bash
chore(git-hooks): add pre-commit and post-merge automation

- Pre-commit hook checks for README.md in all directories
- Post-merge hook logs merges into main branch with timestamp
```

---

🎯 Outcome: Your repo now has **automated GitFlow checks** — enforcing documentation discipline and logging merges into `main`.  