# v2.0.2: Git Configuration Fix 👤

## 🐛 What's Fixed

### Critical Git Configuration Issue
This release fixes the **"Author identity unknown"** error that was preventing commits when git user inputs weren't explicitly provided in workflows.

## 🔍 The Problem

Users were encountering this error:
```
Author identity unknown
*** Please tell me who you are.
Run
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
to set your account's default identity.
fatal: empty ident name (for <>) not allowed
```

This happened when workflows didn't specify `git-user-name` and `git-user-email` inputs.

## ✅ The Solution

### Enhanced Input Handling
- **Added fallback defaults** for all input parameters
- **Proper validation** to ensure git user identity is never empty
- **Debug logging** to show git configuration values
- **Robust error handling** with clear error messages

### Default Values Now Include:
```bash
git-user-name: "github-actions[bot]"
git-user-email: "github-actions[bot]@users.noreply.github.com" 
command: "build"
build-dir: "dist"
commit-message: "chore: update build files"
node-version: "20"
build-only: "false"
```

## 🚀 Usage

### Minimal Setup (Now Works!)
```yaml
- name: Build and Deploy  
  uses: miguelcolmenares/npm-auto-build@v2
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    # No other inputs required - all have sensible defaults!
```

### With Custom Git Identity (Optional)
```yaml
- name: Build and Deploy
  uses: miguelcolmenares/npm-auto-build@v2
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    git-user-name: "My Bot"
    git-user-email: "bot@mycompany.com"
```

## 🔧 Enhanced Logging

The action now shows debug information to help troubleshoot any issues:

```
[INFO] Configuring Git...
[INFO] Git user name: 'github-actions[bot]'
[INFO] Git user email: 'github-actions[bot]@users.noreply.github.com'
[INFO] Build directory is in .gitignore - will force add files
[INFO] Adding build files to Git...
[INFO] Used --force to add ignored files
[INFO] Found staged changes. Proceeding to commit...
[INFO] Committing changes...
[INFO] Pushing changes to repository...
✅ Build completed and committed successfully!
```

## 📋 Complete Fix History

### v2.0.2 (This Release)
- **Fixed**: "Author identity unknown" git configuration error
- **Added**: Fallback defaults for all input parameters  
- **Enhanced**: Debug logging and input validation

### v2.0.1 (Previous)
- **Fixed**: .gitignore directories not being committed (like `dist/`)
- **Added**: `--force` flag for ignored files
- **Enhanced**: Git status logging

### v2.0.0 (Initial)
- **First functional release** (v1.x had Docker issues)
- **Complete rewrite** with proper bash support
- **Node.js 20** Alpine Linux base

## 🏷️ Version Tags

- **`@v2.0.2`** - Specific version with both fixes
- **`@v2`** - Auto-updating tag (now points to v2.0.2)

## ⚡ Impact

### Before v2.0.2:
```yaml
# Required ALL these inputs to work:
- uses: miguelcolmenares/npm-auto-build@v2.0.1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    git-user-name: "github-actions[bot]"
    git-user-email: "github-actions[bot]@users.noreply.github.com"
    command: "build"
    build-dir: "dist"
```

### After v2.0.2:
```yaml
# Only github-token is required:
- uses: miguelcolmenares/npm-auto-build@v2
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

## 🔄 Migration

### If Using `@v2` (Recommended)
✅ **No action needed** - you automatically get this fix!

### If Using Specific Version
Update from `@v2.0.1` to `@v2.0.2` or better yet, use `@v2`

## 🎯 Perfect For

- ✅ **Simple workflows** with minimal configuration
- ✅ **Repositories with `dist/` in .gitignore**  
- ✅ **Teams wanting zero-config automation**
- ✅ **Projects using default npm build scripts**

---

**This release makes the action truly zero-configuration** - just provide your GitHub token and it handles everything else with sensible defaults!