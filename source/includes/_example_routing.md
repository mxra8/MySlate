# Example Routing

This example can help you to understand how CodeDmx's Router works.

Imagine that you have a blog site, and you need to get Year, Month, Day, and Article ID of your url to get the information of the Article (like Wordpress works).

Your url is this: *http://yoursite.com/news/2018/01/12/458219/*

* Year: 2018
* Month: 01
* Day: 12
* Article ID: 458219

But, we don't have any rule to get this kind of page... we need to work on it.

First of all we need to open */application/Config/routers.settings.php*

> You need to create a new rule

```php
<?php
$this->router = new COD_Router();

/* ******* NEW RULE ******* */
$this->router->map('GET|POST', '/news/[i:year]/[i:month]/[i:day]/[i:article_id]/', function($year, $month, $day, $article_id){
    $_GET['year'] = $year; // Will be 2018
    $_GET['month'] = $month; // Will be 01
    $_GET['day'] = $day; // Will be 12
    $_GET['article_id'] = $article_id; // Will be 458219

    require $GLOBALS['COD']->doc_view . 'news.php';
});
/* ******* NEW RULE ******* */

$this->router->map('GET|POST', '/', function(){
    require $GLOBALS['COD']->doc_view . $GLOBALS['COD']->default . '.php';
});
?>
```
