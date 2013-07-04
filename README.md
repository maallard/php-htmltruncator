PHP HTML Truncator
==================

This is a PHP port of the [html_truncator gem](https://github.com/nono/HTML-Truncator).

It will cleanly truncate HTML content, appending an ellipsis or other marker in the most
suitable place.
There is optional support for [php-html5lib](https://github.com/electrolinux/php-html5lib) if parsing
of non-well-formed HTML5 is required. This is because PHP's DOMDocument->loadHTML (or the underlying libxml) doesn't recognize HTML5.


How to use it
-------------

The HtmlTruncator\Truncator class has only one static method, `truncate`, with 3 arguments:

* the HTML-formatted string to truncate
* the number of words to keep (real words, tags and attributes aren't counted)
* some options like the ellipsis (optional, '…' by default). If the option is string it is used as the ellipsis

And a static attribute, `$ellipsable_tags`, which lists the tags that can contain the ellipsis
(by default: p ol ul li div header article nav section footer aside dd dt dl).

Examples
--------

A simple example:

    Truncator::truncate("<p>Lorem ipsum dolor sit amet.</p>", 3);
    # => "<p>Lorem ipsum dolor…</p>"

If the text is too short to be truncated, it won't be modified:

    Truncator::truncate("<p>Lorem ipsum dolor sit amet.</p>", 5);
    # => "<p>Lorem ipsum dolor sit amet.</p>"

If you prefer, you can have the length in characters instead of words:

    Truncator::truncate("<p>Lorem ipsum dolor sit amet.</p>", 12, array('length_in_chars' => true));
    # => "<p>Lorem ipsum …</p>"

You can customize the ellipsis:

    Truncator::truncate("<p>Lorem ipsum dolor sit amet.</p>", 3, " (truncated)");
    # => "<p>Lorem ipsum dolor (truncated)</p>"

And even have HTML in the ellipsis:

    Truncator::truncate("<p>Lorem ipsum dolor sit amet.</p>", 3, '<a href="/more-to-read">...</a>');
    # => "<p>Lorem ipsum dolor<a href="/more-to-read">...</a></p>"

The ellipsis is put at the right place, inside `<p>`, but not `<i>`:

    Truncator::truncate("<p><i>Lorem ipsum dolor sit amet.</i></p>", 3);
    # => "<p><i>Lorem ipsum dolor</i>…</p>"

You can indicate that a tag can contain the ellipsis by adding it to the ellipsable_tags:

    Truncator::$ellipsable_tags[] = "blockquote";
    Truncator::truncate("<blockquote>Lorem ipsum dolor sit amet.</blockquote>", 3);
    # => "<blockquote>Lorem ipsum dolor…</blockquote>"


Credits
-------

This PHP port is entirely based on [Bruno Michel's excellent rubygem](https://github.com/nono/HTML-Truncator), any
bugs introduced are likely mine.

Ported 2013-07-04 by Jude Venn <judev@cuttlefish.com>, released under the MIT license
