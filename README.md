# Process Images

A basic, proof-of-concept Textformatter module for ProcessWire. When the Textformatter is applied to a rich text field it uses [Simple HTML DOM](https://simplehtmldom.sourceforge.io/) to find `<img>` tags in the field value and passes each `img` node through a hookable `TextformatterProcessImages::processImg()` method.

This is a very simple module that doesn't have any configurable settings and doesn't do anything to the field value unless you hook the `TextformatterProcessImages::processImg()` method.

## Hook example

When added to `/site/ready.php` the hook below will replace any Pageimages in a rich text field with a 250px square variation and wrap the `<img>` tag in a link to the original full-size image.

For help with Simple HTML DOM refer to its [documentation](https://simplehtmldom.sourceforge.io/manual.htm).

```php
$wire->addHookAfter('TextformatterProcessImages::processImg', function(HookEvent $event) {

    // The Simple HTML DOM node for the <img> tag
    /** @var \simple_html_dom_node $img */
    $img = $event->arguments(0);
    // The Pageimage in the <img> src, if any (will be null for external images)
    /** @var Pageimage $pageimage */
    $pageimage = $event->arguments(1);
    // The Page object in case you need it
    /** @var Page $page */
    $page = $event->arguments(2);
    // The Field object in case you need it
    /** @var Field $field */
    $field = $event->arguments(3);

    // Only for images that have a src corresponding to a PW Pageimage
    if($pageimage) {
        // Set the src to a 250x250 variation
        $img->src = $pageimage->size(250,250)->url;
        // Wrap the img in a lightbox link to the original
        $img->outertext = "<a class='lightboxclass' href='{$pageimage->url}'>{$img->outertext}</a>";
    }
    
});
```
