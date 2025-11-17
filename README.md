[![WordPress plugin](https://img.shields.io/wordpress/plugin/v/native-lazyload.svg?maxAge=2592000)](https://wordpress.org/plugins/native-lazyload/)
[![WordPress](https://img.shields.io/wordpress/v/native-lazyload.svg?maxAge=2592000)](https://wordpress.org/plugins/native-lazyload/)
[![Build Status](https://api.travis-ci.org/GoogleChromeLabs/wp-native-lazyload.png?branch=main)](https://travis-ci.org/GoogleChromeLabs/wp-native-lazyload)

# Native Lazyload

Lazy-loads media using the native browser feature with **intelligent LCP (Largest Contentful Paint) optimization** for superior Core Web Vitals scores. [Learn more about the `loading` attribute](https://web.dev/native-lazy-loading) or [view the WordPress core ticket](https://core.trac.wordpress.org/ticket/44427) where inclusion of a similar implementation in WordPress core itself is being discussed.

## Key Features

✅ **LCP Optimization** - Automatically excludes above-the-fold images from lazy loading
✅ **Core Web Vitals Friendly** - Improves performance scores by preventing lazy load of critical images
✅ **Smart Detection** - Featured images, hero images, and first content images load eagerly
✅ **fetchpriority Support** - Critical images get `fetchpriority="high"` for faster loading
✅ **Universal Browser Support** - Native `loading` attribute now supported by all major browsers
✅ **Fully Customizable** - Extensive filter hooks for fine-grained control
✅ **PHP 8.4 Compatible** - Tested with the latest PHP versions and modern WordPress standards

If the `loading` attribute is not supported by the browser (older browsers), the plugin falls back to a JavaScript solution based on `IntersectionObserver`. For the case that JavaScript is disabled, but the `loading` attribute _is_ supported by the browser, a `noscript` variant of the respective element will be added that also includes the `loading` attribute without any further changes.

## "Native" means "Fast"

If you have found your way over here, you are probably aware of how crucial performance is for a website's user experience and success. You might also know that lazy-loading is a key feature to improve said performance. However, the solutions for lazy-loading so far still added a bit of overhead themselves, since they relied on loading, parsing and running custom JavaScript logic, that may be more or less heavy on performance.

This plugin largely does away with this pattern. It relies on the new [`loading`](https://github.com/whatwg/html/pull/3752) attribute, which makes lazy-loading a native browser functionality. The attribute is already supported by Chrome, and will be rolled out to other browsers over time. The solution being "native" means that it does not rely on custom JavaScript logic, and thus is more lightweight. And "more lightweight" means "faster".

Last but not least, a neat thing to keep in mind is that this plugin will essentially improve itself over time, as more browsers roll out support for the `loading` attribute.

## Browser Compatibility

The native `loading` attribute is now supported by all major modern browsers:

| Browser | Version | Support |
|---------|---------|---------|
| Chrome  | 77+     | ✅ Full Support |
| Edge    | 79+     | ✅ Full Support |
| Firefox | 75+     | ✅ Full Support |
| Safari  | 15.4+   | ✅ Full Support |
| Opera   | 64+     | ✅ Full Support |

For older browsers, the plugin automatically falls back to a JavaScript-based solution using `IntersectionObserver`, ensuring universal compatibility.

## Core Web Vitals & Performance Best Practices

### Why LCP Optimization Matters

One of the most common mistakes with lazy loading is applying it to **above-the-fold images**, particularly the **Largest Contentful Paint (LCP)** element. This can significantly harm your Core Web Vitals scores and user experience.

This plugin **automatically detects and excludes LCP candidates** from lazy loading:

- ✅ Featured/thumbnail images (often appear above the fold)
- ✅ First 2 images in post content (configurable)
- ✅ Images with classes like `hero`, `banner`, `header-image`
- ✅ Images with `no-lazy` or `eager-load` classes
- ✅ Custom logo (always excluded)

### How It Works

**For LCP Images** (above the fold):
```html
<img src="hero.jpg" loading="eager" fetchpriority="high" class="hero">
```

**For Other Images** (below the fold):
```html
<img src="image.jpg" loading="lazy">
```

This ensures critical images load immediately while still optimizing below-the-fold content.

## Usage & Customization

The plugin works automatically with zero configuration. However, it provides extensive customization options through WordPress filters.

### Exclude Specific Images

Use the `wp_native_lazyload_skip_image` filter to exclude specific images from lazy loading:

```php
add_filter( 'wp_native_lazyload_skip_image', function( $skip, $image_html, $attributes ) {
    // Exclude images by class
    if ( ! empty( $attributes['class'] ) && strpos( $attributes['class'], 'my-critical-image' ) !== false ) {
        return true;
    }

    // Exclude images by src pattern
    if ( ! empty( $attributes['src'] ) && strpos( $attributes['src'], 'logo' ) !== false ) {
        return true;
    }

    return $skip;
}, 10, 3 );
```

### Exclude Specific CSS Classes

Add custom CSS classes that should always be excluded:

```php
add_filter( 'wp_native_lazyload_excluded_classes', function( $classes ) {
    return array_merge( $classes, [
        'priority-image',
        'immediate-load',
        'critical-content'
    ] );
} );
```

### Control Number of Eager-Loaded Images

Change how many images at the start of content should load eagerly:

```php
// Load first 3 images eagerly instead of default 2
add_filter( 'wp_native_lazyload_eager_images_count', function( $count ) {
    return 3;
} );
```

### Control Featured Image Behavior

Disable eager loading for featured images if needed:

```php
add_filter( 'wp_native_lazyload_eager_featured_image', function( $eager, $attributes ) {
    // Only load featured images eagerly on single posts
    return is_single();
}, 10, 2 );
```

### Control fetchpriority Attribute

Disable high priority for featured images:

```php
add_filter( 'wp_native_lazyload_featured_image_high_priority', function( $high_priority, $attributes ) {
    return false; // Featured images won't get fetchpriority="high"
}, 10, 2 );
```

### Disable Lazy Loading for Specific Post Types

Disable lazy loading entirely for specific post types:

```php
add_filter( 'wp_native_lazyload_disabled_post_types', function( $post_types ) {
    return [ 'product', 'portfolio' ];
} );
```

### Disable Lazy Loading for Specific Page Templates

Disable lazy loading for specific page templates:

```php
add_filter( 'wp_native_lazyload_disabled_templates', function( $templates ) {
    return [ 'template-landing-page.php', 'template-fullwidth.php' ];
} );
```

### Disable Lazy Loading Conditionally

Disable lazy loading based on any condition:

```php
add_filter( 'wp_native_lazyload_disabled', function( $disabled ) {
    // Disable on homepage
    if ( is_front_page() ) {
        return true;
    }

    // Disable for WooCommerce product pages
    if ( function_exists( 'is_product' ) && is_product() ) {
        return true;
    }

    return $disabled;
} );
```

### Using CSS Classes for Control

You can control lazy loading behavior directly in your HTML with CSS classes:

```html
<!-- These images will load eagerly with high priority -->
<img src="hero.jpg" class="hero">
<img src="banner.jpg" class="banner">
<img src="header.jpg" class="header-image">

<!-- These images will never be lazy loaded -->
<img src="logo.jpg" class="no-lazy">
<img src="critical.jpg" class="eager-load">
<img src="important.jpg" class="skip-lazy">
```

## Performance Testing Recommendations

After installing the plugin, test your site's performance:

1. **Google PageSpeed Insights** - https://pagespeed.web.dev/
   - Check that LCP score improves
   - Verify no "lazy loading above-fold images" warnings

2. **WebPageTest** - https://www.webpagetest.org/
   - Monitor LCP timing
   - Check fetchpriority attribute application

3. **Chrome DevTools Lighthouse**
   - Run Performance audit
   - Check Core Web Vitals scores

## Download

The easiest way to get the plugin is to install it from your WordPress admin dashboard, or manually [download it from wordpress.org](https://wordpress.org/plugins/native-lazyload/). Alternatively, you can also clone or download this repository to get the development version, but you will need to run a few commands to process assets and set up the autoloader:

1. `composer install`
2. `npm install`
3. `npm run build`

## Requirements

* WordPress >= 4.7
* PHP >= 7.0 (Tested up to PHP 8.4)

## Contributing

Any kind of contributions to Native Lazyload are welcome. Please [read the contributing guidelines](https://github.com/GoogleChromeLabs/wp-native-lazyload/blob/main/CONTRIBUTING.md) to get started.

## Credit

This plugin is partly based on logic from [WP Rig](https://github.com/wprig/wprig/blob/v2.0/inc/Lazyload/Component.php) as well as recommendations from [web.dev](https://web.dev/native-lazy-loading) and [developers.google.com](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video/).
