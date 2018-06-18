# Error reporting

PHP errors are not good enough for development, it's as simple as that. This aims to solve this.

![PHPError](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35151329_2086731074928935_8159052263000637440_o.jpg?_nc_cat=0&oh=46fd588d04581c56e2892ab6e7dbadba&oe=5BAE3343)

When an error strikes, the page is replaced with a full stack trace, syntax highlighting, and all displayed to be readable.

**Works with Ajax too!**

If the server errors during an ajax request, then the request is paused, and the error is displayed in the browser. You can then click to automatically retry the last request.

![PHPError](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/34840885_2086731088262267_312278653956259840_o.jpg?_nc_cat=0&oh=c2df62f8e1b1c132cd5a20779765f46f&oe=5BC0D525)

This requires no changes to your JavaScript, and works with existing JS libraries such as jQuery.

> By default Error Handler hasn't options in line 92 of index.php file

```php
<?php
$handler = new ErrorHandler(null);
$handler->turn_on();
```

## All options

Usage Examples

```php
<?php
$options = array(
    'snippet_num_lines' => 10,
    'background_text'  => 'Error!',
    'error_reporting_off' => 0,
    'error_reporting_on' => E_ALL | E_STRICT
);
$handler = new ErrorHandler($options);
$handler->turn_on();
```

Option | Default | Description
-------------- | -------------- | --------------
catch_ajax_errors | true | When on, this will inject JS Ajax wrapping code, to allow this to catch any future JSON errors.
catch_supressed_errors | false | The @ supresses errors. If set to true, then they are still reported anyway, but respected when false.
catch_class_not_found | true | When true, loading a class that does not exist will be caught. If there are any existing class loaders, they will be run first, giving you a chance to load the class.
error_reporting_on | -1 (everything) | The error reporting value for when errors are turned on by Error Handler.
error_reporting_off | the value of 'error_reporting' | The error reporting value for when Error Handler reporting is turned off. By default it just goes back to the standard level.
application_root | $_SERVER['DOCUMENT_ROOT'] | When it's working out the stack trace, this is the root folder of the application, to use as it's base. A relative path can be given, but lets be honest, an explicit path is the way to guarantee that you will get the path you want. My relative might not be the same as your relative.
display_line_numbers | true | When on, line numbers are shown in the editor, and when off, they are not.
server_name | $_SERVER['SERVER_NAME'] | The name for this server; it's domain address or ip being used to access it. This is displayed in the output to tell you which project the error is being reported in.
ignore_folders | null (no folders) | This is allows you to highlight non-framework code in a stack trace. An array of folders to ignore, when working out the stack trace. This is folder prefixes in relation to the application_root, whatever that might be. They are only ignored if there is a file found outside of them. If you still don't get what this does, don't worry, it's here cos I use it.
application_folders | null (no folders) | Just like ignore, but anything found in these folders takes precedence over anything else.
background_text | an empty string | The text that appeares in the background. By default this is blank. Why? You can replace this with the name of your framework, for extra customization spice.
wordpress | false | This is no longer needed on the latest versions of WordPress. When on, this will use a lower error reporting level, which works with Wordpress. This is because the Wordpress code base is full of minor bugs, and doesn't like strict error reporting.
enable_saving | true |  When true, Error Handler allows external requests to save files. This is so the editor can save changes back.
