# Router

*/application/Config/routers.settings.php*

CodeDmx has a router to help you to mane your routes without use htaccess.
Let's explain this:

## Router Features

* HTTP methods
* Dynamic routing with (named) route parameters
* Flexible regular expression routing, inspired by [Sinatra](http://sinatrarb.com/)
* Reversed routing
* Custom regexes

## Set Base Path

Optionally, if your project lives in a sub-folder of your web root you use the set_base_path() method to set a base path.

*$this->router->set_base_path('/sub-folder/');*

## Router default routes

> Router has default routes, firstly the router is Loaded

```php
<?php
$this->router = new COD_Router();
```

> If you type http://yoursite.com this will enter in this condition

```php
<?php
// Make sure that default file exists in the View folder
// By default, the $GLOBALS['COD']->default is 'home'
$this->router->map('GET|POST', '/', function() {
    require $GLOBALS['COD']->doc_view . $GLOBALS['COD']->default . '.php';
});
```

> If you type http://yoursite.com/some_file/ this will enter in this condicion

```php
<?php
// Make sure that not_found file exists in the View/Static folder
// By default, the $GLOBALS['COD']->not_found is 'File'
// All string before your site will enter in this condition
$this->router->map('GET|POST', '/[:page]/', function($page) {
    if ( ! file_exists($GLOBALS['COD']->doc_view . $page . '.php'))
    {
        require $GLOBALS['COD']->doc_view . 'static' . DS . $GLOBALS['COD']->not_found . '.php';
        exit(1);
    }
    else
    {
        require $GLOBALS['COD']->doc_view . $page . '.php';
        exit(1);
    }
});
```

## Mapping routes

By now, you should have rewritten al requests to be handled by the routers settings which it created the Router instance.

To map your routes, use the map() method. The following example maps all GET / requests.

```php
<?php
$this->router->map( 'GET', '/', 'render_home', 'home' );
```

The map() method accepts the following parameters.

$method (string)
This is a pipe-delimited string of the accepted HTTP requests methods.

Example: GET|POST|PATCH|PUT|DELETE

$route (string)
This is the route pattern to match against. This can be a plain string, one of the predefined regex filters or a custom regex. Custom regexes must start with @.

Examples:

Route | Example Match | Variables
-------------- | -------------- | --------------
/contact/ | /contact/ |
/users/[i:id]/ | /users/12/ | $id: 12
/[a:c]/[a:a]?/[i:id]? | /controller/action/21 | $c: "controller", $a: "action", $id: 21

$target (mixed)
As Router leaves handling routes up to you, this can be anything.

Example using a function callback:
function() { ... }

Example using a controller#action string:
UserController#showDetails

$name (string, optional)
If you want to use reversed routing, specify a name parameter so you can later generate URL's using this route.

Example:
user_details

## Example Mapping

```php
<?php
// map homepage using callable
$this->router->map( 'GET', '/contact/[:string]', function($string) {
    $_GET['key_string'] = $string;

    require $GLOBALS['COD']->doc_view . 'contact.php';
});

// map users details page using controller#action string
$this->router->map( 'GET', '/users/[i:id]/', 'UserController#showDetails' );

// map contact form handler using function name string
$this->router->map( 'POST', '/contact/', 'handleContactForm' );
```

For quickly adding multiple routes, you can use the addRoutes method. This method accepts an array or any kind of traversable.

```php
<?php
$this->router->add_routes(array(
  array('PATCH','/users/[i:id]', 'users#update', 'update_user'),
  array('DELETE','/users/[i:id]', 'users#delete', 'delete_user')
));
```

## Match types

You can use the following limits on your named parameters. Router will create the correct regexes for you.

Expression | Description
-------------- | --------------
* | Match all request URIs
[i] | Match an integer
[i:id] | Match an integer as 'id'
[a:action] | Match alphanumeric characters as 'action'
[h:key] | Match hexadecimal characters as 'key'
[:action] | Match anything up to the next / or end of the URI as 'action'
[create|edit:action] | Match either 'create' or 'edit' as 'action'
[*] | Catch all (lazy, stops at the next trailing slash)
[*:trailing] | Catch all as 'trailing' (lazy)
[**:trailing] | Catch all (possessive - will match the rest of the URI)
.[:format]? | Match an optional parameter 'format' - a / or . before the block is also optional

The character before the colon (the 'match type') is a shortcut for one of the following regular expressions

Expression | Regex
-------------- | --------------
i | [0-9]++
a | [0-9A-Za-z]++
h | [0-9A-Fa-f]++
* | .+?
** | .++
 | [^/\.]++

> You can register your own match types using the add_match_types() method.

```php
<?php
$this->router->add_match_types(array('cId' => '[a-zA-Z]{2}[0-9](?:_[0-9]++)?'));
```

## Matching Requests

To match the current request, just call the `match()` method without any parameters.

*$match = $this->router->match();*

If a match was found, the match() method will return an associative array with the following 3 keys.

target (mixed)
The target of the route, given during mapping the route.

params (array)
Named parameters found in the request url.

name (string)
The name of the route.

Example

```php
<?php
$this->router = new COD_Router();
$this->router->map( 'GET', '/', function() { .. }, 'home' );

// assuming current request url = '/'
$match = $this->router->match();

/*
array(3) {
	["target"]	=> object(Closure)#2 (0) { }
	["params"]	=> array(0) { }
	["name"] 	=> 'home'
}
*/
```

Router does not process the request for you, you are free to use the method you prefer. Here is a simplified example how to process requests using closures though.
