# Day 26 of #90DaysOfDevOps – Managing GitHub Without Leaving the Terminal Using GitHub CLI

## Introduction

As developers and DevOps engineers, we spend a lot of time switching between the terminal and GitHub's web interface. Creating repositories, managing issues, reviewing pull requests, and monitoring workflows often requires opening a browser, navigating through multiple pages, and breaking our workflow.

That's where **GitHub CLI (`gh`)** comes in.

GitHub CLI provides a powerful command-line interface that allows us to interact with GitHub directly from the terminal. From creating repositories to managing pull requests and workflows, almost everything can be done without leaving the command line.

Today I explored GitHub CLI and learned how it can simplify day-to-day GitHub operations and improve productivity.

---

# What is GitHub CLI?

GitHub CLI (`gh`) is an official tool from GitHub that brings GitHub functionality directly into your terminal.

Instead of:

1. Opening a browser
2. Navigating to GitHub
3. Creating repositories manually
4. Opening Pull Requests
5. Checking workflow status

You can do everything using simple commands.

---

# Installing GitHub CLI

The first step was installing GitHub CLI.

### Ubuntu

```bash
sudo apt update
sudo apt install gh -y
```

### macOS

```bash
brew install gh
```

Verify installation:

```bash
gh --version
```

This confirmed that GitHub CLI was successfully installed.

---

# Authenticating with GitHub

After installation, I authenticated my GitHub account.

```bash
gh auth login
```

GitHub CLI supports multiple authentication methods:

* Browser-based OAuth Login
* Personal Access Tokens (PAT)
* SSH Authentication

To verify authentication:

```bash
gh auth status
```

This displayed the currently logged-in GitHub account and authentication status.

---

# Creating a Repository from the Terminal

One of the most impressive features was creating repositories directly from the command line.

```bash
gh repo create day26-gh-demo \
  --public \
  --clone \
  --add-readme
```

This command:

* Created a repository on GitHub
* Made it public
* Added a README
* Cloned it locally

All in a single command.

---

# Managing Repositories

View repository details:

```bash
gh repo view
```

List all repositories:

```bash
gh repo list
```

Open repository in browser:

```bash
gh repo view --web
```

Delete repository:

```bash
gh repo delete <repo-name>
```

Repository management becomes significantly faster when done from the terminal.

---

# Working with Issues

GitHub Issues are commonly used for bug tracking, feature requests, and task management.

Creating an issue:

```bash
gh issue create
```

Listing open issues:

```bash
gh issue list
```

Viewing a specific issue:

```bash
gh issue view 1
```

Closing an issue:

```bash
gh issue close 1
```

### Real-World Automation Use Case

Imagine a monitoring script detecting server downtime.

The script could automatically create a GitHub issue:

```bash
gh issue create \
  --title "Server Down" \
  --body "Production server is unreachable."
```

This makes issue management automation-friendly.

---

# Creating Pull Requests from the Terminal

Pull Requests are a core part of collaborative development.

First, I created a feature branch:

```bash
git switch -c feature-update
```

Made changes and pushed:

```bash
git push -u origin feature-update
```

Created a Pull Request:

```bash
gh pr create --fill
```

The `--fill` flag automatically generates the title and description from commit messages.

---

# Managing Pull Requests

List open PRs:

```bash
gh pr list
```

View PR details:

```bash
gh pr view
```

Merge PR:

```bash
gh pr merge --merge
```

GitHub CLI supports multiple merge methods:

```bash
gh pr merge --merge
gh pr merge --squash
gh pr merge --rebase
```

This means entire Pull Request workflows can be handled without opening GitHub in a browser.

---

# GitHub Actions Preview

GitHub CLI also provides access to workflow runs.

List workflow executions:

```bash
gh run list
```

View workflow details:

```bash
gh run view <RUN_ID>
```

This becomes extremely useful once CI/CD pipelines are introduced.

Instead of opening GitHub Actions pages repeatedly, workflow information can be checked instantly from the terminal.

---

# Useful GitHub CLI Commands

## GitHub API

```bash
gh api user
```

Returns information directly from GitHub's API.

---

## Search Repositories

```bash
gh search repos devops
```

Search GitHub repositories from the terminal.

---

## Create a Gist

```bash
gh gist create notes.txt
```

Share files quickly as GitHub Gists.

---

## Create Releases

```bash
gh release create v1.0.0
```

Useful for versioned application deployments.

---

## Create Aliases

```bash
gh alias set prs "pr list"
```

Now:

```bash
gh prs
```

works as a shortcut for:

```bash
gh pr list
```

---

# Commands Learned Today

```bash
gh auth login

gh auth status

gh repo create

gh repo view

gh repo list

gh issue create

gh issue list

gh issue close

gh pr create

gh pr list

gh pr merge

gh run list

gh api

gh gist create

gh release create

gh alias set

gh search repos
```

---

# Key Learnings

### 1. GitHub CLI Reduces Context Switching

Most GitHub tasks can be completed without opening a browser.

### 2. Repository Management Becomes Faster

Repositories can be created, viewed, cloned, and deleted directly from the terminal.

### 3. Pull Requests Can Be Fully Managed via CLI

Creating, reviewing, and merging Pull Requests becomes much more efficient.

### 4. GitHub CLI is Automation-Friendly

Commands can easily be integrated into Bash scripts, CI/CD pipelines, and DevOps workflows.

### 5. GitHub CLI is a Powerful DevOps Tool

As automation increases, terminal-based GitHub management becomes extremely valuable.

---

# Conclusion

Day 26 introduced me to GitHub CLI, a tool that significantly improves productivity by bringing GitHub functionality directly into the terminal.

From repository management and issue tracking to Pull Requests and workflow monitoring, GitHub CLI provides a streamlined way to work with GitHub without constantly switching to a browser.

For DevOps engineers, this tool is especially useful because it integrates naturally with automation scripts, CI/CD pipelines, and infrastructure workflows.

Day 26 completed. 🚀
