# Routing

- [Introduction](#introduction)
- [Usage](#usage)
    - [With Facade](#with-facade)
    - [Without Facade](#without-facade)
- [Attributes](#attributes)
    - [Name](#name)
    - [Middleware](#middleware)
    - [Prefix](#prefix)
    - [Requirements](#requirements)
    - [Where](#where)
    - [Scheme](#scheme)
    - [Host](#host)
    - [Namespace](#namespace)

## Introduction
Almost every modern framework has a router feature. If we translate, the term router is a route/road/route. In the world of web development, the route in question is the path to a web-based application, so we can say that a router is a module in an application that functions to manage roads/routes in web-based applications.

The router manages the entrance in the form of requests to the application, they sort and process url requests to be processed according to the final destination of the url. It could be that the url functions to retrieve data then display it, delete data, display forms, and process sessions.

Router modules usually implement the http standard. What does it mean? The router also sorts requests based on the http method. Where is the request url with the GET method and which is the POST method (also other http methods such as DELETE, PUT, PATCH). The router module has mapped each of these methods. We cannot request POST method data to url with GET method and vice versa.

In Zeplara itself, it supports routing, Zeplara provides a strong pattern to make it easier for developers to create applications, one of which is that developers can use one route for multiple pages using the optional pattern contained in the host or the pattern itself.

List of supported Http Methods:
- GET | HEAD
- POST
- PUT
- PATCH
- DELETE
- OPTIONS

## Usage
The following are some of the ways to use routing that you can use
### With Facade
```php
Route::get('/', function () {

});

Route::prefix('/api')->group(function () {
    Route::get('/', function () {
    
    });
});
```
### Without Facade
```php
$router = new Router;

$router->addRoute(new Route(
    '/', // 1. pattern (required)
    function () {}, // 2. callback (required)
    ['GET'], // 3. methods (required)
    [
        'name' => 'home'
    ] // 4. attributes (optional)
));

// or
$router->get('/', function () {

})->name('home');
```

## Attributes
You can create attributes for the route that you register in 2 ways, namely when initiating the route or by using the chain method.
### Name
> Note: This attribute can only be used when you register a route instead of a group route.

The name attribute is used to identify the route, it is used when you want to redirect to another page.
```php
$router = new Router;

$router->addRoute(new Route(
    '/',
    function () {},
    ['GET'],
    [
        'name' => 'home'
    ]
));

// or
$router->name('home')->get('/', function () {

});

// or 
$router->get('/', function () {

})->name('home');

// Will show the error, dont use like this
$router->name('/')->group(function () {

});
```
### Middleware
> Note: When you set the middleware attribute without using the middleware() method, the value must be an array type.

Middleware especially in the world of web programming, is a software layer that sits between the router and the controller. Because the position of the middleware is between the router and the controller, the function of the middleware has a generic function, namely:

- Authentication
- Authorization
- Response handler
- And others
```php
$router = new Router;

$router->get('/')->middleware(...$args);

// or
$router->addRoute(new Route(
    '/',
    function () {},
    ['GET'],
    [
        'middleware' => [] // Must be an array value
    ]
));
```
### Prefix
> Note: This attribute will throw an error when you register the route not the group route.

This attribute is used when you want to create a group route, if you create a group route without the prefix, it might be quite tiring or confusing when you have many routes in the group.
```php
$router = new Router;

$router->prefix('/api')->group(function () {
    // The pattern will be /api/users
    $router->get('/users', function () {

    });
});
```
### Requirements
This attribute is used when you want to create a certain condition in the route pattern that you use, this method will be work when the pattern is not static pattern. You can using the regexp in the value of requirement.
```php
$router = new Router;

$router->get('/{name}', function () {

})->requirements(['name' => 'ilham']);
```
### Where
This attribute is alias of Requirements Attribute
### Scheme
> Note: If you specify a scheme attribute on a group route and you specify a scheme on which route directly, the scheme attribute on that route are overwritten but have no effect for other routes in that group.

A Uniform Resource Locator (URL) is a compact string representation of the location for a resource that is available via the Internet. [RFC 2396](https://tools.ietf.org/html/rfc2718#ref-1) defines the general syntax and semantics of URIs, and, by inclusion, URLs.  URLs are designated by including a "\<scheme>:" and then a "\<scheme-specific-part>". Many URL schemes are already defined.

This document provides guidelines for the definition of new URL
schemes, for consideration by those who are defining and registering or evaluating those definitions. The process by which new URL schemes are registered is defined in [RFC 2717](https://tools.ietf.org/html/rfc2718#ref-2)

In Zeplara has support routing with different Scheme, For example, a page with the http scheme will be different from a page with the https scheme
```php
$route = new Router;

$route->scheme('http')->group(function () {
    $route->get('/', function () {
    
    });
});
$route->scheme('https')->group(function () {
    $route->get('/', function () {
    
    });
});

// Route
$route->get('/', function () {

})->scheme('http');

$route->get('/', function () {

})->scheme('https');

// or
$route->addRoute(new Route(
    '/',
    function () {
    
    },
    ['GET'],
    [
        'scheme' => 'http'
    ]
));
$route->addRoute(new Route(
    '/',
    function () {
    
    },
    ['GET'],
    [
        'scheme' => 'https'
    ]
));
```
### Host
> Note: If you specify a host attribute on a group route and you specify a host on which route directly, the host attribute on that route are overwritten but have no effect for other routes in that group.

The hostname is what a device is called on a network. Alternative terms for this are computer name and site name. The hostname is used to distinguish devices within a local network. In addition, computers can be found by others through the hostname, which enables data exchange within a network, for example. Hostnames are used on the internet as part of the fully qualified domain name.

In Zeplara itself has support optional pattern, so you can make many domain or subdomain with one route or group routing.
```php

$router = new Router;

$router->host('{sub?}.example.com')->group(function () {
    $router->get('/', function () {
    
    });
});

$router->get('/', function () {

})->host('{sub?}.example.com');
```
### Namespace
> Note: The namespace attribute can only be used for group routing

A namespace is a group or collection of entities (classes, objects, functions) grouped under one name. The namespace in PHP also describes the abstract directory of a file. The PHP namespace does not specify the directory where the files are stored.
```php
$router = new Router;

$router->namespace('App\Http\Controllers')->group(function () {
    $router->get('/', function () {
    
    });
});
```
