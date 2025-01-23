# Trunk-Based Development

## Table of Contents

1. [Overview of Trunk-Based Development](#overview-of-trunk-based-development)
2. [Branch Structure and Tree Visualization](#branch-structure-and-tree-visualization)
3. [Workflow Examples Using Git Commands](#workflow-examples-using-git-commands)
4. [Best Practices for Trunk-Based Development](#best-practices-for-trunk-based-development)
5. [Advantages of Trunk-Based Development](#advantages-of-trunk-based-development)
6. [Limitations of Trunk-Based Development](#limitations-of-trunk-based-development)
7. [Practical Examples](#practical-examples)
8. [Combined Commands](#combined-commands)
9. [Key Takeaways](#key-takeaways)

---

## Overview of Trunk-Based Development

Trunk-Based Development (TBD) is a streamlined Git workflow that emphasizes:
- A **single shared branch** (usually `main` or `trunk`) as the source of truth.
- **Short-lived feature branches** that are frequently integrated into the shared branch.
- **Continuous Integration (CI)** to ensure the branch is always deployable.

It is commonly used in **Continuous Delivery (CD)** workflows, enabling rapid and frequent releases.

---

## Branch Structure and Tree Visualization

In Trunk-Based Development, the structure avoids long-lived branches, promoting frequent integration. Here is an example:

```
(main)        *--*--*--*--*--*--* (frequent integration)
                |  |  |  |  |  |
(feature/xyz)   *  |  |  |  |  |
                   *  |  |  |  |
                      *  |  |  |
                         *  |  |
                            *--*
```

### Key Points:
- The `main` branch is always **deployable**.
- Feature branches are short-lived and merged back to `main` frequently.
- No long-lived `develop` or `release` branches are used.

---

## Workflow Examples Using Git Commands

### Basic Workflow

1. **Start a New Feature**:
   ```bash
   git checkout main
   git pull origin main       # Ensure main is up-to-date
   git checkout -b feature/new-feature
   ```

2. **Work on Changes**:
   ```bash
   # Make changes, stage, and commit
   git add .
   git commit -m "Implement new feature logic"
   ```

3. **Push and Test**:
   ```bash
   git push -u origin feature/new-feature
   # CI pipelines run automated tests after the push
   ```

4. **Merge into `main`**:
   ```bash
   git checkout main
   git pull origin main
   git merge feature/new-feature
   git push origin main
   ```

---

### Medium Workflow

#### Keeping Feature Branches in Sync with `main`

```bash
git checkout feature/new-feature
git pull --rebase origin main
# Resolve any conflicts
git add .
git rebase --continue
```

#### Avoiding Merge Conflicts with Frequent Commits

```bash
git add .
git commit -m "Update feature step 1"
git push
```

#### Feature Flagging
To keep `main` deployable, use feature flags to hide incomplete work:

```python
# Example in code
if feature_flag_enabled('new_feature'):
    run_new_feature()
else:
    run_old_feature()
```

---

## Best Practices for Trunk-Based Development

### Frequent Integration
- Commit and push changes to `main` multiple times daily.

### Use Automated CI/CD
- Integrate automated testing pipelines to catch bugs early.

### Keep Feature Branches Short-Lived
- Merge branches into `main` as soon as possible to avoid conflicts.

### Feature Flags
- Use feature flags to release incomplete features safely.

### Clean Commit History
- Use squashing or rebasing to maintain a clean, readable history.

---

## Advantages of Trunk-Based Development

1. **Continuous Integration**:
   - Frequent integration ensures rapid feedback and bug detection.
2. **Minimized Merge Conflicts**:
   - Frequent merges reduce the risk of large, difficult-to-resolve conflicts.
3. **Faster Release Cycles**:
   - Enables frequent and reliable deployments.
4. **Simplicity**:
   - No long-lived branches to manage, reducing complexity.

---

## Limitations of Trunk-Based Development

1. **Discipline Required**:
   - Requires developers to integrate frequently and write high-quality commits.
2. **Complexity in Large Teams**:
   - Simultaneous contributions from many developers can create contention.
3. **Dependence on CI/CD**:
   - Heavily relies on automated testing to maintain stability.

---

## Practical Examples

### Example 1: Simple Feature Integration

```bash
git checkout main
git pull origin main
git checkout -b feature/add-login
# Make changes
git add .
git commit -m "Add login functionality"
git push -u origin feature/add-login
# Merge into main
git checkout main
git pull origin main
git merge feature/add-login
git push origin main
```

### Example 2: Handling Updates in `main`

```bash
# While working on a feature, keep it updated with main
git checkout feature/new-feature
git pull --rebase origin main
# Resolve conflicts if necessary
git push --force
```

### Example 3: Continuous Deployment Pipeline

1. Add a `.gitlab-ci.yml` or `.github/workflows/ci.yml` file:

```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Tests
        run: npm test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Production
        run: ./deploy.sh
```

---

## Combined Commands

### Auto-Rebase and Push

```bash
git pull --rebase origin main && git push
```

### Stashing Changes

```bash
git stash
git pull origin main
git stash pop
```

### Conflict Resolution

```bash
git checkout main
git merge feature/conflict-fix
git mergetool
git commit
```

---

## Key Takeaways

1. Trunk-Based Development is ideal for **fast-moving teams** and projects emphasizing **CI/CD**.
2. Focus on **short-lived branches**, frequent integration, and **deployable main**.
3. Use **feature flags** to release incomplete features without breaking production.
4. Leverage tools like **Git hooks** or CI/CD pipelines for quality control and deployment.

By adopting Trunk-Based Development, teams can streamline their workflows, reduce complexity, and achieve faster, more reliable software delivery.
