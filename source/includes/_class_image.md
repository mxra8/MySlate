# Image Class

CodeDmx has a class to manipulate images in PHP as simple as possible.
With this class, we can do:

1. Resize images (free resize, resize to width, resize to height, resize to fit)
2. Crop images
3. Flip / rotate / adjust orientation
4. Adjust brightness and contrast
5. Desaturate, colorize, pixelate, blur, etc
6. Overlay one image onto another (Watermarking)
7. Add text using a font of your choice
8. Convert between GIF, JPG, and PNG formats

## First of all: A simple example

In the first line we try to load the image.
In the second line we shrink it to fit within a 300x250 box, and save it to another path.

```php
<?php
$this->load->library('image');
$this->image->from_file('image.jpg')
			->best_fit(300, 250)
			->to_file('/path/to/new/image.png', 'image/png');
```

## Saving

It's necessary to save the image after you manipulate it.

```php
<?php
// Or you can specify a new filename
$this->image->to_file('new-image.jpg', 'image/jpeg');

// Also, you can specify quality as a second parameter in percents within range 0 - 100
$this->image->to_file('new-image.jpg', 'image/jpeg', 90);
```

## Converting between Formats

When you try to save it, you can determined by the file extension.

```php
<?php
$this->image->from_file('/path/to/image.jpg')
			->to_file('/path/to/new/image.png', 'image/png');
```

## Method Chaining

This class supports method chaining, so you can make multiple changes and save the resulting image with just one line of code:

```php
<?php
$this->image->from_file('/path/to/image.jpg')
			->flip('x')
			->rotate(90)
			->best_fit(300, 250)
			->desaturate()
			->invert()
			->to_file('/path/to/new-image.jpg', 'image/jpeg');
// You can chain all of the methods below as well methods above.
```

## Loaders

### `from_data_uri($uri)`

- `$uri`* (string) - A data URI.

### `from_file($file)`

Loads an image from a file.

- `$file`* (string) - The image file to load.

### `from_new($width, $height, $color)`

Creates a new image.

- `$width`* (int) - The width of the image.
- `$height`* (int) - The height of the image.
- `$color` (string|array) - Optional fill color for the new image (default 'transparent').

### `from_string($string)`

Creates a new image from a string.

- `$string`* (string) - The raw image data as a string. Example:
  ```
  $string = file_get_contents('image.jpg');
  ```

## Savers

### `to_data_uri($mime_type, $quality)`

Generates a data URI.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

Returns a string containing a data URI.

### `to_download($filename, $mime_type, $quality)`

Forces the image to be downloaded to the clients machine. Must be called before any output is sent to the screen.

- `$filename`* (string) - The filename (without path) to send to the client (e.g. 'image.jpeg').
- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

### `to_file($file, $mime_type, $quality)`

Writes the image to a file.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

### `to_screen($mime_type, $quality)`

Outputs the image to the screen. Must be called before any output is sent to the screen.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

### `to_string($mime_type, $quality)`

Generates an image string.

- `$mime_type` (string) - The image format to output as a mime type (defaults to the original mime type).
- `$quality` (int) - Image quality as a percentage (default 100).

## Utilities

### `get_aspect_ratio()`

Gets the image's current aspect ratio.

Returns the aspect ratio as a float.

### `get_exif()`

Gets the image's exif data.

Returns an array of exif data or null if no data is available.

### `get_height()`

Gets the image's current height.

Returns the height as an integer.

### `get_mime_type()`

Gets the mime type of the loaded image.

Returns a mime type string.

### `get_orientation()`

Gets the image's current orientation.

Returns a string: 'landscape', 'portrait', or 'square'

### `get_width()`

Gets the image's current width.

Returns the width as an integer.

## Manipulation

### `auto_orient()`

Rotates an image so the orientation will be correct based on its exif data. It is safe to call this method on images that don't have exif data (no changes will be made).

### `best_fit($max_width, $max_height)`

Proportionally resize the image to fit inside a specific width and height.

- `$max_width`* (int) - The maximum width the image can be.
- `$max_height`* (int) - The maximum height the image can be.

### `crop($x1, $y1, $x2, $y2)`

Crop the image.

- $x1 - Top left x coordinate.
- $y1 - Top left y coordinate.
- $x2 - Bottom right x coordinate.
- $y2 - Bottom right x coordinate.

### `flip($direction)`

Flip the image horizontally or vertically.

- `$direction`* (string) - The direction to flip: x|y|both

### `max_colors($max, $dither)`

Reduces the image to a maximum number of colors.

- `$max`* (int) - The maximum number of colors to use.
- `$dither` (bool) - Whether or not to use a dithering effect (default true).

### `overlay($overlay, $anchor, $opacity, $x_offset, $y_offset)`

Place an image on top of the current image.

- `$overlay`* (string|SimpleImage) - The image to overlay. This can be a filename, a data URI, or a SimpleImage object.
- `$anchor` (string) - The anchor point: 'center', 'top', 'bottom', 'left', 'right', 'top left', 'top right', 'bottom left', 'bottom right' (default 'center')
- `$opacity` (float) - The opacity level of the overlay 0-1 (default 1).
- `$x_offset` (int) - Horizontal offset in pixels (default 0).
- `$y_offset` (int) - Vertical offset in pixels (default 0).

### `resize($width, $height)`

Resize an image to the specified dimensions. If only one dimension is specified, the image will be resized proportionally.

- `$width`* (int) - The new image width.
- `$height`* (int) - The new image height.

### `rotate($angle, $background_color)`

Rotates the image.

- `$angle`* (int) - The angle of rotation (-360 - 360).
- `$background_color` (string|array) - The background color to use for the uncovered zone area after rotation (default 'transparent').

### `text($text, $options, &$boundary)`

Adds text to the image.

- `$text*` (string) - The desired text.
- `$options` (array) - An array of options.
  - `font_file`* (string) - The TrueType (or compatible) font file to use.
  - `size` (int) - The size of the font in pixels (default 12).
  - `color` (string|array) - The text color (default black).
  - `anchor` (string) - The anchor point: 'center', 'top', 'bottom', 'left', 'right',
    'top left', 'top right', 'bottom left', 'bottom right' (default 'center').
  - `x_offset` (int) - The horizontal offset in pixels (default 0).
  - `y_offset` (int) - The vertical offset in pixels (default 0).
  - `shadow` (array) - Text shadow params.
    - `x`* (int) - Horizontal offset in pixels.
    - `y`* (int) - Vertical offset in pixels.
    - `color`* (string|array) - The text shadow color.
- `$boundary` (array) - If passed, this variable will contain an array with coordinates that
  surround the text: [x1, y1, x2, y2, width, height]. This can be used for calculating the
  text's position after it gets added to the image.

### `thumbnail($width, $height, $anchor)`

Creates a thumbnail image. This function attempts to get the image as close to the provided dimensions as possible, then crops the remaining overflow to force the desired size. Useful for generating thumbnail images.

- `$width`* (int) - The thumbnail width.
- `$height`* (int) - The thumbnail height.
- `$anchor` (string) - The anchor point: 'center', 'top', 'bottom', 'left', 'right', 'top left', 'top right', 'bottom left', 'bottom right' (default 'center').

##  Drawing

### `arc($x, $y, $width, $height, $start, $end, $color, $thickness)`

Draws an arc.

- `$x`* (int) - The x coordinate of the arc's center.
- `$y`* (int) - The y coordinate of the arc's center.
- `$width`* (int) - The width of the arc.
- `$height`* (int) - The height of the arc.
- `$start`* (int) - The start of the arc in degrees.
- `$end`* (int) - The end of the arc in degrees.
- `$color`* (string|array) - The arc color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `border($color, $thickness)`

Draws a border around the image.

- `$color`* (string|array) - The border color.
- `$thickness` (int) - The thickness of the border (default 1).

### `dot($x, $y, $color)`

Draws a single pixel dot.

- `$x`* (int) - The x coordinate of the dot.
- `$y`* (int) - The y coordinate of the dot.
- `$color`* (string|array) - The dot color.

### `ellipse($x, $y, $width, $height, $color, $thickness)`

Draws an ellipse.

- `$x`* (int) - The x coordinate of the center.
- `$y`* (int) - The y coordinate of the center.
- `$width`* (int) - The ellipse width.
- `$height`* (int) - The ellipse height.
- `$color`* (string|array) - The ellipse color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `fill($color)`

Fills the image with a solid color.

- `$color` (string|array) - The fill color.

### `line($x1, $y1, $x2, $y2, $color, $thickness)`

Draws a line.

- `$x1`* (int) - The x coordinate for the first point.
- `$y1`* (int) - The y coordinate for the first point.
- `$x2`* (int) - The x coordinate for the second point.
- `$y2`* (int) - The y coordinate for the second point.
- `$color` (string|array) - The line color.
- `$thickness` (int) - The line thickness (default 1).

### `polygon($vertices, $color, $thickness)`

Draws a polygon.

- `$vertices`* (array) - The polygon's vertices in an array of x/y arrays. Example:
  ```
  [
    ['x' => x1, 'y' => y1],
    ['x' => x2, 'y' => y2],
    ['x' => xN, 'y' => yN]
  ]
  ```
- `$color`* (string|array) - The polygon color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `rectangle($x1, $y1, $x2, $y2, $color, $thickness)`

Draws a rectangle.

- `$x1`* (int) - The upper left x coordinate.
- `$y1`* (int) - The upper left y coordinate.
- `$x2`* (int) - The bottom right x coordinate.
- `$y2`* (int) - The bottom right y coordinate.
- `$color`* (string|array) - The rectangle color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

### `rounded_rectangle($x1, $y1, $x2, $y2, $radius, $color, $thickness)`

Draws a rounded rectangle.

- `$x1`* (int) - The upper left x coordinate.
- `$y1`* (int) - The upper left y coordinate.
- `$x2`* (int) - The bottom right x coordinate.
- `$y2`* (int) - The bottom right y coordinate.
- `$radius`* (int) - The border radius in pixels.
- `$color`* (string|array) - The rectangle color.
- `$thickness` (int|string) - Line thickness in pixels or 'filled' (default 1).

## Filters

### `blur($type, $passes)`

Applies the blur filter.

- `$type` (string) - The blur algorithm to use: 'selective', 'gaussian' (default 'gaussian').
- `$passes` (int) - The number of time to apply the filter, enhancing the effect (default 1).

### `brighten($percentage)`

Applies the brightness filter to brighten the image.

- `$percentage`* (int) - Percentage to brighten the image (0 - 100).

### `colorize($color)`

Applies the colorize filter.

- `$color`* (string|array) - The filter color.

### `contrast($percentage)`

Applies the contrast filter.

- `$percentage`* (int) - Percentage to adjust (-100 - 100).

### `darken($percentage)`

Applies the brightness filter to darken the image.

- `$percentage`* (int) - Percentage to darken the image (0 - 100).

### `desaturate()`

Applies the desaturate (grayscale) filter.

### `duotone($light_color, $dark_color)`

Applies the duotone filter to the image.

- `$light_color`* (string|array) - The lightest color in the duotone.
- `$dark_color`* (string|array) - The darkest color in the duotone.

### `edge_detect()`

Applies the edge detect filter.

### `emboss()`

Applies the emboss filter.

### `invert()`

Inverts the image's colors.

### `opacity()`

Changes the image's opacity level.

- `$opacity`* (float) - The desired opacity level (0 - 1).

### `pixelate($size)`

Applies the pixelate filter.

- `$size` (int) - The size of the blocks in pixels (default 10).

### `sepia()`

Simulates a sepia effect by desaturating the image and applying a sepia tone.

### `sharpen()`

Sharpens the image.

### `sketch()`

Applies the mean remove filter to produce a sketch effect.

## Color utilities

### `(static) adjust_color($color, $red, $green, $blue, $alpha)`

Adjusts a color by increasing/decreasing red/green/blue/alpha values independently.

- `$color`* (string|array) - The color to adjust.
- `$red`* (int) - Red adjustment (-255 - 255).
- `$green`* (int) - Green adjustment (-255 - 255).
- `$blue`* (int) - Blue adjustment (-255 - 255).
- `$alpha`* (float) - Alpha adjustment (-1 - 1).

### `(static) darken_color($color, $amount)`

Darkens a color.

- `$color`* (string|array) - The color to darken.
- `$amount`* (int) - Amount to darken (0 - 255).

### `extract_colors($count = 10, $backgroundColor = null)`

Extracts colors from an image like a human would do.™ This method requires the third-party library \League\ColorExtractor. If you're using Composer, it will be installed for you automatically.

- `$count` (int) - The max number of colors to extract (default 5).
- `$backgroundColor` (string|array) - By default any pixel with alpha value greater than zero will be discarded. This is because transparent colors are not perceived as is. For example, fully transparent black would be seen white on a white background. So if you want to take transparency into account, you have to specify a default background color.

### `get_colorAt($x, $y)`

Gets the RGBA value of a single pixel.

- `$x`* (int) - The horizontal position of the pixel.
- `$y`* (int) - The vertical position of the pixel.

### `(static) lighten_color($color, $amount)`

Lightens a color.

- `$color`* (string|array) - The color to lighten.
- `$amount`* (int) - Amount to darken (0 - 255).

### `(static) normalize_color($color)`

Normalizes a hex or array color value to a well-formatted RGBA array.

- `$color`* (string|array) - A CSS color name, hex string, or an array [red, green, blue, alpha].

You can pipe alpha transparency through hex strings and color names. For example:

  #fff|0.50 <-- 50% white
  red|0.25 <-- 25% red
