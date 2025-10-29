# 🧪 Testing NPM Auto Build Action Locally

This guide shows you different ways to test the GitHub Action locally before publishing it.

## ✅ Available Testing Methods

### 1. 🐳 **Docker Testing (Recommended)**

This is the simplest and most direct method:

```bash
# 1. Build the Docker image
docker build -t npm-auto-build-test .

# 2. Create a test project
mkdir test-project && cd test-project
npm init -y
echo '{"scripts": {"build": "mkdir -p dist && echo \"console.log('Hello');\" > dist/main.js"}}' > package.json

# 3. Run the action
docker run --rm \
  -v "$(pwd):/github/workspace" \
  -e INPUT_COMMAND="build" \
  -e INPUT_DIRECTORY="." \
  -e INPUT_BUILD_DIR="dist" \
  -e INPUT_COMMIT_MESSAGE="test: automated build" \
  -e INPUT_GITHUB_TOKEN="" \
  -e INPUT_GIT_USER_NAME="Test User" \
  -e INPUT_GIT_USER_EMAIL="test@example.com" \
  npm-auto-build-test
```

### 2. 🎯 **Automated Script Testing**

Use the included testing script:

```bash
# Give execution permissions
chmod +x test.sh

# Run the test
./test.sh

# Test with custom parameters
./test.sh "build:prod" "build" "./my-project"
```

### 3. 🏃 **Testing with Act (GitHub Actions locally)**

Install `act` to simulate GitHub Actions completely:

```bash
# Install act
brew install act  # On macOS
# or
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash  # On Linux

# Run in a repository with workflow
act push
```

### 4. 🔧 **Manual Step-by-Step Testing**

For detailed debugging:

```bash
# 1. Create test project
mkdir my-test-app && cd my-test-app
git init
npm init -y

# 2. Add build script
echo '{
  "name": "test-app",
  "scripts": {
    "build": "mkdir -p dist && echo \"Built at: $(date)\" > dist/build-info.txt"
  }
}' > package.json

# 3. Initial commit
git add . && git commit -m "Initial commit"

# 4. Run action
docker run --rm \
  -v "$(pwd):/github/workspace" \
  -e INPUT_COMMAND="build" \
  -e INPUT_BUILD_DIR="dist" \
  -e INPUT_COMMIT_MESSAGE="chore: update build" \
  -e INPUT_GITHUB_TOKEN="" \
  npm-auto-build-test
```

## 🔍 **Environment Variables for Testing**

The action accepts these environment variables for configuration:

```bash
INPUT_COMMAND="build"                    # npm script to execute
INPUT_DIRECTORY="."                      # Project directory  
INPUT_BUILD_DIR="dist"                   # Build directory
INPUT_COMMIT_MESSAGE="chore: build"      # Commit message
INPUT_GITHUB_TOKEN=""                    # Token (empty for local tests)
INPUT_GIT_USER_NAME="Test User"          # Git user name
INPUT_GIT_USER_EMAIL="test@example.com"  # Git email
INPUT_NODE_VERSION="18"                  # Node.js version
```

## 📋 **Testing Checklist**

- [ ] ✅ Action builds correctly with Docker
- [ ] 🔍 Detects `package.json` correctly
- [ ] 📦 Installs dependencies (npm/yarn)
- [ ] 🏗️ Executes specified build command
- [ ] 📁 Finds build directory (auto-detection)
- [ ] 🔧 Configures Git correctly
- [ ] 🔍 Detects changes in build files
- [ ] ✨ Commits only when there are changes
- [ ] 📝 Uses custom commit message
- [ ] 🚫 Doesn't fail if no changes
- [ ] 🔍 Clear and useful logging

## 🐛 **Troubleshooting**

### Error: "no such file or directory"

```bash
# Rebuild image without cache
docker build --no-cache -t npm-auto-build-test .
```

### Error: "Build directory not found"

```bash
# Specify correct directory
-e INPUT_BUILD_DIR="build"  # for React
-e INPUT_BUILD_DIR="public" # for Gatsby
-e INPUT_BUILD_DIR="lib"    # for TypeScript libraries
```

### Error: "No changes detected"

This is normal if the build generates the same content. To test with changes:

```bash
# Use timestamp in the build
echo '{"scripts": {"build": "mkdir -p dist && echo \"Built: $(date)\" > dist/main.js"}}' > package.json
```

## 📊 **Expected Results**

### ✅ **Successful Execution**

```log
[INFO] Starting NPM Auto Build Action
[INFO] Found package.json, proceeding with build
[INFO] Installing dependencies...
[INFO] Running build command: npm run build
[INFO] Adding build files to Git...
[INFO] Committing changes...
[INFO] ✅ Build completed and committed successfully!
```

### ⚠️ **No Changes (Normal)**

```log
[INFO] No changes detected in build directory. Nothing to commit.
```

### ❌ **Common Error**

```log
[ERROR] package.json not found in /github/workspace
[ERROR] Build script 'build' not found in package.json
```

## 🎯 **Suggested Test Cases**

1. **React Project**: `create-react-app` with `npm run build`
2. **Vue Project**: Vue CLI with `npm run build`  
3. **TypeScript Library**: With build to `lib/` or `dist/`
4. **Monorepo**: Multiple packages with separate builds
5. **Project with Yarn**: Verify automatic detection
6. **Build without changes**: Verify no unnecessary commits

## 🚀 **Testing in CI/CD**

For testing in your own CI/CD:

```yml
name: Test Action
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Test action
        uses: ./
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## 🛠️ **Advanced Testing Scripts**

Additional testing scripts are available in the `scripts/` folder:

- `scripts/test-debug.sh` - Detailed debugging version
- `scripts/test-local.sh` - Original local testing script  
- `scripts/Dockerfile.debug` - Debug Docker configuration

Use these for advanced testing scenarios or when contributing to the project.