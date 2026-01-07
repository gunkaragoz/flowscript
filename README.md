# flowscript
Simple intuitive offline flow chart diagramming tool inspired by eraser

## Version

Current version: **0.1.5**

### Versioning Guidelines

This project follows [Semantic Versioning](https://semver.org/) with automated version bumping:

- **Patch version** (0.0.X): Automatically incremented for bug fixes
  - Use commit prefix: `fix:` or `bugfix:`
  - Example: `fix: correct PNG export clipping issue`

- **Minor version** (0.X.0): Automatically incremented for new features
  - Use commit prefix: `feat:` or `feature:`
  - Example: `feat: add dark mode support`

- **Major version** (X.0.0): Manually set for breaking changes
  - Requires manual update to `version.json`

When you push to the `main` branch with commits using conventional commit prefixes (`fix:` or `feat:`), the version will automatically bump and create a new release tag.

---

# FlowScript Best Practices Guide

## Basic Structure

```
// 1. Define nodes first
// 2. Define connections after
// 3. Groups at the end
```

---

## Node Naming

**Do:** Use clear, action-oriented labels
```
Validate Input [decision]
Send Email
Save to Database [database]
```

**Don't:** Use vague or technical jargon
```
Check1 [decision]
Process
DB_OP_001
```

---

## Decision Nodes (Conditions)

**Always label both branches with yes/no or true/false:**

```
Is Valid? [decision]

Is Valid? > Process Data : yes
Is Valid? > Show Error : no
```

Other common label pairs:
- `yes` / `no`
- `true` / `false`
- `valid` / `invalid`
- `success` / `failure`
- `found` / `not found`

---

## Terminal Nodes

**Mark start and end points clearly:**

```
Start [terminal] :globe:
End [terminal] :check:
Error Exit [terminal] {red} :x:
```

---

## Color Coding

Use colors consistently for status:

| Color | Use For |
|-------|---------|
| `{green}` | Success, completion, positive outcomes |
| `{red}` | Errors, failures, warnings |
| `{yellow}` | Pending, waiting, caution |
| `{blue}` | Information, processing |
| `{purple}` | User actions, inputs |
| `{orange}` | External services, async |

```
Success [terminal] {green} :check:
Error {red} :x:
Waiting {yellow}
```

---

## Icons

Add icons for quick visual recognition:

```
// User actions
Login :user:
Input Form :mail:

// Infrastructure
API Server :api:
Database [database] :postgresql:
Cache :cache:

// Status
Complete :check:
Failed :x:
Alert :alert:
```

---

## Groups

**Use groups for:**
- Logical separation (Frontend / Backend / Database)
- Service boundaries (AWS / GCP / On-prem)
- Swim lanes (User / System / Admin)

```
Frontend {purple}:
  React App :code:
  Redux Store :cache:

Backend {blue}:
  API Gateway :api:
  Auth Service :lock:
```

**Connect groups for high-level architecture:**
```
Frontend > Backend : REST API
Backend > Database : queries
```

---

## Flow Direction

**Keep flow top-to-bottom:**
- Start nodes at the top (no incoming edges)
- End nodes at the bottom (no outgoing edges)
- Avoid upward arrows when possible

**Exception:** Loops should go back up
```
Retry [decision]
Retry > Start : yes
Retry > End : no
```

---

## Complete Example

```
// === User Registration Flow ===

// Entry point
Start [terminal] :globe:

// User actions
Enter Details [io] :user:
Submit Form

// Validation
Validate Input [decision]
Show Errors {red} :alert:

// Processing
Check Email Exists [decision]
Email Taken {yellow}
Create Account
Send Welcome Email :mail:

// Exit
Success [terminal] {green} :check:

// Flow
Start > Enter Details
Enter Details > Submit Form
Submit Form > Validate Input

Validate Input > Show Errors : invalid
Validate Input > Check Email Exists : valid
Show Errors > Enter Details

Check Email Exists > Email Taken : yes
Check Email Exists > Create Account : no
Email Taken > Enter Details

Create Account > Send Welcome Email
Send Welcome Email > Success
```

---

## Quick Reference

| Element | Syntax |
|---------|--------|
| Basic node | `Node Name` |
| Decision | `Question? [decision]` |
| Terminal | `Start [terminal]` |
| Database | `DB [database]` |
| Input/Output | `User Input [io]` |
| With icon | `Server :server:` |
| With color | `Error {red}` |
| Connection | `A > B` |
| Labeled connection | `A > B : label` |
| Chain | `A > B > C` |
| Group | `Name:` + indented children |
| Colored group | `Name {blue}:` |
| Comment | `// comment` |
