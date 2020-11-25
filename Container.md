# Container
- [Introduction](#introduction)
- [Bindings](#bindings)
    - [Basic Binding](#basic-binding)
    - [Alias Binding](#alias-binding)
    - [Resolved Binding](#resolved-binding)

## Introduction
A container is both a registry composed of objects and a mechanism for retrieving them. It's the library and the librarian, so to speak. Containers provide developers a tool to more easily manage dependencies.

The container in zeplara is the result of the implementation of [PSR-11](https://www.php-fig.org/psr/psr-11/). The container in zeplara is very powerful so you don't need to worry anymore when entering arguments into the function or method that the container calls. Container in zeplara can call class, function, method and object callable.

## Bindings
Binding in a container in zeplara is a store that stores values ​​and they can be called in several places. Binding is useful when you call a class, function, method, and object callable.
### Basic Binding
```php
$container = new Container;
$container->set('Zeplara\Routing\Router', Zeplara\Routing\Router::class);

$container = new Container;
$container->set('Zeplara\Routing\Router', function () {
    return new Zeplara\Routing\Router;
});
```
### Alias Binding

### Resolved Binding
