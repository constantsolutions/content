---
title: "Cloudways Breeze cache plugin with WPML Multi Currency switcher"
date: "2019-11-12"
categories: 
  - "web-development"
tags: 
  - "caching"
  - "wordpress"
  - "wpml"
---

If you are using Cloudways and/or their caching plugin Breeze you might have issues if you're running a WooCommerce shop with WPML Multi Currency Switcher - at least I did, after migrating to Cloudways and switching away from W3 Total Cache.

The issue will only appear to non-admins, or guests/visitors visiting your site without being logged in (where cache is then not enabled, as per how Breeze works).

After a bit of researching I realized that this had something to do with WPML and how they work, and found below filter to be working. Add it to your themes functions.php file or anywhere you would add custom coding to your WordPress installation.

```
    add_filter('wcml_is_cache_enabled_for_switching_currency', function($cache_enabled) {
        return true;
    });
```
