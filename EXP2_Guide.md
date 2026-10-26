# Experiment 2: CI/CD Pipelines Using GitHub Actions and Jenkins

## 2.1 Objective

Create a Node.js project with Jest tests, automate CI using GitHub Actions, and automate the build and test process using a Jenkins Pipeline.

## 2.2 Prerequisites

Install:

- Node.js
- npm
- Git
- Jenkins
- GitHub account

### Verify Node.js and npm

```powershell
node --version
npm --version
```

### Install Jenkins on Windows

Download Jenkins:

```text
https://www.jenkins.io/download/windows/
```

Install it using the Windows installer. Start Jenkins and open:

```text
http://localhost:8080
```

Complete the initial setup:

1. Unlock Jenkins using the password displayed by the installer.
2. Install the suggested plugins.
3. Create the first administrator account.
4. Keep the Jenkins URL as:

   ```text
   http://localhost:8080
   ```

   For a public GitHub repository, you will later need a public Jenkins URL, a Jenkins controller running on a public host, or a locally installed GitHub Actions alternative.

---

## 2.3 Create the Node.js Project

Open PowerShell:

```powershell
cd C:\Users\Documents
mkdir node-ci-demo
cd node-ci-demo
```

Initialize Git:

```powershell
git init
git branch -M main
```

Initialize the npm package:

```powershell
npm init -y
```

Install Jest as a development dependency:

```powershell
npm install --save-dev jest
```

Verify that `package.json` and `package-lock.json` were created:

```powershell
Get-ChildItem
```

---

## 2.4 Create the Test and Application Files

### Create `sum.js`

```powershell
@"
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

module.exports = { add, subtract };
"@ | Set-Content -Encoding UTF8 sum.js
```

### Create the Test File

Create the `tests` directory:

```powershell
mkdir tests
```

Create `tests/sum.test.js`:

```powershell
@"
const { add, subtract } = require('../sum');

describe('addition', () => {
  test('adds two positive numbers', () => {
    expect(add(4, 6)).toBe(10);
  });

  test('adds a negative number', () => {
    expect(add(-3, 5)).toBe(2);
  });
});

describe('subtraction', () => {
  test('subtracts two numbers', () => {
    expect(subtract(10, 4)).toBe(6);
  });
});
"@ | Set-Content -Encoding UTF8 tests/sum.test.js
```

### Add a Test Script

Open `package.json`:

```powershell
notepad package.json
```

Add this script under `scripts`:

```json
"scripts": {
  "test": "jest --coverage",
  "build": "mkdir build && copy sum.js build\\sum.js"
}
```

A complete example is:

```json
{
  "name": "node-ci-demo",
  "version": "1.0.0",
  "description": "Node.js CI/CD demonstration project",
  "main": "index.js",
  "scripts": {
    "test": "jest --coverage",
    "build": "mkdir build && copy sum.js build\\sum.js"
  },
  "keywords": [],
  "author": "",
  "license": "MIT",
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

### Optional: Add `.gitignore`

```powershell
@"
node_modules/
coverage/
build/
.env
"@ | Set-Content -Encoding UTF8 .gitignore
```

---

## 2.5 Run the Tests Locally

Run:

```powershell
npm test
```

Expected result:

```text
Test Suites: 1 passed
Tests:       3 passed
```

Run the build command locally:

```powershell
npm run build
```

Verify the build directory:

```powershell
Get-ChildItem build
```

---

## 2.6 Create and Push the GitHub Repository

### Create the Repository Manually

1. Go to [https://github.com/new](https://github.com/new).
2. Enter the repository name:

   ```text
   node-ci-demo
   ```

3. Select `Public` or `Private`.
4. Do not initialize it with a README, `.gitignore`, or a license.
5. Click **Create repository**.

### Add the Remote and Push

```powershell
git remote add origin https://github.com/your-username/node-ci-demo.git
git add .
git commit -m "Initial Node.js CI project"
git push -u origin main
```

If GitHub requests authentication, enter your username and a personal access token.

---

## 2.7 Create the GitHub Actions Workflow

Create the workflow directory:

```powershell
mkdir .github
mkdir .github/workflows
```

Create the workflow file:

```powershell
@"
name: Node.js CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:

jobs:
  build-and-test:
    name: Build and Test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
"@ | Set-Content -Encoding UTF8 .github/workflows/ci.yml
```

Verify the file exists:

```powershell
Get-Content .github/workflows/ci.yml
```

Commit and push it:

```powershell
git add .
git commit -m "Add GitHub Actions CI workflow"
git push
```

---

## 2.8 Run and Verify GitHub Actions

### Manual Procedure

1. Open the GitHub repository.
2. Select the **Actions** tab.
3. Select the **Node.js CI** workflow.
4. Select the latest run.
5. Open the `build-and-test` job.
6. Verify that these steps pass:
   - Checkout source code
   - Set up Node.js
   - Install dependencies
   - Run tests
7. Click each step to view the console output.
8. Click **Capture screenshot** or take a screenshot showing:
   - The workflow name
   - Successful status
   - All completed steps

### Trigger Manually

In the Actions tab:

1. Select **Node.js CI**.
2. Click **Run workflow**.
3. Select the `main` branch.
4. Click **Run workflow**.

A new run should begin automatically.

---

## 2.9 Configure Jenkins for GitHub

### Recommended Setup

For a public repository, Jenkins running locally can use:

- A GitHub personal access token
- Multibranch Pipeline
- GitHub webhook integration, if Jenkins is accessible

If Jenkins is running locally, GitHub cannot normally reach `localhost`. Therefore, either:

- Use only the GitHub token with periodic branch scanning, or
- Expose Jenkins using a secure tunneling/hosting service, or
- Use a cloud-based Jenkins controller.

### Create a GitHub Personal Access Token

In GitHub:

1. Open **Settings**.
2. Open **Developer settings**.
3. Open **Personal access tokens**.
4. Select **Tokens (classic)**.
5. Generate a token.
6. Select the repository scope, such as `repo` for private repositories.
7. Copy and securely store the token.

Do not paste the token into screenshots or commit it to Git.

### Add Credentials to Jenkins

1. Open Jenkins.
2. Select **Manage Jenkins**.
3. Select **Credentials**.
4. Select the appropriate credential store.
5. Click **Add credentials**.
6. Choose:
   - Kind: **Username with password**
   - Username: GitHub username
   - Password: GitHub personal access token
7. Give it a recognizable ID, such as:

   ```text
   github-node-ci
   ```

8. Save the credential.

---

## 2.10 Create the Jenkins Pipeline Job

### Recommended: Multibranch Pipeline

1. Open Jenkins.
2. Select **New Item**.
3. Enter:

   ```text
   node-ci-pipeline
   ```

4. Select **Multibranch Pipeline**.
5. Click **OK**.
6. Under **Branch Sources**, click **Add Branch Source**.
7. Select **GitHub**.
8. Configure:
   - API endpoint:

     ```text
     https://api.github.com
     ```

   - Credentials: `github-node-ci`
   - Repository owner: your GitHub username
   - Repository name: `node-ci-demo`
9. Click **Save**.
10. Click **Scan Multibranch Pipeline Now**.

Jenkins should discover the `main` branch and create a branch job.

### Alternative: Pipeline Job

1. Select **New Item**.
2. Select **Pipeline**.
3. Name it `node-ci-pipeline`.
4. Under **Pipeline**, select:
   - `Pipeline script from SCM`, or
   - `Pipeline script` for a quick demonstration.
5. Enter your GitHub repository URL.
6. Select the GitHub credential.
7. Set the branch to:

   ```text
   */main
   ```

8. Save.

For the SCM-based option, the `Jenkinsfile` must be committed to the repository.

---

## 2.11 Create the `Jenkinsfile`

Create the file in the project root:

```powershell
@"
pipeline {
    agent any

    environment {
        NODE_VERSION = '20'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-node-ci',
                    url: 'https://github.com/your-username/node-ci-demo.git'
            }
        }

        stage('Install') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
                archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully.'
        }

        failure {
            echo 'CI/CD pipeline failed. Check the console output.'
        }
    }
}
"@ | Set-Content -Encoding UTF8 Jenkinsfile
```

### Important Windows Jenkins Note

If the Jenkins controller runs on Windows, the `sh` step may not be available unless the appropriate plugins are installed. In that case, use `bat` steps:

```powershell
@"
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-node-ci',
                    url: 'https://github.com/your-username/node-ci-demo.git'
            }
        }

        stage('Install') {
            steps {
                bat 'node --version'
                bat 'npm --version'
                bat 'npm ci'
            }
        }

        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
                archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
            }
        }
    }
}
"@ | Set-Content -Encoding UTF8 Jenkinsfile
```

Commit and push:

```powershell
git add Jenkinsfile
git commit -m "Add Jenkins CI/CD pipeline"
git push
```

If the Jenkins job is a simple Pipeline job with `Pipeline script from SCM`, it will detect the updated `Jenkinsfile`. If it is a multibranch project, run **Scan Multibranch Pipeline Now** again.

---

## 2.12 Execute and Verify the Jenkins Pipeline

1. Open the Jenkins project.
2. Select **Build Now** or the branch build.
3. Select the build number.
4. Select **Console Output**.
5. Verify the stages:
   - Checkout
   - Install
   - Test
   - Build
6. Confirm that the build is marked with a blue/success indicator.
7. Take a screenshot showing:
   - Stage names
   - `SUCCESS`
   - Successful console output

### Troubleshooting

#### `npm ci` cannot find `package-lock.json`

Run:

```powershell
npm install
git add package-lock.json
git commit -m "Add npm lock file"
git push
```

#### `jest` is not recognized

Run:

```powershell
npm install --save-dev jest
npm test
```

Commit the updated `package.json` and lock file.

#### Permission Denied

Verify that the repository URL and credential ID are correct. For public repositories, a token may not be necessary, but using one is still reliable.

#### Git Is Not Recognized by Jenkins

Ensure Git is installed and available in Jenkins's `PATH`. Restart Jenkins after installation if necessary.

#### `sh` Command Is Not Recognized

Install the relevant shell/agent plugins or replace `sh` with `bat` on a Windows Jenkins controller.

---

## 2.13 Comparison: GitHub Actions and Jenkins

| Feature | GitHub Actions | Jenkins |
|---|---|---|
| Hosting | Integrated with GitHub | Usually self-hosted; also available in the cloud |
| Setup | Configuration is mostly in the repository | Requires controller installation and configuration |
| Pipeline definition | `.github/workflows/*.yml` | `Jenkinsfile` |
| Plugins/extensions | Marketplace actions and reusable workflows | Large plugin ecosystem |
| Credentials | GitHub repository or organization secrets | Jenkins Credentials Store |
| Triggering | Native GitHub events and schedules | Webhooks, polling, plugins, and other triggers |
| Maintenance | Less infrastructure maintenance | Server, plugins, agents, and upgrades require maintenance |
| Cloud agents | GitHub-hosted runners | Cloud or self-hosted Jenkins agents |
| Best fit | Projects already hosted on GitHub | Complex pipelines, private infrastructure, and multi-platform environments |

---

## 2.14 Conclusion

The experiment demonstrated two approaches to CI/CD automation for a Node.js project. GitHub Actions automatically checked out the code, installed Node.js and dependencies, and ran Jest tests directly from a workflow stored in the repository. Jenkins performed similar build and test stages using a `Jenkinsfile` and provided greater infrastructure control and extensibility.

---