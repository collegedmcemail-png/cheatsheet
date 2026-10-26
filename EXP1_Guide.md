# DevOps Practical Experiments

## Complete Step-by-Step Guide

This guide covers four practical experiments:

1. Version Control Operations Using Git
2. CI/CD Pipelines Using GitHub Actions and Jenkins
3. Scrum-Based Agile Project Management Using Jira
4. Containerizing Applications Using Docker

The commands and procedures are suitable for **Windows 10/11 with PowerShell**, unless otherwise stated.

---

# Experiment 1: Version Control Operations Using Git

## 1.1 Objective

Create a local Git repository for a project named **Student Management System**, perform basic version-control operations, connect it to GitHub, and demonstrate branching and merging.

## 1.2 Prerequisites

Make sure the following tools are available:

- Git
- GitHub account
- Internet connection
- Visual Studio Code, optionally
- GitHub Desktop, optionally

## 1.3 Install and Verify Git

### Download Git

Download Git for Windows from:

```text
https://git-scm.com/download/win
```

During installation, accept the default options.

### Verify Git

Open PowerShell and run:

```powershell
git --version
```

Expected result:

```text
git version 2.x.x
```

### Configure Git Username and Email

```powershell
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Check the configuration:

```powershell
git config --global --list
```

---

## 1.4 Create the Project

Navigate to the required parent directory:

```powershell
cd C:\Users\Documents
```

Create the project directory:

```powershell
mkdir "Student Management System"
cd "Student Management System"
```

Confirm the current directory:

```powershell
Get-Location
```

Expected path:

```text
C:\Users\Documents\Student Management System
```

---

## 1.5 Initialize the Local Git Repository

Run:

```powershell
git init
```

Expected output:

```text
Initialized empty Git repository in C:/Users/Documents/Student Management System/.git/
```

Verify repository status:

```powershell
git status
```

The output should indicate that you are on the default branch, usually `master` or `main`.

---

## 1.6 Create `student.txt`

Create the file using Notepad:

```powershell
notepad student.txt
```

Enter the following sample information:

```text
Student Management System

Student ID: 101
Name: Rahul Sharma
Course: B.Tech Computer Science
Semester: 3

Student ID: 102
Name: Priya Patel
Course: B.Tech Computer Science
Semester: 3
```

Save the file and close Notepad.

Alternatively, create the file with PowerShell:

```powershell
@"
Student Management System

Student ID: 101
Name: Rahul Sharma
Course: B.Tech Computer Science
Semester: 3

Student ID: 102
Name: Priya Patel
Course: B.Tech Computer Science
Semester: 3
"@ | Set-Content -Encoding UTF8 student.txt
```

Display the file:

```powershell
Get-Content student.txt
```

---

## 1.7 Stage and Commit the File

### Check the Current Status

```powershell
git status
```

The file should appear as an untracked file.

### Stage the File

```powershell
git add student.txt
```

Or stage all files:

```powershell
git add .
```

Verify staging:

```powershell
git status
```

### Commit the File

```powershell
git commit -m "Initial commit"
```

Verify the commit:

```powershell
git log --oneline
```

Expected output should include:

```text
<commit-hash> Initial commit
```

---

## 1.8 Set the Main Branch

If the current branch is named `master`, rename it to `main`:

```powershell
git branch -M main
```

Verify:

```powershell
git branch
```

Expected output:

```text
* main
```

---

## 1.9 Create a Remote Repository on GitHub

### Manual Procedure

1. Sign in to [https://github.com](https://github.com).
2. Click the `+` icon in the upper-right corner.
3. Select **New repository**.
4. Enter the repository name:

   ```text
   Student Management System
   ```

   You may also use:

   ```text
   Student-Management-System
   ```

5. Under **Repository visibility**, choose:
   - `Public` for an open repository, or
   - `Private` for a private repository.
6. Under **Initialize this repository with**, select:
   - **Add a README file**, if desired, or
   - **Add a `.gitignore`**, or
   - Leave all initialization options unchecked.
7. Click **Create repository**.

### Important Note

If you selected **Add a README file** during repository creation, GitHub creates a separate commit on the remote `main` branch. Because the local and remote histories are initially different, a direct push may be rejected.

The simplest solution for this experiment is to leave the remote repository uninitialized, or follow the remote-history merge procedure below.

---

## 1.10 Link the Local Repository to GitHub

Copy the HTTPS repository URL from GitHub. It will look like:

```text
https://github.com/your-username/Student-Management-System.git
```

Add it as the remote:

```powershell
git remote add origin https://github.com/your-username/Student-Management-System.git
```

Verify the remote:

```powershell
git remote -v
```

Expected output:

```text
origin  https://github.com/your-username/Student-Management-System.git (fetch)
origin  https://github.com/your-username/Student-Management-System.git (push)
```

You can also change the remote URL later:

```powershell
git remote set-url origin https://github.com/your-username/Student-Management-System.git
```

---

## 1.11 Push the Commit to GitHub

Push the `main` branch:

```powershell
git push -u origin main
```

The `-u` option sets `main` as the upstream tracking branch.

Verify the tracking configuration:

```powershell
git branch -vv
```

Open the GitHub repository in a browser. The `student.txt` file and the **Initial commit** message should be visible.

### Authentication

If Git requests credentials:

- Use a GitHub username and a personal access token as the password, or
- Use GitHub Credential Manager, VS Code authentication, or SSH authentication.

A personal access token can be created from:

```text
GitHub → Settings → Developer settings → Personal access tokens
```

---

## 1.12 Handle a Remote README Commit

If `git push` produces a rejected non-fast-forward error, use one of the following approaches.

### Recommended for a Simple Experiment: Push with Force

Use this only if the remote repository contains no work that must be preserved:

```powershell
git push -u origin main --force
```

### Alternative: Merge the Remote History

Fetch the remote changes:

```powershell
git pull origin main --rebase --allow-unrelated-histories
```

Resolve any conflict if necessary, then push:

```powershell
git push -u origin main
```

---

## 1.13 Create the `feature-update` Branch

Create and switch to it:

```powershell
git checkout -b feature-update
```

Verify:

```powershell
git branch
```

Expected output:

```text
* feature-update
  main
```

---

## 1.14 Modify `student.txt`

Open the file:

```powershell
notepad student.txt
```

Append two additional student records:

```text
Student ID: 103
Name: Aman Verma
Course: B.Tech Information Technology
Semester: 2

Student ID: 104
Name: Sneha Reddy
Course: B.Tech Computer Science
Semester: 4
```

Save and close the file.

Alternatively, append using PowerShell:

```powershell
@"

Student ID: 103
Name: Aman Verma
Course: B.Tech Information Technology
Semester: 2

Student ID: 104
Name: Sneha Reddy
Course: B.Tech Computer Science
Semester: 4
"@ | Add-Content -Encoding UTF8 student.txt
```

View the updated file:

```powershell
Get-Content student.txt
```

Check the changes:

```powershell
git status
git diff
```

---

## 1.15 Commit the Changes on `feature-update`

Stage the file:

```powershell
git add student.txt
```

Commit it:

```powershell
git commit -m "Add additional student records"
```

Verify:

```powershell
git log --oneline
```

---

## 1.16 Merge `feature-update` into `main`

Switch to `main`:

```powershell
git checkout main
```

Verify that `main` does not yet contain the new records:

```powershell
Get-Content student.txt
```

Merge the feature branch:

```powershell
git merge feature-update
```

A successful output may state:

```text
Updating ...
Fast-forward
```

Verify the merged content:

```powershell
Get-Content student.txt
```

Verify the commit history:

```powershell
git log --oneline --graph --decorate --all
```

Push the merged result:

```powershell
git push origin main
```

The GitHub repository should now contain all four student records.

### If a Merge Conflict Occurs

Inspect the conflicted files:

```powershell
git status
```

Open the file:

```powershell
notepad student.txt
```

Remove conflict markers such as:

```text
<<<<<<< HEAD
=======
>>>>>>> feature-update
```

Keep the correct content, save the file, and complete the merge:

```powershell
git add student.txt
git commit -m "Resolve merge conflict"
```

---

## 1.17 Display the Complete Commit History

Use the detailed history:

```powershell
git log
```

Use a compact graph:

```powershell
git log --oneline --graph --decorate --all
```

Show file changes in each commit:

```powershell
git log --stat
```

Show the history for `student.txt`:

```powershell
git log --oneline -- student.txt
```

---

## 1.18 Optional GitHub Verification

In the GitHub repository:

1. Open the **Commits** tab.
2. Confirm that both commits are visible.
3. Open `student.txt`.
4. Confirm that all four student records are present.
5. Open the branch list.
6. Confirm that both `main` and `feature-update` exist.

---

## 1.19 Conclusion

The experiment demonstrated how Git can be used to initialize and manage a Student Management System project. The `student.txt` file was tracked, staged, and committed with meaningful history. A feature branch was created to add additional records and later merged into the `main` branch. GitHub provided a remote collaboration and backup platform, while the commit log made the project's development history easy to understand.

---
