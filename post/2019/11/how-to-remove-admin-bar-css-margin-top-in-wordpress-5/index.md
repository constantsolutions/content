---
title: "How to remove admin bar css (margin-top) in WordPress 5"
date: "2019-11-05"
categories: 
  - "web-development"
tags: 
  - "wordpress"
---

The WordPress admin bar is outputting below HTML code to add margin-top to the html and body element of the page.

```
<style type="text/css" media="print">#wpadminbar { display:none; }</style>
<style type="text/css" media="screen">
	html { margin-top: 32px !important; }
	* html body { margin-top: 32px !important; }
	@media screen and ( max-width: 782px ) {
		html { margin-top: 46px !important; }
		* html body { margin-top: 46px !important; }
	}
</style>
```

However, if you like to have full control over what's happening and perhaps fixing this in your own stylesheet and thereby avoding unnecessary HTML, you can add this nifty line to your functions.php file (in your theme or plugin folder):

```
add_theme_support( 'admin-bar', array( 'callback' => '__return_false' ) );
```

Previously you could have this code, but **it is no longer working:**

```
add_action('get_header', function() {
	remove_action('wp2_head', '_admin_bar_bump_cb');
});
```

You're welcome ?
