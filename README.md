Laravel 4.2 - Layout View
===============

[![Build Status](https://api.travis-ci.org/awjudd/l4-layoutview.png)](https://travis-ci.org/awjudd/l4-layoutview)
[![ProjectStatus](http://stillmaintained.com/awjudd/l4-layoutview.png)](http://stillmaintained.com/awjudd/l4-layoutview)

A quick and easy way to handle different layouts in **Laravel 4**

## Features

 - Adds the ability to have your views automatically chosen based on your selected layout
 - Supports a primary layout and a fallback layout

## Quick Start

In the `require` key of `composer.json` file add the following

```
"awjudd/layoutview": "1.1.*"
```

Run the Composer update command

```
$ composer update
```

In your `config/app.php` add `'Awjudd\Layoutview\LayoutviewServiceProvider'` to the end of the `$providers` array

```php
'providers' => array(

    'Illuminate\Foundation\Providers\ArtisanServiceProvider',
    'Illuminate\Auth\AuthServiceProvider',
    ...
    'Awjudd\Layoutview\LayoutviewServiceProvider',

),

'aliases' => array(

    'App'             => 'Illuminate\Support\Facades\App',
    'Artisan'         => 'Illuminate\Support\Facades\Artisan',
    ...
    'View'            => 'Awjudd\Layoutview\Facades\LayoutViewFacade',
),
```

### Setup

Setup for Layout View is simple.  All you need to do is change the mapping in the aliases array for the "View" to point to the new type.

Once done, the next step for you to do is to configure the namespaces that you want included as well as the priority of the views that are being rendered.  To publish the configuration you can do the following:

```
$ php artisan config:publish awjudd/layoutview
```

There are two ways to configure what the base and fallback layouts are.  The two ways are as follows:

 - In the configuration file, you can fill in the following values in the configuration file:

```php
'layout' => array (

    /**
     * The layout that will be used to fall back on if the selected doesn't exist.
     * 
     * @var string
     */
    'fallback' => NULL,

    /**
     * The layout that will be the first choice to be used.
     * 
     * @var string
     */
    'selected' => NULL,
),
```

 - In your base controller call the following functions:

```php
View::setSelectedLayout('base layout name');
View::setFallbackLayout('fallback layout name');
```

### Configuration

Within the configuration file there is a single key.  This key is `'namespaces'`.  Fill this array with any namesapces that you would like the application to look through the folders of in order to find the best-fitting view.

**Please Note** The order that the elements are added in this array dictate the order in which they are scanned through for display purposes.  Because of this, you will want your most specific namespaces to be listed first.

```php
'namespaces' => array (
    /*
     * Should always keep a blank one here to search in no namespaces.
     */
    '',
),
```

## Folder Structure

In order for this to work, this package requires the following folder structure (in short, your layout name is the top level).

    ./views
        ./base
            ./home
                ./index.blade.php
        ./fallback
            ./home
                ./index.blade.php
                ./test.blade.php
            ./layout
                ./html.blade.php

In this example, "base" and "fallback" are your two possible layouts. If you have "base" as your selected layout, and "fallback" as your fallback view (meaning if one doesn't exist in your main, it will look there next).  If you do the following:

```php
View::make('home.index');
```

It will render the view in 'base/home/index'.  However, if you do one that doesn't exist in the base folder, then it will automatically pick up the one from the fallback.  Example:

```php
View::make('home.test');
```

Will render 'fallback/home/test'.

## License

Layout View is free software distributed under the terms of the MIT license

## Release Notes:

1.1.0:
 - Laravel 4.2 support

1.1.1:
 - Removing the necessity to add the "::" into the namespaces
 - Making it so that if you don't specify a base/fallback layout it will ignore the folder

## Additional Information

Any issues, please [report here](https://github.com/awjudd/l4-layoutview/issues)
