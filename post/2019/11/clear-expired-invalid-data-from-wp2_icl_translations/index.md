---
title: "Clear expired/invalid data from wp2_icl_translations"
date: "2019-11-14"
categories: 
  - "web-development"
tags: 
  - "optimization"
  - "wordpress"
  - "wpml"
---

I managed to clear out 18.000 unused / invalid rows from the \`wp2\_icl\_translations\` table, after realizing a lot of old/expired translations was present in the table. This can happen if you're doing huge import/exports to your site, or delete products/pages/posts manually (via SQL) and also are using WPML.

Run below query on your database to delete unused rows (rows that point to a page/post that no longer exists in the database). **Remember to backup your table before doing this, even though I would say it's pretty safe to run it.**

```
DELETE wp2_icl_translations FROM wp2_icl_translations
LEFT JOIN wp2_posts p ON p.id = element_id
WHERE element_type IN ('post_product_variation', 'post_product', 'post_post', 'post_page') and p.id is null
```

If you'd like to see which rows would be deleted first, you could run this query and have a look at the results:

```
SELECT element_id, element_type, p.id as post_id, p.post_title, p.post_type, p.post_date FROM wp2_icl_translations
LEFT JOIN wp2_posts p ON p.id = element_id
WHERE element_type IN ('post_product_variation', 'post_product') and p.id is null
```
