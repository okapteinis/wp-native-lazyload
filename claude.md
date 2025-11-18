# Instructions for Claude Code

When working on this WordPress plugin, follow these guidelines:

## PHP 8.4 Compatibility Checks

- Review all PHP code for PHP 8.4 compatibility issues
- Check for deprecated dynamic properties and add explicit property declarations
- Look for implicit nullable types and make them explicit
- Verify all function signatures have proper type hints
- Add strict type declarations at the top of PHP files using `declare(strict_types=1)`
- Check for deprecated or removed PHP functions
- Test with PHP 8.4 enabled error reporting

## WordPress and ClassicPress Compatibility

- Verify compatibility with WordPress 6.7
- Verify compatibility with ClassicPress
- Check that no deprecated WordPress functions are used

## Security Checks

- Verify all user inputs are properly sanitized using `sanitize_text_field()` and similar functions
- Check for proper output escaping to prevent XSS
- Ensure no SQL injection vulnerabilities
- Verify nonce usage for CSRF protection
- Run security scans if available

## Testing

- Run all PHPUnit tests and ensure they pass
- Use PHPStan for static analysis
- Run PHPCS for code style checks
- Test locally with PHP 8.4 and WordPress 6.7

## Commit Guidelines

- Always include both co-authors with their exact emails:
  - Ojārs Kapteinis <ojars@kapteinis.lv>
  - Claude (AI Assistant) <code@anthropic.com>

- Always preserve the original license: Apache License 2.0

- Use clear commit message format with type and summary line, for example:
  ```
  feat: Add new feature for X
  fix: Resolve issue with Y
  docs: Update documentation for Z
  ```

- Push all changes to the nightly branch only after tests pass
  - Note: The `nightly` branch is used for AI-assisted development workflows
  - For general contributions, refer to CONTRIBUTING.md which specifies the `main` branch
