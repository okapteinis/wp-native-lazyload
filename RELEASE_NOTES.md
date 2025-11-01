# 📦 Native Lazyload v1.1.0 - Release Notes

**Release Date:** November 1, 2025
**Branch:** nightly
**Status:** ✅ Production Ready

---

## 🎯 Release Overview

This release updates the Native Lazyload plugin for **PHP 8.4 compatibility** and includes minor security improvements. The plugin continues to provide native browser lazy-loading for images and iframes with JavaScript fallback support.

---

## 📋 What's New in v1.1.0

### ⚙️ PHP 8.4 Compatibility

#### Updated Minimum PHP Version
- **Previous:** PHP 7.0
- **Current:** PHP 7.4
- **Tested with:** PHP 7.4, 8.0, 8.1, 8.2, 8.3, 8.4

**Why this change?**
- PHP 7.0-7.3 reached end-of-life years ago and no longer receive security updates
- PHP 7.4 is the oldest version still reasonably supported
- Ensures compatibility with modern PHP 8.x features and behavior
- Aligns with WordPress core recommendations

**File:** `native-lazyload.php`
```php
// Line 30 - Updated version check
if ( version_compare( phpversion(), '7.4', '<' ) ) {

// Line 66 - Updated error message
'7.4',
```

---

### 🔒 Security Improvements

#### Enhanced $_SERVER Access Sanitization

**Location:** `src/Context.php:81-92`

**Before:**
```php
public function is_ajax() : bool {
    if ( wp_doing_ajax() ) {
        return true;
    }

    // phpcs:ignore WordPress.Security.ValidatedSanitizedInput.InputNotSanitized
    return ! empty( $_SERVER['HTTP_X_REQUESTED_WITH'] )
        && strtolower( wp_unslash( $_SERVER['HTTP_X_REQUESTED_WITH'] ) ) === 'xmlhttprequest';
}
```

**After:**
```php
public function is_ajax() : bool {
    if ( wp_doing_ajax() ) {
        return true;
    }

    if ( ! isset( $_SERVER['HTTP_X_REQUESTED_WITH'] ) ) {
        return false;
    }

    $requested_with = sanitize_text_field( wp_unslash( $_SERVER['HTTP_X_REQUESTED_WITH'] ) );
    return strtolower( $requested_with ) === 'xmlhttprequest';
}
```

**Improvements:**
- ✅ Added explicit `isset()` check before accessing superglobal
- ✅ Added `sanitize_text_field()` for proper input sanitization
- ✅ Removed phpcs ignore comment (no longer needed)
- ✅ Improved code clarity and security compliance

---

### 🌐 WordPress Compatibility

- **Tested up to:** WordPress 6.7
- **Minimum version:** WordPress 4.7 (unchanged)
- **Multisite:** Fully compatible

---

## 📊 Release Statistics

| Metric | Value |
|--------|-------|
| **Files Changed** | 3 |
| **Lines Added** | ~15 |
| **Lines Removed** | ~6 |
| **Breaking Changes** | 0 |
| **Security Improvements** | 1 |
| **Version Bump** | Minor (1.0.2 → 1.1.0) |

---

## 🔧 Technical Requirements

### Minimum Requirements
- **WordPress:** 4.7 or higher
- **PHP:** 7.4 or higher (updated from 7.0)
- **Browser:** Any modern browser with `loading` attribute support

### Tested With
- **WordPress:** 6.7
- **PHP:** 7.4, 8.0, 8.1, 8.2, 8.3, 8.4
- **Browsers:** Chrome, Firefox, Safari, Edge

### Compatibility
- ✅ PHP 8.4 fully compatible
- ✅ WordPress 6.7 tested
- ✅ AMP compatible
- ✅ Multisite compatible
- ✅ Translation ready

---

## ⚠️ Upgrade Notes

### Breaking Changes
**None** - This release is fully backward compatible with v1.0.2

### Migration Path
1. Ensure your server runs PHP 7.4 or higher
2. Update plugin files (automatic via WordPress admin or manual)
3. No database migration required
4. No settings changes needed
5. Plugin will continue working with existing configuration

### Post-Update Actions
✅ No action required - plugin works immediately after update

---

## 🎯 What the Plugin Does

Native Lazyload uses the browser's native `loading` attribute to lazy-load images and iframes, improving page load performance without JavaScript overhead.

### Key Features
- ✅ **Native browser lazy-loading** using `loading="lazy"` attribute
- ✅ **JavaScript fallback** for browsers without native support
- ✅ **Automatic** - works on all images/iframes in content
- ✅ **Skip class** - add `skip-lazy` to exclude specific elements
- ✅ **No settings** - works out of the box
- ✅ **AMP compatible** - doesn't break AMP pages
- ✅ **Performance focused** - minimal overhead

### What Gets Lazy-Loaded
- Post content images (`the_content`)
- Post thumbnails (`post_thumbnail_html`)
- Avatars (`get_avatar`)
- Widget text images (`widget_text`)
- Image tags (`get_image_tag`)
- Iframes in content

### What Doesn't Get Lazy-Loaded
- Images with `skip-lazy` class
- Custom logo images
- Admin bar content
- AMP pages (AMP handles this natively)
- AJAX responses (for predictability)

---

## 📖 Code Quality

### Already Modern
This plugin was already well-written with modern PHP practices:

✅ **Type hints everywhere**
```php
public function __construct( Context $context )
public function context() : Context
public function filter_add_lazyload_placeholders( string $content ) : string
```

✅ **Strict comparisons**
```php
if ( null !== static::$instance )
if ( 'img' === $tag )
if ( false !== strpos( $classes, 'custom-logo' ) )
```

✅ **Namespaced classes**
```php
namespace Google\Native_Lazyload;
```

✅ **Clean architecture**
- Separation of concerns
- Single responsibility principle
- Dependency injection pattern

### v1.1.0 Improvements
- Enhanced input sanitization
- Updated PHP version requirements
- Improved security compliance

---

## 🐛 Known Issues

None at this time.

---

## 🔄 Disabling JavaScript Fallback

If you want to rely purely on native browser support without the JavaScript fallback:

```php
add_filter( 'native_lazyload_fallback_script_enabled', '__return_false' );
```

Add this to your theme's `functions.php` or a custom plugin.

---

## 📚 Documentation

For more information, see:
- **CHANGELOG.md** - Full version history
- **README.md** - Plugin overview
- **readme.txt** - WordPress.org plugin page content
- **CONTRIBUTING.md** - Contribution guidelines

---

## 🤝 Credits

### v1.1.0 Contributors
- **Ojārs Kapteinis** - PHP 8.4 migration, security improvements, documentation

### Original Authors
- **Google Chrome Labs** - Original plugin development
- **Felix Arntz (flixos90)** - Original development and maintenance

### Acknowledgments
- Based on logic from [WP Rig](https://github.com/wprig/wprig)
- Recommendations from [web.dev](https://web.dev/native-lazy-loading)
- Guidance from [developers.google.com](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/)

---

## 📄 Licenses

### Original Plugin
- **License:** Apache License 2.0
- **License URI:** https://www.apache.org/licenses/LICENSE-2.0
- **Copyright:** 2019 Google LLC

### v1.1.0 Modifications
- **Modifications License:** CC BY-NC-ND 4.0 (Attribution-NonCommercial-NoDerivatives 4.0 International)
- **Contributor:** Ojārs Kapteinis
- **Scope:** PHP 8.4 compatibility updates, documentation additions

---

## 📞 Support

### Reporting Issues
- **WordPress.org Support:** https://wordpress.org/support/plugin/native-lazyload/
- **GitHub Issues (Original):** https://github.com/GoogleChromeLabs/wp-native-lazyload/issues
- **GitHub Issues (Fork):** https://github.com/okapteinis/wp-native-lazyload/issues

### Translation
- **Translate:** https://translate.wordpress.org/projects/wp-plugins/native-lazyload

### Security Issues
For security vulnerabilities, please contact the maintainer directly rather than creating a public issue.

---

## 🚀 What's Next?

### Potential Future Updates
- Continued PHP compatibility updates
- WordPress core compatibility maintenance
- Performance optimizations
- Additional browser feature detection

### Browser Support Trends
The `loading` attribute is now supported by:
- ✅ Chrome 77+
- ✅ Firefox 75+
- ✅ Safari 15.4+
- ✅ Edge 79+

As native support grows, the JavaScript fallback becomes less necessary, making the plugin even more lightweight over time.

---

## ✨ Summary

This update ensures Native Lazyload continues to work flawlessly with modern PHP versions while maintaining all existing functionality. The plugin remains lightweight, fast, and effective at improving page load performance through native browser lazy-loading.

**Upgrade recommended for all users running PHP 7.4 or higher.**

---

**Thank you for using Native Lazyload!**

This plugin helps millions of websites load faster by leveraging native browser capabilities. Your contribution to web performance matters.
