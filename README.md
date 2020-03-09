# alldebrid-php

Simple abstraction wrapper around Alldebrid API v4 in PHP.

Requires PHP 7.1 or newer.

## Documentation

The documentation for the Alldebrid API can be found [here][apidocs].

The PHP library documentation can be found in this README. An example folder is also provided to show library usage.


## Installation

You can install **alldebrid-php** in two ways, direct include or by using composer or by downloading the source.


### Via Composer:

**alldebrid-php** is available on Packagist as the
[`alldebrid/alldebrid`](https://packagist.org/packages/alldebrid/alldebrid) package:

```
composer require alldebrid/alldebrid
```

Then add the autoloader to your application with this line: ``require("vendor/autoload.php")``

### Via include:

A standalone version of this library is included, download and include it manually : 

```php
include('./alldebrid.standalone.php'); 
```


## Quickstart

### Authentication

The Alldebrid API requires an agent and an apikey to authenticate requests. The agent is your app / library / script name ( [doc](https://docs.alldebrid.com/v4/#authentication)).

You can view , create and manage your API keys in your [Alldebrid Apikey dashboard][apikeys], or generate them remotly (with user action) though the PIN flow ( [doc](https://docs.alldebrid.com/v4/#pin-auth) / [example](https://github.com/Alldebrid/alldebrid-php/blob/master/examples/pin.php) ).

```php
$agent = 'myAppName'; // Your project name
$apikey = 'YYYYYY'; // Apikey

$alldebrid = new \Alldebrid\Alldebrid($agent, $apikey);
```

### Use and error handling

This library use Go-style error handling by default, but can be configured to use Exception if you choose so.

```php
$agent = 'myAppName'; // Your project name
$apikey = 'YYYYYY'; // Apikey

$alldebrid = new \Alldebrid\Alldebrid($agent, $apikey);

// Go-style, always return an array [ $response, $error ]
[ $user, $error ] = $alldebrid->user();

if($error) {
    // Api call failed or returned an error
    $errorMessage = $user;
    die("Could not get user informations, error " . $error . " : " . $errorMessage . "\n");
}

// No error, you can consume the response 
echo "Hello, " . $user['username'] . "\n";
```

Using the library with Exceptions 

```php
$agent = 'myAppName'; // Your project name
$apikey = 'YYYYYY'; // Apikey

$alldebrid = new \Alldebrid\Alldebrid($agent, $apikey);
$alldebrid->setErrorMode('exception');

try {
    $user = $alldebrid->user();
} catch(Exception $e) {
    die("Exception " . $e->getMessage() . "\n");
}

// No error, you can consume the response 
echo "Hello, " . $user['username'] . "\n";
```

### High and low-level use

This library provides multiple way to use the Alldebrid API, once the agent and apikey set.

The lowest use is the call the api() function with the desired endpoint and params

```php
$alldebrid = new \Alldebrid\Alldebrid($agent, $apikey);
$myLink = 'https://example.com/example';
[ $response, $error ] = $alldebrid->api('link/infos', ['link' => [ $myLink ] ]);
```


Every API endpoint has its own wrapper you can use : 

```php
$alldebrid = new \Alldebrid\Alldebrid($agent, $apikey);
$myLink = 'https://example.com/example';
[ $response, $error ] = $alldebrid->linkInfos($myLink);
```

For links, magnets and pin flow, helper object are provided : 

```php
$alldebrid = new \Alldebrid\Alldebrid($agent, $apikey);
$myLink = 'https://example.com/example';

$link = $alldebrid->link($myLink);
[ $response, $error ] = $link->infos();
//  you can then call $link->unlock() if there is no error 
```

Every calls of this library are documented in the [example folder](https://github.com/Alldebrid/alldebrid-php/tree/master/examples).

## Getting help

If you need help installing or using the library, please check the [Api docs](apidocs) first, check the [example codes](https://github.com/Alldebrid/alldebrid-php/tree/master/examples) and then [contact us](https://alldebrid.com/contact/) if you don't find an answer to your question.

If you've instead found a bug in the library or would like new features added, go ahead and open issues or pull requests against this repo!

[apidocs]: https://docs.alldebrid.com
[apikeys]: https://alldebrid.com/apikeys/