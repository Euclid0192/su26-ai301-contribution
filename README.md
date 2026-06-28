# Contribution 1: Add support for UTF8SMTP email format

**Contribution Number:** 1

**Student:** Nam Nguyen 

**Issue:** [https://github.com/homarr-labs/homarr/issues/3764](https://github.com/homarr-labs/homarr/issues/3764)

**Status:** Phase III (in progress)

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

- [x] Test case 1: Validate UTF8SMTP email with umlauts in local part (`jörg@example.com`)
- [x] Test case 2: Validate UTF8SMTP email with unicode domain (`user@münchen.de`)
- [x] Test case 3: Verify standard ASCII emails still pass (`user@example.com`)

### Integration Tests

- [x] Integration scenario 1: Validation package tests (294 tests passing)
- [x] Integration scenario 2: Auth package tests (1,911 tests passing)

### Manual Testing

Pending - Requires dev server restart to pick up monorepo package changes. Tests to perform:

- Create user with `jörg@example.com` (umlaut in local part)
- Edit profile with `user@münchen.de` (umlaut in domain)
- Verify standard ASCII emails still work

---

## Implementation Notes

### Week 3 Progress

Created centralized UTF8SMTP email validation for Homarr to support international characters (ä, ö, ü) in email addresses. Zod v4's default email validation uses an ASCII-only regex that rejects these characters.

**Approach:**

- Created new `packages/validation/src/email.ts` a reusable email validation exporting three schemas:
  - `utf8EmailSchema` - Base UTF8SMTP validation using `z.regexes.unicodeEmail`
  - `optionalEmailSchema` - For user creation (empty string stays empty)
  - `nullableEmailSchema` - For profile editing (empty → null)
- Updated all email validation occurrences across the codebase to use the new schemas
- Added 3 unit tests for UTF8SMTP email validation

### Week 4 progress

**Current Status:** Debugging frontend validation issue - all backend tests pass but UI still rejects international characters.

**Problem:**
- All unit tests and integration tests pass (2,205 total tests)
- However, typing international characters in the frontend (e.g., `jörg@example.com`) still yields "Invalid email" error before submitting the request
- The validation error appears immediately in the UI, suggesting client-side validation is not using the updated schemas

**Investigation Focus:**
- Checking if frontend components are importing from the correct validation package
- Verifying that the Next.js app is using `optionalEmailSchema` from `packages/validation`
- Investigating potential caching issues with the monorepo build
- Examining if there are duplicate validation schemas in the frontend code that bypass the centralized `email.ts` module

### Code Changes

- **Files modified:** 6 files (1 new, 5 updated)

1. `packages/validation/src/email.ts` (new file)
2. `packages/validation/src/user.ts`: uses `optionalEmailSchema` and `nullableEmailSchema`
3. `packages/validation/src/form/i18n.spec.ts`: added 3 UTF8SMTP test cases
4. `packages/auth/providers/credentials/authorization/ldap-authorization.ts`: uses `utf8EmailSchema`
5. `packages/old-import/src/user-schema.ts`: uses `nullableEmailSchema`
6. `apps/nextjs/src/app/[locale]/manage/users/create/_components/create-user-stepper.tsx`: uses `optionalEmailSchema`

- **Key commits**:
1. [Adding UTF8 email format validation](https://github.com/Euclid0192/homarr/commit/2f87d3a06e3d2d2f21c7f526e9464bb7ac940dbd)

- **Tests passing:** 2,205 total (294 validation + 1,911 auth)
- **Approach decisions:** Created dedicated `email.ts` module to centralize email validation logic, avoiding duplication of the UTF8SMTP regex pattern. Exported two variations (optional vs nullable) to match existing codebase patterns for different use cases.

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
