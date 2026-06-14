# Contribution 1: Add support for UTF8SMTP email format

**Contribution Number:** 1

**Student:** Nam Nguyen 

**Issue:** https://github.com/homarr-labs/homarr/issues/3764

**Status:** Phase II (complete)

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

Setting up the Homarr development environment presented several challenges:

1. **Package Manager Mismatch**: The project uses pnpm while I was using npm. I needed to install pnpm to ensure compatibility with the project's dependency management.

2. **Node Version Difference**: The project requires Node.js v24 while I was running v22. I had to upgrade my Node.js version to match the project's requirements.

3. **Redis on Windows**: The project requires redis-server, but the `redis-server` command doesn't work natively on Windows. I needed to find an alternative way to run Redis (likely using WSL, Docker, or a Windows-compatible Redis distribution).

### Steps to Reproduce

1. Click on the **Account** icon in the navigation
2. Navigate to **User** → **Manage user**
3. Click **Edit current account**
4. Type in an email with international characters (e.g., `jörg@example.com` or `user@münchen.de`)

### Reproduction Evidence

- **Screenshots/logs:**
<img width="2095" height="1450" alt="Screenshot 2026-06-14 115433" src="https://github.com/user-attachments/assets/3ee5d081-4eb2-4fa0-8589-7a810dcf929c" />

- **My findings:** The issue is permanent (not transient) as I can reproduce it multiple times. As long as there's an international character (like ö) in the email address, the UI renders an "email invalid" error immediately.

- Branch link: https://github.com/Euclid0192/homarr/tree/nam/add-email-utf8smtp-format

---

## Solution Approach

### Analysis

The root cause of this issue is that Homarr uses Zod for email validation, and Zod's default email validation regex does not support international characters (UTF8SMTP format). Zod's current regex only matches ASCII characters in email addresses, causing any email containing characters like ä, ö, ü to be rejected as invalid.


### Proposed Solution

Replace the existing Zod email validation regex with the UTF8SMTP-compatible regex from Zod PR #3678. This regex properly handles international characters (Umlaute and other non-ASCII characters) in email addresses while maintaining backward compatibility with standard ASCII emails.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** 
- The current Zod email validator rejects emails containing international characters (ä, ö, ü, etc.)
- Need to update the validation regex to support UTF8SMTP email format
- Reference implementation available from Zod PR #3678 with the updated regex pattern

**Match:** 
- Find where Zod email validation schemas are defined in the Homarr codebase
- Look for imports of `z.email()` or similar email validation patterns
- Check if there are custom email validation utilities

**Plan:** 
1. Locate the Zod email validation schemas in the codebase (likely in auth/user management modules)
2. Identify the current regex pattern or `z.email()` usage
3. Replace it with the UTF8SMTP-compatible regex from Zod PR #3678
4. Test with various email addresses containing Umlaute characters (e.g., `jörg@example.com`, `user@münchen.de`)
5. Ensure existing valid ASCII emails still pass validation
6. Verify the UI no longer shows "email invalid" error for internationalized emails

**Implement:** https://github.com/Euclid0192/homarr/tree/nam/add-email-utf8smtp-format

**Review:** 
- [ ] Follows Homarr coding standards and conventions
- [ ] The regex change is minimal and targeted
- [ ] Backward compatible - existing ASCII emails still validate
- [ ] No breaking changes to the API
- [ ] Code is properly documented
- [ ] Tests cover both internationalized and standard email formats

**Evaluate:** 
- Manual testing: Verify that emails with Umlaute (ä, ö, ü) no longer show "email invalid" error in the UI
- Manual testing: Verify that standard ASCII emails still validate correctly
- Unit tests: Ensure all email validation test cases pass
- Regression testing: Confirm no other validation logic is affected

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
