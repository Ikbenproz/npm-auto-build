# NPM Auto Build Action

**Automatically build and commit your npm project's build files to your repository**

## What it does

This action runs your npm build script and automatically commits the generated build files to your repository, keeping your source code clean while maintaining compiled assets for deployment.

## Key benefits

- ✨ **Automated builds** - Never forget to build before release
- 🧹 **Clean repository** - Keep source and build files separate  
- 🔄 **Consistent builds** - Always built in the same CI environment
- ⚡ **Zero configuration** - Works out of the box with sensible defaults
- 📦 **Smart detection** - Automatically finds your build directory
- 🛠️ **Flexible** - Configurable for any npm project

## Perfect for

- React, Vue, Angular applications
- TypeScript libraries  
- Static site generators (Gatsby, Next.js)
- Documentation sites
- Any npm project with build output

## Quick start

```yaml
- name: Auto Build
  uses: miguelcolmenares/npm-auto-build@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

That's it! The action will automatically detect your build script and directory, run the build, and commit any changes.