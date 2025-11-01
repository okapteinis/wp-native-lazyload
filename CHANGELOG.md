# Changelog

All notable changes to Native Lazyload will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-11-01

### PHP 8.4 Compatibility Update

#### Changed
- **Minimum PHP version updated from 7.0 to 7.4**
  - Updated version check in `native-lazyload.php:30`
  - Updated error message to display "7.4" instead of "7.0"
  - Ensures compatibility with PHP 8.0, 8.1, 8.2, 8.3, and 8.4

#### Improved
- **Enhanced $_SERVER access sanitization** (`src/Context.php:81-92`)
  - Added proper `isset()` check before accessing `$_SERVER['HTTP_X_REQUESTED_WITH']`
  - Added `sanitize_text_field()` for proper input sanitization
  - Improved security and PHP 8.4 compliance

#### Updated
- **WordPress compatibility**
  - Tested with WordPress 6.7
  - Confirmed compatibility with latest WordPress version

- **Plugin version**
  - Version bumped from 1.0.2 to 1.1.0
  - Minor version bump due to minimum requirement change

### 🔧 Technical Details

#### Files Modified
1. **native-lazyload.php**
   - Line 14: Version updated to 1.1.0
   - Line 30: PHP version check updated to 7.4
   - Line 66: Error message updated to show 7.4

2. **src/Context.php**
   - Lines 81-92: Improved `is_ajax()` method with proper sanitization

3. **readme.txt**
   - Lines 3: Added `okapteinis` as contributor
   - Line 5: Tested up to WordPress 6.7
   - Line 6: Requires PHP updated to 7.4
   - Line 7: Stable tag updated to 1.1.0
   - Lines 67-74: Added changelog entry for v1.1.0

#### Code Quality
- ✅ All existing type hints preserved
- ✅ Strict comparisons (===, !==) maintained
- ✅ No breaking changes to public API
- ✅ Backward compatible with existing implementations

#### Testing
- Tested with PHP 7.4, 8.0, 8.1, 8.2, 8.3, 8.4
- Tested with WordPress 6.7
- No regressions in functionality

### 📊 Statistics
- **Files changed:** 3
- **Lines added:** ~15
- **Lines removed:** ~6
- **Breaking changes:** None
- **Security improvements:** 1 (sanitization)

### 👥 Contributors
- **Ojārs Kapteinis** - PHP 8.4 migration and documentation

### 📄 License Note
These modifications are licensed under CC BY-NC-ND 4.0
Original plugin remains under Apache License 2.0

---

## [1.0.2] - Previous Release

### Fixed
- Fix broken images which are using data URI scheme (e.g. base64-encoded images). Props [ieim](https://github.com/ieim)
- Fix images in IE 11 not being loaded until the user starts scrolling. Props [Soean](https://github.com/Soean)
- Fix image loading script not working in IE10 and other browsers that do not support `dataset`

---

## [1.0.1] - Previous Release

### Improved
- Improve compatibility with other plugins by using more specific class and only adding it for JS fallback
- Run lazy-load script on `DOMContentLoaded` when necessary to improve compatibility with plugins like Autoptimize
- Do not transform elements inside an AJAX response due to lack of predictability of the context and script execution

---

## [1.0.0] - Initial Release

### Added
- Initial release
- Native lazy-loading support using browser `loading` attribute
- JavaScript fallback for browsers without native support
- Support for images and iframes
- AMP compatibility
- `skip-lazy` class for excluding elements
- Filter `native_lazyload_fallback_script_enabled` to disable JS fallback

### Credit
This plugin is partly based on logic from [WP Rig](https://github.com/wprig/wprig/blob/v2.0/inc/Lazyload/Component.php) as well as recommendations from [web.dev](https://web.dev/native-lazy-loading) and [developers.google.com](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/)

---

[1.1.0]: https://github.com/okapteinis/wp-native-lazyload/compare/1.0.2...1.1.0
[1.0.2]: https://github.com/GoogleChromeLabs/wp-native-lazyload/releases/tag/1.0.2
[1.0.1]: https://github.com/GoogleChromeLabs/wp-native-lazyload/releases/tag/1.0.1
[1.0.0]: https://github.com/GoogleChromeLabs/wp-native-lazyload/releases/tag/1.0.0
