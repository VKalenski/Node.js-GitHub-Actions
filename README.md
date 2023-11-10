# GitHub-Actions

git init
npm install
npm update
npm audit fix
npm run dev
npm test
npm run build

---
git add remote https://vilislavkalenski@github.com/...
git remote set-url origin https://...@gutgub
git push --set-upstream origin main
---

**Links**

https://github.com/marketplace/actions/checkout
https://github.com/actions/checkout

https://github.com/actions/cache

https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners#supported-software
https://github.com/actions/runner-images/blob/main/images/linux/Ubuntu2204-Readme.md

---

### **GitHub Actions Alternatives**

- Jenkins
- GitLab CI/CD
- Azure Pipelines
- AWS CodePipeline

---

# **What Is Git?**

A (free) version control system
A tool for managing source code changes

- Save code snapshots ("commits")
- Work with alternative code versions (“branches”)
- Move between branches & commits

With Git, you can easily roll back to older code snapshots or develop new features without breaking production code.

---

# **What Is GitHub?**

A cloud Git repository & services provider
Store & manage Git repositories

Cloud Git repository storage (“push” & “pull”)
- Backup, work across machines & work in teams
- Public & private, team management & more

Code management & collaborative development
- Via “Issues”, “Projects”, “Pull Requests” & more

Automation & CI / CD
- Via GitHub Actions, GitHub Apps & more

---

# **Git Repositories**

Git features can be used in projects with Git repositories

1. **A repository is a folder used by Git to track all changes of a given project**
- Git commands require a 
repository in a project
2. **Create Git repositories via git init**
- Only required once per 
folder / project
3. **Some projects initialize Git for you**
- e.g., React projects

Cloud storage for your Git repositories (public/private) - Backup, cross-device usage, collaboration
- “Connected” to local Git repositories via git remote add
- Local commits are uploaded via git push
- Remote commits are downloaded via git pull

---

# Working with Commits (Code Snapshots)

Create Commits
git add <file(s)>
Stage changes for next commi
+
git commit
Create a commit that includes all staged changes

Move between Commits
git checkout <id>
Temporarily move to another commit

Undo Commits
git revert <id>
Revert changes of commit by creating a new commit

git reset --hard <id>
Undo changes by deleting all commits since <id>


---

**Understanding Staging**

Staging controls which changes are part of a commit
- With staging, you can make sure that not all code changes made are added to a snapshot
- If all changes should be included, you can use git add . to include all files in a Git repository

---

# **Key Commands**

git init - Initialize a Git repository (only required once per project)
git add <file(s)> - Stage code changes (for the next commit)
git commit -m “…” - Create a commit for the staged changes (with a message)
git status - Get the current repository status (e.g., which changes are staged)
git log - Output a chronologically ordered list of commits
git checkout <id> - Temporarily move back to commit <id>
git revert <id> - Revert the changes of commit <id> (by creating a new commit)
git reset <id> - Undo commit(s) up to commit <id> by deleting commits

---

# Forking & Pull Requests

1. **Repository Forking**
- Creates a standalone copy of a repository
- Can be used to work on code without affecting the original repository

2. **Pull Requests**
- Requests merging code changes into a branch
- Can be based on a forked repository or another branch from the same repository
- Pull requests allow for code reviews before merging changes

---

# GitHub Actions: Fundamentals

### Key Elements

1. **Workflows**
- Attached to a GitHub repository
- Contain one or more Jobs
- Triggered upon Events
2. **Jobs**
- Define a Runner (execution environment)
- Contain one or more Steps
- Run in parallel (default) or sequential
- Can be conditional
3. **Steps**
- Execute a shell script or an Action
- Can use custom or third-party actions
- Steps are executed in order
- Can be conditional

---

### What Are Actions?

Command(“run”)
A (typically simple) shell command that’s defined by you

**Action**
- A (custom) application that performs a (typically complex) frequently repeated task
- You can build your own Actions but you can also use official or community Actions

---

**Event**
e.g., push, pull_request

---

A Note About Fork Pull Request Workflows

By default, Pull Requests based on Forks do NOT trigger a workflow
- Reason: Everyone can fork & open pull requests
- Malicious workflow runs & excess cost could be caused

First-time contributors must be approved manually

---

### Cancelling & Skipping Workflow Runs

**Cancelling**
- By default, Workflows get cancelled if Jobs fail
- By default, a Job fails if at least one Step fails
- You can also cancel workflows manually

**Skipping**
- By default, all matching events start a workflow
- Exceptions for “push” & “pull_request”
- Skip with proper commit message

---

### **Job Data & Outputs**

#### **Understanding Job Outputs**

**Artifacts**
1. Output files & folders
- Typically used for sharing log files, app binaries etc.

**Job Outputs**
1. Simple values
- Typically used for reusing a value in different jobs
- Example: Name of a file generated in a previous build step 

---

#### Caching Dependencies

Get Code - Install Dependencies - Test App/Build Project

- Often repeated
- Dependencies don’t change frequently


## Module Summary

**Artifacts**
- Jobs often product assets that should be shared or analyzed
- Examples: Deployable website files, logs, binaries etc.
- These assets are referred to as “Artifacts” (or “Job Artifacts”)
- GitHub Actions provides Actions for uploading & downloading

**Outputs**
- Besides Artifacts, Steps can product and share simple values
- These outputs are shared via ::set-output
- Jobs can pick up & share Step outputs via the steps context
- Other Jobs can use Job outputs via the needs context

**Caching**
- Caching can help speed up repeated, slow Steps
- Typical use-case: Caching dependencies
- But any files & folder can be cached
- The cache Action automatically stores & updates cache values (based on the cache key)
- Important: Don’t use caching for artifacts!

---

### Environment Variables & Secrets

Environment Variables vs Secrets

Some environment variable values should never be exposed
- Example: Database access password

Use Secrets
- Together with environment variables

GitHub Repository Environments - Attached to Repositories
- Workflow Jobs can target Environments

1. **Different Configurations**
- Different Secrets for different environments (e.g., different database passwords)

2. **Special protection rules**
- e.g., only events related to certain branches can use an environment

---

**Module Summary**

1. **Environment Variables**
- Dynamic values used in code (e.g., database name)
- May differ from workflow to workflow
- Can be defined on Workflow-, Job- or Step-level
- Can be used in code and in the GitHub Actions Workflow
- Accessible via interpolation and the env context object

2. **Secrets**
- Some dynamic values should not be exposed anywhere
- Examples: Database credentials, API keys etc.
- Secrets can be stored on Repository-level or via Environments
- Secrets can be referenced via the secrets context object

3. **GitHub Actions Environments**
- Jobs can reference different GitHub Actions Environments
- Environments allow you to set up extra protection rules
- You can also store Secrets on Environment-level

---

### Controlling Execution Flow

Special Conditional Functions

**failure()**
- Returns true when any previous Step or Job failed

**success()**
- Returns true when none of the previous steps have failed

**always()**
- Causes the step to always execute, even when cancelled

**cancelled()**
- Returns true if the workflow has been cancelled

### Module Summary

**Conditional Jobs & Steps**
- Control Step or Job execution with if & dynamic expressions
- Change default behavior with failure(), success(), cancelled() or always()
- Use continue-on-error to ignore Step failure

**Matrix Jobs**
- Run multiple Job configurations in parallel
- Add or remove individual combinations
- Control whether a single failing Job should cancel all other Matrix Jobs via continue-on-error

**Reusable Workflows**
- Workflows can be reused via the workflow_call event
- Reuse any logic (as many Jobs & Steps as needed)
- Work with inputs, outputs and secrets as required

---

### Using Containers

**What Are Containers?**
**Docker Container**
- Packages that contain code + its execution environment
- Advantage: Reproducible execution environment & results

#### Containers & GitHub Actions

**Docker Container**
- Packages that contain code + its execution environment
- Advantage: Reproducible execution environment & results

**GitHub Actions Runners**
- Your containerized Job is hosted by the Runner
- Steps execute inside the container
- You can also create Services: Utility containers used by your Steps (Example: Testing Database)

Why Use Containers?

**Container**
Full control over environment & installed software

**Just the Runner**
Choose from list of pre-defined environments (incl. installed software)

---

**Module Summary**

**Containers**
- Packages of code + execution environment
- Great for creating re-usable execution packages & ensuring consistency
- Example: Same environment for testing + production

**Containers for Jobs**
- You can run Jobs in pre-defined environments
- Build your own container images or use public images
- Great for Jobs that need extra tools or lots of customization

**Service Containers**
- Extra services can be used by Steps in Jobs
- Example: Locally running, isolated testing database
- Based on custom images or public / community images

---

#### **Building Custom Actions**

**Why Custom Actions?**

1. Simplify Workflow Steps
- Instead of writing multiple (possibly very complex) Step definitions, you can build and use a single custom Action
- Multiple Steps can be grouped into a single custom Action

2. No Existing (Community) Action
- Existing, public Actions might not solve the specific problem you have in your Workflow
- Custom Actions can contain any logic you need to solve your specific Workflow problems


---

**Different Types of Custom Actions**

**JavaScript Actions**
- Execute a JavaScript file
- Use JavaScript (NodeJS) + any packages of your choice
- Pretty straightforward (if you know JavaScript)

**Docker Actions**
- Create a Dockerfile with your required configuration
- Perform any task(s) of your choice with any language
- Lots of flexibility but requires Docker knowledge

**Composite Actions**
- Combine multiple Workflow Steps in one single Action
- Combine run (commands) and uses (Actions)
- Allows for reusing shared Steps (without extra skills)

---

Module Summary

**What & Why?**
- Simplify Workflows & avoid repeated Steps
- Implement logic that solves a problem not solved by any 
- publicly available ActionCreate & share Actions with the Community


**Composite Actions**
- Create custom Actions by combining multiple Steps
- Composite Actions are like “Workflow Excerpts”
- Use Actions (via uses) and Commands (via run) as needed


**JavaScript & Docker Actions**
- Write Action logic in JavaScript (NodeJS) with @actions/toolkit
- Alternatively: Create your own Action environment with Docker
- Either way: Use inputs, set outputs and perform any logic

---

Permissions & Security

Security Concerns

**Script Injection**
- A value, set outside a Workflow, is used in a Workflow
- Example: Issue title used in a Workflow shell command
- Workflow / command behavior could be changed

**Malicious Third-Party Actions**
- Actions can perform any logic, including potentially malicious logic
- Example: A third-party Action that reads and exports your secrets
- Only use trusted Actions and inspect code of unknown / untrusted authors

**Permission Issues**
- Consider avoiding overly permissive permissions
- Example: Only allow checking out code (“read-only”)
- GitHub Actions supports fine grained permissions control




