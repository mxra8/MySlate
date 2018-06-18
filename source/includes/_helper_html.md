# HTML Helper

The HTML Helper file contains functions that assist in working with HTML.

> First of all you need to load the helper file

```php
<?php
$this->load->helper('html');
```

## Embed string

Parse text to find url's for embed enabled services like: youtube.com, blip.tv, vimeo.com, dailymotion.com, flickr.com, smugmug.com, hulu.com, revision3.com, wordpress.tv, funnyordie.com, soundcloud.com, slideshare.net and instagram.com and embed elements automatically.

```php
<?php
/**
 * @param  string $string
 * @param  string $width default: 560
 * @param  string $height default: 315
 *
 * @return string
*/
echo $this->html->embed('https://www.youtube.com/watch?v=OK7tWZdO0Ys');
// output: <iframe width="560" height="315" src="https://www.youtube.com/embed/OK7tWZdO0Ys?feature=oembed" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
```

## Create link tag

Lets you create HTML &#x3C;a /&#x3E; tags. This is useful for stylesheet links, as well as other links.

## Validate email address

```php
<?php
var_dump($this->html->validate_email('test@mysite.com'));
// output void
```

## Generate QR code

```php
<?php
/**
* @param  string  $string
* @param  integer $width default: 150
* @param  integer $height default: 150
* @param  array $attributes
*/

echo $this->html->get_qr_code('my string to qr code');
// output: <img src="http://chart.apis.google.com/chart?chs=150x150&cht=qr&chl=my+string+to+qr+code" width="150" height="150"  />
```

## Heading

Generates an HTML heading tag

```php
<?php
/**
 * @param	string	content
 * @param	int	heading level
 * @param	string
 *
 * @return	string
*/
echo $this->html->heading('My heading', '1', 'class="my_head"');
// output: <h1 class='my_head'>My heading</h1>
```

## Image

Generates an &#x3C;img /&#x3E; element

```php
<?php
/**
 * @param	mixed $src
 * @param	mixed $attributes
 *
 * @return	string
*/
echo $this->html->img('https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35151329_2086731074928935_8159052263000637440_o.jpg?_nc_cat=0&oh=46fd588d04581c56e2892ab6e7dbadba&oe=5BAE3343', 'class="my_image"');
// output: <img src="https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35151329_2086731074928935_8159052263000637440_o.jpg?_nc_cat=0&oh=46fd588d04581c56e2892ab6e7dbadba&oe=5BAE3343" alt="" class="my_image" />
```

## UL

Generates an &#x3C;ul /&#x3E; element

```php
<?php
$list = array(
        'red',
        'blue',
        'green',
        'yellow'
);

$attributes = array(
        'class' => 'boldlist',
        'id'    => 'mylist'
);
echo $this->html->ul($list, $attributes);
// output: <ul class="boldlist" id="mylist">
//  <li>red</li>
//  <li>blue</li>
//  <li>green</li>
//  <li>yellow</li>
//</ul>
```

## OL

Generates an &#x3C;ol /&#x3E; element

```php
<?php
$list = array(
        'red',
        'blue',
        'green',
        'yellow'
);

$attributes = array(
        'class' => 'boldlist',
        'id'    => 'mylist'
);
echo $this->html->ol($list, $attributes);
// output: <ol class="boldlist" id="mylist">
//  <li>red</li>
//  <li>blue</li>
//  <li>green</li>
//  <li>yellow</li>
//</ol>
```
