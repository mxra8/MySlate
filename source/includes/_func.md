# Google analytics and Compress Page

## Google analytics

You can use a function to create your Google Analytics Code.

<aside class="warning">Remember to edit your config.php file to set your Google Analytics ID</aside>

> Imagine this file

```php
<html>
    <head>
        <link href='//my_css.css'>
        <script src='//jquery.min.js'></script>
        <?php echo $this->create_ga ?>
    </head>
</html>
```

> And this will output something like this:

```php
<html>
    <head>
        <link href='//my_css.css'>
        <script src='//jquery.min.js'></script>
        <!-- Global site tag (gtag.js) - Google Analytics -->
        <script async src='https://www.googletagmanager.com/gtag/js?id=UA-XXXXXXX-X'></script>
        <script>
            window.dataLayer = window.dataLayer || [];
            function gtag(){dataLayer.push(arguments);}
            gtag('js', new Date());
            gtag('config', 'UA-XXXXXXX-X');
        </script>
    </head>
</html>
```

## Compress page

You can compress all the code of your page.

> Image this file

```php
<?php
$this->compress_page();
?>
<html>
    <head>
        <link href='//my_css.css'>
        <script src='//jquery.min.js'></script>
    </head>
</html>
```

> And this will output something like this:

```php
<html><head><link href='//my_css.css'><script src='//jquery.min.js'></script></head></html>
```
