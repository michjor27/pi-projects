# Dev Workflow: Mac ↔ GitHub ↔ Raspberry Pis

Repo: `pi-projects` (github.com/michjor27/pi-projects)
Structure: one repo, one subfolder per Pi project (e.g. `fusion_hat_ai/`, `proofervision/`)

Rule of thumb: **edit on the Mac, run/test on the Pi.** Never edit the same files on both without syncing in between.

---

## One-time setup (already done)

- SSH key generated and added to GitHub on the Mac
- Repo created on GitHub, cloned/pushed to `~/Documents/github_projects/pi-projects`
- `.gitignore` includes `.DS_Store`, `__pycache__/`, `*.pyc`, `venv/`, `.env`, `*.log`

### Adding a new Pi (do once per Pi)

```
ssh pi@<pi-hostname>.local
ssh-keygen -t ed25519 -C "pi-hostname"
cat ~/.ssh/id_ed25519.pub
```
Copy the printed key → GitHub → Settings → SSH and GPG keys → New SSH key → paste → save.

Test, then clone:
```
ssh -T git@github.com
git clone git@github.com:michjor27/pi-projects.git ~/pi-projects
```

### Adding a new subproject (on the Mac)

```
cd ~/Documents/github_projects/pi-projects
mkdir new_project_name
cd new_project_name
touch README.md
cd ..
git add new_project_name
git commit -m "add new_project_name subproject"
git push
```

---

## Daily workflow

### 1. Edit on the Mac (VS Code, local — not Remote-SSH)

Work normally in `~/Documents/github_projects/pi-projects/<subproject>/`.

### 2. Push changes to GitHub

```
cd ~/Documents/github_projects/pi-projects
git add .
git commit -m "describe what changed"
git push
```

### 3. Pull changes onto the Pi you're testing on

```
ssh pi@<pi-hostname>.local
cd ~/pi-projects
git pull
cd <subproject>
python3 main.py
```

### 4. If you made changes ON the Pi (e.g. debugging live) — sync back before editing on the Mac again

```
# on the Pi
cd ~/pi-projects
git add .
git commit -m "pi-side fix"
git push
```
```
# back on the Mac
cd ~/Documents/github_projects/pi-projects
git pull
```

---

## Quick reference — commands

| Action | Command |
|---|---|
| Check what's changed | `git status` |
| Stage all changes | `git add .` |
| Commit staged changes | `git commit -m "message"` |
| Send commits to GitHub | `git push` |
| Get latest from GitHub | `git pull` |
| See commit history | `git log --oneline` |
| Undo unstaged edits to a file | `git restore <file>` |

---

## Common gotchas

- **"Everything up-to-date" after `git push`** → you forgot to `git commit` first. `git add` only stages; it doesn't save.
- **`ssh push -u origin main`** → typo. Push is a `git` command, not `ssh`. Should be `git push -u origin main`.
- **`! [rejected] (fetch first)`** → remote has commits you don't have locally. Run `git pull` first, then push again.
- **Divergent branches error on pull** → run `git pull --no-rebase` (does a normal merge; may open Vim for a merge commit message — press `i`, type message, `Esc`, `:wq`, Enter).
- **`.DS_Store` keeps showing as changed** → make sure it's in `.gitignore` and run `git rm --cached .DS_Store` once to untrack it.
- **Merge conflicts** → run `git status` to see which files conflict, open them, resolve the `<<<<<<<` / `=======` / `>>>>>>>` markers manually, then `git add <file>` and `git commit`.
