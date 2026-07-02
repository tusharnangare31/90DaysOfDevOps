# Day 38 – YAML Basics: Learning the Language Behind Every DevOps Pipeline

## Introduction

Every DevOps engineer writes YAML almost every day.

Whether it's **Docker Compose**, **Kubernetes**, **GitHub Actions**, **GitLab CI**, **Ansible**, or cloud configuration files, YAML is everywhere. Before writing CI/CD pipelines, it's important to understand how YAML works because even a single indentation mistake can break an entire deployment.

Today's objective was to understand YAML syntax from scratch, write different YAML files manually, validate them using `yamllint`, and learn common mistakes that every beginner encounters.

---

# Task 1 – Creating My First YAML File

The first task was to create a simple `person.yaml` file describing myself.

### person.yaml

```yaml
name: Tushar Nangare
role: DevOps Engineer
experience_year: 1
learning: true
```

I verified the file using:

```bash
cat person.yaml
```

### Observation

- YAML stores data as key-value pairs.
- Quotes are optional unless special characters are used.
- Boolean values are written as `true` or `false`.
- The file is simple and human-readable.

---

## Screenshot

**person.yaml Output**

*(Add Screenshot 1 here)*

---

# Task 2 – Working with Lists

Next, I expanded the same file by adding lists.

```yaml
name: Tushar Nangare
role: DevOps Engineer
experience_year: 1
learning: true

tools:
  - Docker
  - Kubernetes
  - Jira
  - Linux
  - Git & GitHub

hobbies: [Coding, Swimming, Gym, Watching Movies]
```

After editing, I verified the file again.

---

## Screenshot

**Updated person.yaml**

*(Add Screenshot 2 here)*

---

## Two Ways to Create Lists in YAML

### 1. Block Style

```yaml
tools:
  - Docker
  - Kubernetes
  - Git
```

### 2. Inline Style

```yaml
tools: [Docker, Kubernetes, Git]
```

### Notes

Block style is easier to read and maintain.

Inline style is useful when the list is very small.

---

# Task 3 – Nested Objects

YAML supports nested data using indentation.

I created a second file called `server.yaml`.

```yaml
server:
  name: frontend
  ip: 192.168.0.1
  port: 5000

database:
  host: localhost
  name: app_db

  credentials:
    user: admin
    password: admin
```

This structure represents parent-child relationships.

---

## Screenshot

**server.yaml**

*(Add Screenshot 3 here)*

---

## Understanding Nested Objects

Here,

```
database
   ├── host
   ├── name
   └── credentials
           ├── user
           └── password
```

Indentation defines hierarchy.

Unlike JSON or XML, YAML does not use braces `{}` or tags.

---

# Task 4 – Multi-line Strings

YAML supports two types of multi-line strings.

---

## Using |

```yaml
startup_script_literal: |
  echo "Starting Server..."
  sudo systemctl start nginx
  echo "Done"
```

This preserves every newline exactly as written.

---

## Using >

```yaml
startup_script_folded: >
  echo "Starting Server..."
  sudo systemctl start nginx
  echo "Done"
```

This converts multiple lines into one continuous line.

---

## Screenshot

**Literal vs Folded Strings**

*(Add Screenshot 4 here)*

---

## Difference Between | and >

### |

- Preserves line breaks.
- Best for shell scripts.
- Useful for certificates.
- Good for configuration blocks.

Example output

```
Line 1
Line 2
Line 3
```

---

### >

- Folds multiple lines into one.
- Best for descriptions.
- Useful for documentation.
- Makes long text cleaner.

Example output

```
Line 1 Line 2 Line 3
```

---

# Task 5 – Validating YAML

A YAML file may look correct but still contain syntax errors.

To validate YAML files, I installed **yamllint**.

```bash
sudo apt update

sudo apt install yamllint
```

Then validated both files.

```bash
yamllint person.yaml

yamllint server.yaml
```

---

## Intentionally Creating Errors

To understand YAML validation, I intentionally introduced mistakes.

### Mistake 1

Used a **Tab** instead of spaces.

Error:

```
syntax error: found character '\t'
that cannot start any token
```

---

### Mistake 2

Incorrect inline list formatting.

Error:

```
too few spaces after comma
```

---

### Mistake 3

Incorrect indentation.

YAML immediately failed validation because indentation defines the structure.

After fixing every issue, both files validated successfully.

---

## Screenshot

**yamllint Validation**

*(Add Screenshot 5 here)*

---

# Task 6 – Spot the Difference

Correct YAML

```yaml
name: devops

tools:
  - docker
  - kubernetes
```

Incorrect YAML

```yaml
name: devops

tools:
- docker
  - kubernetes
```

---

## What's Wrong?

The first list item is not properly indented under `tools`.

YAML depends completely on indentation.

One missing space can make the file invalid.

---

# What I Learned Today

### 1. YAML Uses Spaces Only

Tabs are not allowed.

Always use spaces.

---

### 2. Indentation Defines Structure

YAML does not use braces like JSON.

Everything depends on indentation.

---

### 3. YAML is Human Readable

Its syntax is much cleaner than XML or JSON.

---

### 4. Multiple Ways to Create Lists

- Block style
- Inline style

---

### 5. Nested Objects are Easy

Indentation naturally creates parent-child relationships.

---

### 6. Multi-line Strings

`|`

Preserves formatting.

`>`

Folds multiple lines into one.

---

### 7. Validation is Important

Always validate YAML before using it in:

- Docker Compose
- Kubernetes
- GitHub Actions
- GitLab CI
- Ansible

---

# Challenges Faced

### Remembering Indentation

Coming from JSON, it feels unusual not using braces.

---

### Forgetting Spaces After Commas

Inline lists require proper spacing.

Incorrect

```yaml
[Docker,Kubernetes]
```

Correct

```yaml
[Docker, Kubernetes]
```

---

### Using Tabs

My first validation failed because I accidentally inserted a tab.

YAML only accepts spaces.

---

# Files Created

```
2026
└── day-38
    ├── person.yaml
    ├── server.yaml
    └── day-38-yaml.md
```

---

# Commands Used

```bash
cat person.yaml

cat server.yaml

vim person.yaml

vim server.yaml

yamllint person.yaml

yamllint server.yaml
```

---

# Key Takeaways

- YAML is the foundation of modern DevOps.
- Indentation is more important than syntax symbols.
- Lists can be block style or inline.
- Nested objects make configuration easy to understand.
- `|` preserves new lines while `>` folds them.
- Validation tools help catch mistakes before deployment.
- Even one extra tab or missing space can break Docker Compose or Kubernetes manifests.

---

# Conclusion

Today's session was all about mastering the fundamentals of YAML before moving into CI/CD pipelines. I created multiple YAML files, explored key-value pairs, lists, nested objects, and multi-line strings, and learned how to validate configurations using `yamllint`.

One of the biggest lessons was realizing how strict YAML is with indentation. Unlike programming languages where formatting is optional, YAML treats whitespace as part of its syntax. A single tab, incorrect indentation level, or missing space after a comma can cause the entire configuration to fail.

Although YAML looks simple at first glance, it is one of the most important skills for anyone working with Docker Compose, Kubernetes, GitHub Actions, GitLab CI/CD, Jenkins, Ansible, and cloud-native technologies. Building a strong foundation now will make writing infrastructure and deployment pipelines much easier in the coming days.