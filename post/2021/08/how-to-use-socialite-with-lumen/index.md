---
title: "How to use Socialite with Lumen"
date: "2021-08-29"
categories: 
  - "random"
---

So I am playing around with a hobby project in Lumen, and I had a few issues setting up Socialite with [Lumen](https://lumen.laravel.com/).

Anyhow, as it often is with development, after a bit of persistence, I found a solution.

All I really needed was to fetch user details from a Facebook Access Token, that I have already fetched in another application.

### Step 1:  Install

**Follow installation guide from the official documentation:** [https://laravel.com/docs/8.x/socialite#installation](https://laravel.com/docs/8.x/socialite#installation)

### Step 2: config/services.php

**Manually create config/services.php folder in the root of your Lumen project.** This config folder is not default in Lumen and you'll have to manually create it. Mine looks like this:

```
<?php
return [
    'facebook' => [
        'client_id' => env('FACEBOOK_CLIENT_ID'),
        'client_secret' => env('FACEBOOK_CLIENT_SECRET'),
        'redirect' => 'http://example.com/callback-url',
    ]
];
```

Depending on your project, you might need to add more to your configuration.

### Step 3: Modify bootstrap/app.php

In order for Lumen to register the newly created **config/services.php** file, we'll need to modify **bootstrap/app.php** a bit to look for this configuration, as well as registering the ServiceProvider for Socialite.

Under "Register Config Files" add: **$app->configure('services');**

```
/*
|--------------------------------------------------------------------------
| Register Config Files
|--------------------------------------------------------------------------
|
| Now we will register the "app" configuration file. If the file exists in
| your configuration directory it will be loaded; otherwise, we'll load
| the default version. You may register other files below as needed.
|
*/

$app->configure('app');
$app->configure('services'); // add this
```

And then register your service provider, add: **$app->register(Laravel\\Socialite\\SocialiteServiceProvider::class);** under ServiceProviders:

```
/*
|--------------------------------------------------------------------------
| Register Service Providers
|--------------------------------------------------------------------------
|
| Here we will register all of the application's service providers which
| are used to bind services into the container. Service providers are
| totally optional, so you are not required to uncomment this line.
|
*/

$app->register(Flipbox\LumenGenerator\LumenGeneratorServiceProvider::class);
// $app->register(App\Providers\AppServiceProvider::class);
// $app->register(App\Providers\AuthServiceProvider::class);
// $app->register(App\Providers\EventServiceProvider::class);


$app->register(Laravel\Socialite\SocialiteServiceProvider::class); // add this
```

### Hurray, it \*should\* now work

At least it did for me. Now in my routes/web.php I can use Socialite like this:

```
$router->get('/get-facebook-user', function () {
    $user = Socialite::driver('facebook')->userFromToken('SOME_TOKEN');

    dd($user); // the facebook user
});
```
