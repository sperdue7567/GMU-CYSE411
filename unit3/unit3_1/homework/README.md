# CYSE 411 – Unit 3.1 DevSecOps Lab

## Objective

This lab explores DevSecOps by integrating Static Application Security Testing (SAST) into CI/CD pipelines using Semgrep and GitHub Actions.

You will:
- Detect vulnerabilities using SAST
- Configure multiple pipelines
- Create custom security rules
- Map vulnerabilities to detection logic

---

## IMPORTANT RULE

You MUST NOT run Semgrep locally.

All scans must be executed via GitHub Actions pipelines.

---

## Vulnerabilities in the System

The application contains:

- SQL Injection
- Cross-Site Scripting (XSS)
- Path Traversal
- CSRF (missing protection)

---

# SEMGRESP Rules Design Tutorial

## 1) How to create customize rules using SEMGRESP

A Semgrep rule is essentially: “A structured pattern + logic that finds insecure code and explains why it’s bad.”

It works at the AST / semantic level, not just regex.

A rule answers:
- What code pattern am I looking for?
- Under what conditions is it dangerous?
- What should the developer do instead?

## 2. Basic Rule Structure

A Semgrep rule is written in YAML:
```yaml
rules:
  - id: my-first-rule
    patterns:
      - pattern: eval($X)
    message: Avoid using eval()
    languages: [javascript]
    severity: ERROR

```
### Key Fields

| Field                  | Purpose                                  |
|------------------------|------------------------------------------|
| `id`                   | Unique rule name                         |
| `pattern` / `patterns` | Defines what code pattern to detect      |
| `message`              | Message shown to the developer           |
| `languages`            | Target languages (JS, Python, Java, etc) |
| `severity`             | Issue level (INFO, WARNING, ERROR)       |

---



## Tasks

### Task 1 – Basic Pipeline

Run provided workflow:
- sast-basic.yml (.github/workflows/)

---

### Task 2 – Create Threat-Specific Pipelines

You must create the following workflows:

- sast-xss.yml
- sast-sqli.yml
- sast-path.yml
- sast-csrf.yml

Each workflow must:
- Use a specific Semgrep rule
- Target a specific vulnerability
- Run independently

---

### Task 3 – Enforcement

At least TWO workflows must:
- Fail the pipeline when vulnerabilities are found

(Hint: use --error)

---

### Task 4 – Custom Rule (IMPORTANT)

Create your own Semgrep rule:

File:
semgrep/custom-rule.yaml

Requirements:
- Must detect a new pattern
- Must be inspired by:
https://semgrep.dev/docs/writing-rules/rule-ideas

Examples:
- Detect use of eval()
- Detect hardcoded secrets
- Detect unsafe file access

---

### Task 5 – Custom Pipeline

Create:
.github/workflows/sast-custom.yml

This workflow must:
- Use your custom rule
- Run successfully in pipeline

---

### Task 6 – Filtering

Modify at least one workflow to:
- Filter severity (e.g., ERROR only)
- Combine multiple rules

---

## Autograder

Run:
python tests/grader.py

---

## Deliverables

- 5 workflow YAML files
- 1 custom Semgrep rule

---

## Key Insight

DevSecOps is not about running tools.

It is about:
- Selecting the right detection logic
- Mapping threats to controls
- Enforcing security automatically



TODO: https://semgrep.dev/docs/writing-rules/rule-ideas
