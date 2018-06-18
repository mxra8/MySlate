# Paginator Class

CodeDmx has a lightweight PHP paginator, for generating pagination controls in the style of Stack Overflow and Flickr. The "first" and "last" page links are shown inline as page numbers, and excess page numbers are replaced by ellipses.

These examples show how the paginator handles overflow when there are a lot of pages. They're rendered using the sample templates provided in the examples directory, which depend on Twitter Bootstrap. You can easily use your own custom HTML to render the pagination control instead.

Default template:

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35229369_2087362041532505_3353137485461848064_n.jpg?_nc_cat=0&oh=1f1376e9c94372900737b4eee6f641db&oe=5BB22BB6)

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35151550_2087362051532504_5115176637876404224_o.jpg?_nc_cat=0&oh=0f8cf646da7ffee3f0b6d75ac44106d8&oe=5BA7424C)

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35294582_2087362044865838_4028467238162923520_o.jpg?_nc_cat=0&oh=73ad0633e40c59d2c0396b5ea2503923&oe=5BAF329A)

Small template (useful for mobile interfaces):

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35118939_2087363084865734_3141840629395357696_n.jpg?_nc_cat=0&oh=a38cad332f40f23edb130e959197f793&oe=5BC432DB)

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35239577_2087363054865737_1077832290723168256_n.jpg?_nc_cat=0&oh=0b69f6a95a15b0daffe942fb528f8ed9&oe=5BB60E33)

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35239412_2087363048199071_1706095395202924544_n.jpg?_nc_cat=0&oh=cf231007266d9f977ac3f2c70a50b7ad&oe=5BBD5AEF)

The small template renders the page number as a select list to save space:

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35114579_2087363061532403_7011802941312991232_n.jpg?_nc_cat=0&oh=c23bad97824a8cd15345ab6e1cbdfca4&oe=5B773DCA)

> First of all you need to load the library file

```php
<?php
$this->load->library('paginator');
```

## Basic usage

```php
<?php
$total_items = 1000;
$items_per_page = 50;
$current_page = 8;
$url_pattern = '/foo/page/(:num)';

$paginator = $this->paginator->get_pagination($total_items, $items_per_page, $current_page, $url_pattern);
?>
<html>
  <head>
    <!-- The default, built-in template supports the Twitter Bootstrap pagination styles. -->
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
  </head>
  <body>

    <?php
      // Example of rendering the pagination control with the built-in template.
      // See below for information about using other templates or custom rendering.

      echo $paginator;
    ?>

  </body>
</html>
```

> This will output the following:

![Paginator](https://scontent.fgdl4-1.fna.fbcdn.net/v/t1.0-9/35151550_2087362051532504_5115176637876404224_o.jpg?_nc_cat=0&oh=0f8cf646da7ffee3f0b6d75ac44106d8&oe=5BA7424C)

```php
<ul class="pagination">
    <li><a href="/foo/page/7">&laquo; Previous</a></li>
    <li><a href="/foo/page/1">1</a></li>
    <li class="disabled"><span>...</span></li>
    <li><a href="/foo/page/5">5</a></li>
    <li><a href="/foo/page/6">6</a></li>
    <li><a href="/foo/page/7">7</a></li>
    <li class="active"><a href="/foo/page/8">8</a></li>
    <li><a href="/foo/page/9">9</a></li>
    <li><a href="/foo/page/10">10</a></li>
    <li><a href="/foo/page/11">11</a></li>
    <li><a href="/foo/page/12">12</a></li>
    <li class="disabled"><span>...</span></li>
    <li><a href="/foo/page/20">20</a></li>
    <li><a href="/foo/page/9">Next &raquo;</a></li>
</ul>
```

## Rendering a custom pagination control

Use get_pages(), get_next_url(), and get_prev_url() to render a pagination control with your own HTML. For example:

```php
<?php
$total_items = 1000;
$items_per_page = 50;
$current_page = 8;
$url_pattern = '/foo/page/(:num)';

$this->paginator->get_pagination($total_items, $items_per_page, $current_page, $url_pattern);
?>
<ul class="pagination">
    <?php if ($this->paginator->get_prev_url()): ?>
        <li><a href="<?php echo $this->paginator->get_prev_url(); ?>">&laquo; Previous</a></li>
    <?php endif; ?>

    <?php foreach ($this->paginator->get_pages() as $page): ?>
        <?php if ($page['url']): ?>
            <li <?php echo $page['is_current'] ? 'class="active"' : ''; ?>>
                <a href="<?php echo $page['url']; ?>"><?php echo $page['num']; ?></a>
            </li>
        <?php else: ?>
            <li class="disabled"><span><?php echo $page['num']; ?></span></li>
        <?php endif; ?>
    <?php endforeach; ?>

    <?php if ($this->paginator->get_next_url()): ?>
        <li><a href="<?php echo $this->paginator->get_next_url(); ?>">Next &raquo;</a></li>
    <?php endif; ?>
</ul>

<p>
    <?php echo $this->paginator->get_total_items(); ?> found.

    Showing
    <?php echo $this->paginator->get_current_page_first_item(); ?>
    -
    <?php echo $this->paginator->get_current_page_last_item(); ?>.
</p>
```

## Pages data structure

*$this->paginator->get_pages()*

get_pages() returns a data structure like the following:

```php
<?php
array (
    array ('num' => 1,     'url' => '/foo/page/1',  'isCurrent' => false),
    array ('num' => '...', 'url' => NULL,           'isCurrent' => false),
    array ('num' => 5,     'url' => '/foo/page/5',  'isCurrent' => false),
    array ('num' => 6,     'url' => '/foo/page/6',  'isCurrent' => false),
    array ('num' => 7,     'url' => '/foo/page/7',  'isCurrent' => false),
    array ('num' => 8,     'url' => '/foo/page/8',  'isCurrent' => true),
    array ('num' => 9,     'url' => '/foo/page/9',  'isCurrent' => false),
    array ('num' => 10,    'url' => '/foo/page/10', 'isCurrent' => false),
    array ('num' => 11,    'url' => '/foo/page/11', 'isCurrent' => false),
    array ('num' => 12,    'url' => '/foo/page/12', 'isCurrent' => false),
    array ('num' => '...', 'url' => NULL,           'isCurrent' => false),
    array ('num' => 20,    'url' => '/foo/page/20', 'isCurrent' => false),
)
```

## Customizing the number of pages shown

By default, no more than 10 pages are shown, including the first and last page, with the overflow replaced by ellipses. To change the default number of pages:

*$this->paginator->set_max_pages_to_show(5);*
