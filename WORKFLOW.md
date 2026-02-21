# WORKFLOW (H.I.D.D.E.N)

This document describes the development workflow for the H.I.D.D.E.N Unreal Engine project.

## TL;DR
- Never work directly on `main`
- One branch per task/feature
- Always merge via Pull Request
- Avoid editing the same `.umap` / `.uasset` at the same time
- Create/move/rename Unreal assets only inside the Unreal Editor (not Finder/Explorer)

---

## 1) Branch Strategy

### Protected branch
- `main` is protected
- No direct pushes to `main`
- Changes land in `main` only via Pull Request (PR)

### Branch naming
Use one branch per task:
- `feature/<short-name>` (new gameplay/system)
- `fix/<short-name>` (bug fixes)
- `chore/<short-name>` (project setup, refactors, cleanup)

Examples:
- `feature/player-input`
- `feature/interaction-system`
- `fix/door-timeline`
- `chore/content-structure`

---

## 2) Daily Start (always)

Before starting work:
```bash
git switch main
git pull
```

Always begin from an up-to-date `main`.

---

## 3) Standard Feature Workflow

### 1. Create a New Branch
```bash
git switch -c feature/<name>
```

### 2. Work in Unreal Engine
Open the project:
- Double-click `HIDDEN.uproject`

While working:
- Save frequently
- Before committing: `File → Save All`

Important:
- Never move/rename Unreal assets in Finder/Explorer
- Always create/move/rename assets inside Unreal Editor

### 3. Check Changes
```bash
git status
```

### 4. Commit Changes (Small Commits Preferred)
```bash
git add .
git commit -m "Short description of the change"
```

### 5. Push the Branch
First push:
```bash
git push -u origin feature/<name>
```

Subsequent pushes:
```bash
git push
```

### 6. Open a Pull Request
- Base branch: `main`
- Compare branch: `feature/<name>`
- Assign the other developer as reviewer

### 7. Review & Merge
- Reviewer approves
- Merge into `main`

### 8. Cleanup After Merge
```bash
git switch main
git pull
git branch -d feature/<name>
```

---

## 4) Working in Parallel (Two Developers)
Unreal assets (`.uasset`, `.umap`) are binary and cannot be merged reliably.

**Important Rule**

Do not edit the same asset at the same time.

High-risk files:
- `.umap` (Maps)
- Core Blueprints
- Shared systems

**Recommended separation:**

Developer A:
- Core systems
- Player Blueprint
- Interaction systems
- Audio systems

Developer B:
- Level design
- Greyboxing
- Environment setup

If both must work on the same file:
- Coordinate before editing.

---

## 5) Syncing When `main` Changes
If `main` was updated while you are working:

```bash
git switch main
git pull
git switch feature/<name>
git merge main
```

Keep it simple. Avoid rebasing unless necessary.

---

## 6) Git LFS
The project uses Git LFS for Unreal assets:
- `*.uasset`
- `*.umap`

Make sure Git LFS is installed:
```bash
git lfs install
```

---

## 7) Ignored Folders (Never Commit)

The following folders must never be committed:
- `Binaries/`
- `Intermediate/`
- `Saved/`
- `DerivedDataCache/`

---

## 8) Unreal-Specific Best Practices
- Always use "Save All" before committing
- Fix redirectors when moving assets
- Keep PRs small and focused
- Avoid massive multi-system commits

---

## 9) Conflict Strategy
If a `.uasset` or `.umap` conflict occurs:
1. Communicate immediately
2. Decide whose version stays
