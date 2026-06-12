# Contribution 1: Add support for UTF8SMTP email format

**Contribution Number:** 1

**Student:** Nam Nguyen 

**Issue:** https://github.com/homarr-labs/homarr/issues/3764

**Status:** Phase II (in progress)

---

## Why I Chose This Issue

I chose issue #3764 "Add support for UTF8SMTP email format" because I think this project is so cool. The UI is very attractive, and I feel like this is something I'd definitely give a try and use in my everyday life. Also, the issue has tags "enhancement" and "good first issue", making it a manageable first contribution.

Homarr is a modern dashboard for organizing self-hosted services, and being able to support international characters (like Umlaute: ä, ö, ü) in email addresses is important for users worldwide. This issue provides a perfect entry point into the codebase - it's focused on updating an email validation regex to support UTF8SMTP format. My contribution will improve the user experience for international users who have special characters in their email addresses.

---

## Understanding the Issue

### Problem Description

Currently, Homarr's email validation does not support UTF8SMTP email format, which means email addresses containing international characters (Umlaute like ä, ö, ü) are rejected as invalid. This prevents users with such characters in their email addresses from using the application properly.

### Expected Behavior

Email addresses containing international characters (e.g., `user@münchen.de`, `jörg@example.com`) should be accepted as valid email addresses when the UTF8SMTP format is supported.

### Current Behavior

The current email validator rejects emails with non-ASCII characters, causing validation errors for users with internationalized email addresses.

### Affected Components

- Email validation logic (likely in authentication or user management modules)
- The regex pattern used for email validation

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1 - Navigate to user registration/login]
2. [Step 2 - Enter an email with Umlaute characters, e.g., `test@münchen.de`]
3. [Observed result - Email is rejected as invalid]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used
- [Zod PR #3678 - Reference regex implementation for UTF8 email validation](https://github.com/colinhacks/zod/pull/3678/files#diff-52632a4861fc9d7dc2dacef13cd91d60286dd706c1bb57438b8ee6a579a8796aR605) - Contains the regex pattern referenced in the issue
- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
