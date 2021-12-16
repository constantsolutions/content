---
title: "Nested layouts in Nuxt/Vue.js"
date: "2020-02-21"
categories: 
  - "random"
---

If you're like me, and like to keep things tidy and prefer not to repeat yourself, you will most likely find yourself needing to nest layouts when developing a Nuxt application.

This is luckily a fairly easy thing to do, expect you need to do a bit of tweaking.

Assuming you haven't modified your layouts/default.vue file, it will most likely look like this:

```
// layouts/default.vue
<template>
	<div>
		<nuxt />
	</div>
</template>
```

If we'd like to apply nested layouts, we could take advantage of **slots** and do something like this:

```
// layouts/default.vue
<template>
	<div>
		<nuxt v-if="!$slots.default" />
		<slot />
	</div>
</template>
```

Then we could add a new layout, like so:

```
// layouts/newLayout.vue
<template>
	<default-layout>
		<h1>Surrounding layout</h1>
		<nuxt />
	</default-layout>
</template>
<script>
import DefaultLayout from '~/layouts/default.vue';

export default {
	components: {
		DefaultLayout
	}
}
</script>
```

Congratulations. You now have a nested layout. In any file you'd like to use the nested layout, you simply set your layout like so:

```
<script>
export default {
	layout: 'newLayout' // name of your new layout
}
</script>
```
