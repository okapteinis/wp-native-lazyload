---
description: Creates commits with co-author attribution and preserves original license.
---

# Commit Command

This command helps create properly formatted commits for the wp-native-lazyload project with co-author attribution and license preservation.

## Instructions

When the user runs `/commit`, follow these steps:

### 1. Review Changes
Use the following git commands to understand what has changed:
- `git status` - See all untracked and modified files
- `git diff` - See unstaged changes
- `git diff --staged` - See staged changes
- `git log -3 --oneline` - See recent commit history for context

### 2. Compatibility Checks
Before committing, verify:
- **WordPress 6.7 compatibility** - Check for deprecated functions or features
- **ClassicPress compatibility** - Ensure no WordPress-only features that break ClassicPress
- **PHP 8.4 compatibility** - Check for deprecated PHP features, proper type declarations, and compatibility issues

### 3. Stage Changes
Use `git add` to stage relevant files:
- `git add <file>` for specific files
- Review with `git status` after staging

### 4. Create Commit Message
Format the commit message as follows:

```
<Brief summary of changes in imperative mood>

<Optional detailed description of what was changed and why>

Co-authored-by: Ojārs Kapteinis <ojars@kapteinis.lv>
Co-authored-by: Claude (AI Assistant) <code@anthropic.com>

License: Apache License 2.0 (preserved)
```

**Commit message guidelines:**
- Use imperative mood for the summary (e.g., "Add feature" not "Added feature")
- Focus on WHY the change was made, not just WHAT changed
- Keep the summary under 72 characters
- Reference compatibility improvements (e.g., "Ensure PHP 8.4 compatibility")
- Mention security improvements if applicable
- Note WordPress 6.7 or ClassicPress compatibility updates

### 5. Create the Commit
Use a heredoc for proper formatting:

```bash
git commit -m "$(cat <<'EOF'
<Summary line>

<Detailed description if needed>

Co-authored-by: Ojārs Kapteinis <ojars@kapteinis.lv>
Co-authored-by: Claude (AI Assistant) <code@anthropic.com>

License: Apache License 2.0 (preserved)
EOF
)"
```

### 6. Verify the Commit
After committing:
- Run `git log -1` to verify the commit was created correctly
- Run `git status` to confirm the working directory is clean

### 7. Testing and Push (if applicable)
- If tests exist, run them to ensure nothing is broken
- Only push to the **nightly branch** after successful tests
- Use `git push -u origin nightly` for pushing to the nightly branch

## Important Notes

- **Always preserve the Apache License 2.0** - Never change the license in commits
- **Include both co-authors** in every commit made through this command
- **Check compatibility** with WordPress 6.7, ClassicPress, and PHP 8.4 before committing
- **Run tests** before pushing to ensure code quality
- **Push to nightly branch only** after successful tests
- **Never use `git commit --amend`** unless explicitly requested by the user
- **Never force push** unless explicitly requested by the user

## Example Commit Messages

### Example 1: Feature Addition
```
Add lazy loading support for video elements

Extends lazy loading functionality to support HTML5 video elements
with native loading attribute. Ensures compatibility with WordPress 6.7
and PHP 8.4 by using proper type declarations.

Co-authored-by: Ojārs Kapteinis <ojars@kapteinis.lv>
Co-authored-by: Claude (AI Assistant) <code@anthropic.com>

License: Apache License 2.0 (preserved)
```

### Example 2: Compatibility Fix
```
Ensure PHP 8.4 compatibility for type declarations

Updates code to use proper type hints and return types compatible
with PHP 8.4. Removes deprecated dynamic property usage and adds
proper property declarations.

Co-authored-by: Ojārs Kapteinis <ojars@kapteinis.lv>
Co-authored-by: Claude (AI Assistant) <code@anthropic.com>

License: Apache License 2.0 (preserved)
```

### Example 3: Security Improvement
```
Improve input validation and escaping

Adds additional sanitization for user inputs and ensures all output
is properly escaped. Follows WordPress 6.7 security best practices.

Co-authored-by: Ojārs Kapteinis <ojars@kapteinis.lv>
Co-authored-by: Claude (AI Assistant) <code@anthropic.com>

License: Apache License 2.0 (preserved)
```

## Workflow Summary

1. **Review**: `git status`, `git diff`, `git log`
2. **Check**: WordPress 6.7, ClassicPress, PHP 8.4 compatibility
3. **Stage**: `git add <files>`
4. **Commit**: With co-authors and license preservation
5. **Verify**: `git log -1`, `git status`
6. **Test**: Run tests if available
7. **Push**: To nightly branch only after successful tests
