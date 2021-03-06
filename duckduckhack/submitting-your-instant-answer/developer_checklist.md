# Developer Checklist

Before submitting your Instant Answer, please go over this checklist to make sure everything is ready and has been done correctly.

## General Questions

(Know what should go here? Then **please** [contribute to the documentation](https://github.com/duckduckgo/duckduckgo-documentation/blob/master/CONTRIBUTING.md)!)

- Is it well documented? Make sure you comment your code well so that it's easier for other people to maintain.

- Did you write any custom CSS?
    - Did you namespace the CSS? Every Instant Answer should be contained in a `<div>` with a unique id. This id should be used to target your styles and avoid overwriting any global styles.

## Goodie

(Know what should go here? Then **please** [contribute to the documentation](https://github.com/duckduckgo/duckduckgo-documentation/blob/master/CONTRIBUTING.md)!)

- Can this Instant Answer return unsafe content (bad words, etc.)
    - Did you set `is_unsafe` to true?

- Can this Instant Answer return an HTML response?
    - Have you guaranteed that the response does not contain unsanitized user-supplied strings (e.g., the query string) which could lead to [cross-site scripting attacks](https://duckduckgo.com/Cross-site_scripting?ia=about)?

## Spice

(Know what should go here? Then **please** [contribute to the documentation](https://github.com/duckduckgo/duckduckgo-documentation/blob/master/CONTRIBUTING.md)!)

- Can this Instant Answer return unsafe content (bad words, etc.)
    - Did you set `is_unsafe` to true?

## Fathead

(Know what should go here? Then **please** [contribute to the documentation](https://github.com/duckduckgo/duckduckgo-documentation/blob/master/CONTRIBUTING.md)!)

## Longtail

(Know what should go here? Then **please** [contribute to the documentation](https://github.com/duckduckgo/duckduckgo-documentation/blob/master/CONTRIBUTING.md)!)
