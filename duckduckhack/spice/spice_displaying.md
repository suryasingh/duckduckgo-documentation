# Adding Your Spice to the DuckDuckGo AnswerBar

Once your Instant Answer has been triggered, and the API request has returned a response to the client, the final step is to display your results onscreen.

![answerbar](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Fdiagrams%2Fanswerbar.png&f=1)

## Contents of the Spice Frontend Callback

The Spice frontend callback function, [covered in the basic tutorial](https://duck.co/duckduckhack/spice_basic_tutorial#npm-spice-frontend-javascript), contains the code which displays your spice. 

The most important part of this callback, and often the only part, is calling [`Spice.add()`](#codespiceaddcode-overview). This function is powerful and gives you a lot of control over how your results' appearance, context, and user interactions. This document provides an in-depth overview of all that `Spice.add()` allows you to do.

In some (rare) scenarios, you may also want to handle the AnswerBar's events (for example, to stop media playing when the user hides your Instant Answer). These [events](#events) are covered at the end of this page.

------

## `Spice.add()` Overview

```javascript
Spice.add({

	// Required properties:
	
    id: String,
    name: String,
    data: Object,

    meta: {

        hidden: boolean,
        searchTerm: String,
        itemType: String,

        primaryText: String,
        secondaryText: String,

        sourceName: String,
        sourceLogo: String,
        sourceUrl: String (url),
        sourceIcon: Boolean,
        sourceIconUrl: String (url),

		snippetChars: Integer
    },

    templates: {
        group: String,

        item: String|Function,
        item_custom: String|Function,
        item_mobile: String|Function,

        detail: String|Function,
        detail_custom: String|Function,
        detail_mobile: String|Function,
        item_detail: String|Function,

        options: Object

        variants: Object
        
        elClass: Object
    },

    // Optional properties:

    normalize: Function,

    relevancy: {
        type: String,
        skip_words: [, String],
        primary: [, Object],
        dup: String,
    },

    view: String,
    model: String,

    sort_fields: Object,
    sort_default: String|Object,

    onItemSelect: Function,
    onItemUnselect: Function,
    onShow: Function,
    onHide: Function

});
```

### Required Properties

- [id](#codeidcode-string-required) - A unique identifier for your Spice. The `id` should match the name of your callback function
- [name](#codenamecode-string-required) - The name that will be used for your Spice's AnswerBar tab
- [data](#codedatacode-object-required) - The object containing the data to be used by your templates
- [meta](#codemetacode-object-required) - Used to define elements of the **MetaBar** including the "More at" link
- [templates](#codetemplatescode-object-required) - Used to specify the template group and all other templates that are being used

### Optional Properties

- [normalize](#codenormalizecode-function-optional) - This allows you to normalize the `data` before it is passed on to the template
- [relevancy](#coderelevancycode-object-optional) - Used to ensure the relevancy of your Spice's result
- [view](#codeviewcode-string-optional) - This allows you to explicitly specify the view class used for displaying the Instant Answer
- [model](#codemodelcode-string-optional) - This allows you to use one of our predefined data models that include domain specific helpers/normalization/formatting.

------

## `id` *string* [required]

A unique identifier for your Spice. The `id` should match the name of your callback function. For example, if your callback function is named `ddg_spice_name`, your `id` should be `spice_name`.

------

## `name` *string* [required]

The name that will be used for your Spice's AnswerBar tab. The Spice system will determine the final tab name, but it's best to provide a category or topic that describes the kind of information your Spice provides. Here are some examples:

<table>
    <thead>
        <tr>
            <th>Spice IA</th>
            <th><pre>name</pre></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>GitHub</td>
            <td>'Software'</td>
        </tr>
        <tr>
            <td>Last.fm</td>
            <td>'Music'</td>
        </tr>
        <tr>
            <td>HackerNews</td>
            <td>'News'</td>
        </tr>
        <tr>
            <td>Twitter</td>
            <td>'Social'</td>
        </tr>
        <tr>
            <td>Amazon</td>
            <td>'Products'</td>
        </tr>
    </tbody>
</table>

------

## `data` *object* [required]

The object containing the data to be used by your templates. In most cases, it is best to pass along `api_result` to `data`, so that all of your API response is accessible to your templates.

------

## `meta` *object* [required]

The following options are used to define elements of the MetaBar including the "More at" link. 

![metabar example](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Fdiagrams%2Fmetabar.png&f=1)

![metabar item example](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Fdiagrams%2Fmetabar_item.png&f=1)

The following are all properties of the `meta: {}` object.

- ### `sourceName` *string* [required]

    The name of the source as it should be shown in the "More at" link. For example, in "More at Quixey", "Quixey" [is specified](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/quixey/quixey.js#L77) as `sourceName`.

- ### `sourceUrl` *url string* [required]

	The URL to follow when the "More at" link is clicked. This value is the `href` attribute of the "More at" link. This can refer to the main page of the source, or better yet, the specific page relevant to the user's query. 
	
	A secure **https://** connection should be used whenever possible.

	#### Examples
	
	- In [rand_word.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/rand_word/rand_word.js#L14), the `sourceUrl` is a hardcoded address.
	- In [is_it_up.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/is_it_up/is_it_up.js#L15), the `sourceUrl` is dynamically generated to direct to a specific page relating to the search query.

- ### `searchTerm` *string* [optional]

	Determines the **modifier** in the MetaBar's description: "Showing 15 `itemType` for `searchTerm`".
	
    The `searchTerm` is used to describe the `itemType` and it can be determined by removing any skip words from the original query.

	For example, searching ["alternatives to emacs"](https://duckduckgo.com/?q=alternative+to+emacs&ia=software), will display the description "Showing 12 Alternatives for **GNU Emacs**" in the MetaBar. In this case, the phrase "GNU Emacs" is the `searchTerm`.
	
	![metabar description](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Fdiagrams%2Fmetabar_description.png&f=1)
	
	If no `searchTerm` is specified, the description will simply read "Showing 12 Movies" or, if no `itemType` specified, "Showing 12 Items".
	
	#### Examples 

	- In [news.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/news/news.js#L89), `searchTerm` is passed the search query after some basic cleanup.
	- In [images.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/images/images.js#L19), `searchTerm` is passed the original query as-is.
	- In [alternative_to.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/alternative_to/alternative_to.js#L16), `searchTerm` is passed a name provided by the API.
	- In [songkick_geteventid.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/songkick/geteventid/songkick_geteventid.js#L38), `searchTerm` is passed a city name.

- ### `itemType` *string* [optional]

    Determines the **noun** in the MetaBar's description: "Showing 15 `itemType` for `searchTerm`". In other words, `itemType` is the type of item being shown (e.g., Videos, Images, Recipes).
	
	Searching for ["alternatives to emacs"](https://duckduckgo.com/?q=alternative+to+emacs&ia=software), "Showing 12 **Alternatives** for GNU Emacs", the word "Alternatives" is the `itemType`.
	
	If no `itemType` is specified, the default itemType is 'Items'. For example, the MetaBar description will read "Showing 12 Items", or "Showing 12 Items for Electronics" when a `searchTerm` is provided.
	
	#### Examples
	
	- In [news.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/news/news.js#L90), the `itemType` is `'News articles'`.
	- In [alternative_to.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/alternative_to/alternative_to.js#L17), the itemType is `'Alternatives'`.

- ### `primaryText` *string* [optional]

    If defined, this text will replace the MetaBar's "Showing **n** Items" text. 

	#### Example

	- In [parking.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/parking/parking.js#L89), the `primaryText` is set to `'Parking near'` plus the location.

- ### `secondaryText` *string* [optional]

    This is an optional text label, displayed to the left of the "More at" link. For example, a weather forecast Spice might use this to indicate the temperature unit, such as "Temperature in F&deg;".

- ### `sourceLogo` *url string* [optional]

    If defined, the image provided will replace the `sourceName` with a logo. Generally this is not necessary; in rare cases, API providers require that a specific image be used to represent their brand.

	#### Example
	
	From [quixey.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/quixey/quixey.js#L79):
	
    ```javascript
    meta:{
        ...
        sourceLogo: {
            url: DDG.get_asset_path('quixey','quixey_logo.png'),
            width: '45',
            height: '12'
        }
    }
    ```
	
	This is the [result](https://duckduckgo.com/?q=money+apps&ia=apps):
	
	![sourcelogo](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Fdiagrams%2Fsourcelogo.png&f=1)

- ### `sourceIcon` *boolean* [optional]

    A boolean flag that determines if a favicon should be shown to the left of the "More at" link. The favicon is determined automatically by DuckDuckGo based on the `sourceUrl`. 

	When a `sourceUrl` is given, this will default to `true`. It should only be set to `false` when the `sourceUrl` domain does not have a favicon.

- ### `sourceIconUrl` *url string* [optional]

    If the `sourceUrl` domain has no favicon (or if a different favicon is preferred), you can explicitly set a url path for the favicon shown to the left of the "More at" link. This value, if set, will take precedence any favicons pulled from the `sourceUrl` domain.

- ### `snippetChars` *integer* [optional]

    For blocks of text that require truncation, `snippetChars` allows you to specify the maximum number of characters before truncation (to whole words with ellipses). This applies mainly to `description` elements in templates.

	This property is expected to be used in rare cases. Each template comes with its own optimal, default `snippetChars` value.

------

## `templates` *object* [required]

A `templates: {}` property should be used to specify the [template group](https://duck.co/duckduckhack/spice_templates_overview#template-groups-reference) and all other templates that are being used. Template options can also be provided to enable or disable features depending on the chosen template group.

More about how templates work can be found in the [template overview](https://duck.co/duckduckhack/spice_templates_overview).

<!-- /summary -->

- ### `group` *string* [required, unless `item` and `detail` are specified]

    Used to specify the base template (layout) to be used. Each template `group` is composed of several features. The template groups available are described in the [template overview](https://duck.co/duckduckhack/spice_templates_overview).

    This will tell the template system that the templates belonging to the given group will be used for the `item`, `detail`, etc. unless otherwise manually overridden.

    For example, `group: 'info'` will implicitly set:

    ```javascript
    templates:{
        item: 'basic_item',
        item_detail: 'basic_info_item_detail',
        detail: 'basic_info_detail'
    }
    ```

- ### `item` *string or function* [required if no `group` is specified]

    The template to be used for the body of each tile in a tile view.

    **Note:** The `item` template is only used when your Spice Instant Answer returns multiple items (like the recipe or app Instant Answers), meaning the object given to `data` is an *`array`* with more than 1 elements.

    - Generally, a string is provided to indicate the name of the built-in Spice template to be used, e.g., "products_item"

    - In rare cases, where necessary, a function referencing a custom template can be passed. Passing a custom template is a measure of last resort due to maintenance difficulty. Learn more about [picking templates](https://duck.co/duckduckhack/spice_templates_overview#picking-a-template-group); if you feel that no current templates fit your idea, please contact us at [open@duckduckgo.com](mailto:open@duckduckgo.com) and we'll happily help you find a solution.

- ### `item_mobile` *string or function* [optional]

    An alternative `item` template to be used when displaying on smaller screens, such as mobile and hand-held devices.

- ### `detail` *string or function* [required if no `group` is specified]

    The template to be used for the detail area.

    *For multiple items*, the detail area will be located below the tiles, and will display when a tile is clicked. If your Spice returns multiple items, the `detail` template is **optional**.

    *For a single item*, the detail area will be right below the AnswerBar and will display instantly. If your Spice always returns a single item, only a `detail` template is **required**.

    **Note:** The `detail` templates is **optional for a tile view** and should only be used to provide additional information for each tile.

- ### `detail_mobile` *string or function* [optional]

    An alternative `detail` template to be used when displaying on smaller screens, such as mobile and hand-held devices.

- ### `item_detail` *string or function* [optional]

    An alternative `detail` template to be used when a tile is clicked. Learn more about when `item_detail` is used in the [templates overview](https://duck.co/duckduckhack/spice_templates_overview#clicking-on-an-item).

- ### `options` *object* [optional]

    Allows you to explicitly disable or enable the [available features](https://duck.co/duckduckhack/spice_templates_reference) of your template using boolean values or references to sub-templates. 

	For example, you might set the the `content` feature of the [`basic_info_detail`](https://duck.co/duckduckhack/spice_templates_reference#codebasicinfodetailcode-template) template to a particular sub-template. 
	
	For example:
	
	```javascript
    templates: {
        group: 'info',
        options: {
            content: 'record',
            rowHighlight: true
        }
    }
	```

	Available features will vary with each chosen template (see the [templates reference](https://duck.co/duckduckhack/spice_templates_reference) for details on each template). For example, the `basic_info_detail` template doesn't have a `brand` feature, so setting `brand: true` or `brand: false` will have no effect.
	
	These implicit [default options](https://duck.co/duckduckhack/spice_templates_overview#a-note-on-default-template-options) apply when neither an `options` object nor a templates `group` are set.

- ### `variants` *object* [optional]

	If you'd like to modify a template to fit your needs, the Spice framework offers preset options called [Variants](https://duck.co/duckduckhack/spice_templates_reference#spice-variants). Variants are passed as the `variants` property of `templates`. Variants correspond to pre-determined css classes (or combinations of classes) from the [DDG style guide](https://duckduckgo.com/styleguide) that work particularly well in each context.

- ### `elClass` *object* [optional]

	When variants don't suffice, you can [directly choose classes](https://duck.co/duckduckhack/spice_templates_variants#directly_specifying_classes) based on the [DDG style guide](https://duckduckgo.com/styleguide) through the `elClass` property of `templates`. **This feature is mainly used for specifying text size and color.**

	Classes can be directly specified to the same elements as Variants; the locations are identical. If you are specifying both `variants` and `elClass`, both will be applied together.

------

## `normalize` *function* [optional]

Specifying this optional function allows you to normalize each item in the `data` object before it is used by the template. You can use this function to rename properties, add new properties, or modify values.

This function is applied both for single results or multiple results. When dealing with multiple items returned in the `data` object, the normalize function iterates over each item. Each item is individually normalized and passed to its own instance of the `item` template.

### Usage

The function set for `normalize` is expected to return an object with the properties to be rendered for each item. 

The object returned by the `normalize` function is *combined* with the original respective item from `data`.  

The `normalize` function will **extend the original `data` item** instead of replacing it. (This uses jQuery's `$.extend()` method, shallow copy.) Each normalized item will contain its original properties unless they were explicitly overwritten in the `normalize` function.

#### Use with Built-In Templates

Normalize can be particularly useful if you are using a [built-in template](https://duck.co/duckduckhack/spice_templates_reference#spice-templates) (for example, `basic_image_item`). 

Built-in templates expect that certain properties will be present (such as `title` and `image`). The `normalize` function allows you to provide those (or normalize their values if the property already existed in your `data`).

For example, if you have a `data` object that looks like this:

```javascript
// original data object from API
{
    heading: "My awesome title",
    image:   "http://website.com/image.png"
}
```

You will likely want to pass the `heading` property as the `title` for the **basic_image_item** template, so your `normalize` function would need to look like this:

```javascript
normalize: function(item){
    return {
        title: item.heading
    };
}
```

This would result in your `data` object looking like this once it gets passed along to the template:

```javascript
// normalized data object
{
    heading: "My awesome title",
    image:   "http://website.com/image.png",
    title:   "My awesome title"
}
```

Now, your object has all the required properties for the **basic_image_item** template and everything will be displayed as expected.

#### Important Note on Enabling Features

If you intend to use a [template feature](https://duck.co/duckduckhack/spice_templates_reference) that is [disabled by default](https://duck.co/duckduckhack/spice_templates_overview#a-note-on-default-template-options) or [disabled by a template group defaults](https://duck.co/duckduckhack/spice_templates_overview#template-groups-reference), that feature must be **enabled in the `options`** to display.

Even if the property exists in the `data` object, the template system will ignore it if the feature is disabled. 

In this example:

```javascript
normalize: function(item){
    return {
        // these won't work!
        rating: item.customerRating,
        brand: item.brandname
    };
},
templates: {
    group: 'products_simple'
}
```

The template **will *not* display** the `rating` or `brand` as they are **disabled** by default in the `products_simple` group default options. Only once they are explicitly enabled in an `options` object will they work:

```javascript
normalize: function(item){
    return {
        // now they'll show, because they're enabled below
        rating: item.customerRating,
        brand: item.brandname
    };
},
templates: {
    group: 'products_simple',
    options: {
        rating: true,
        brand: true
    }
}
```

### `exactMatch` *boolean* and `boost` *boolean*

Two special properties, `exactMatch` and `boost`, can also be set in the `normalize` function to add particular items to the list of exact matches or boosted items. 

When the tile view displays, the **exact match** items will come **first**, followed by the **boosted** items and then the rest of the items.

Example:

```javascript
normalize: function(item) {
    if (item.name === DDG.get_query()){
        item.exactMatch = true;
    } else if (item.developer.name === DDG.get_query()) {
        item.boost = true;
    }

    return { ... }
}
```

------

## `relevancy` *object* [optional]

When dealing with multiple items, the `relevancy` property can be used to ensure the relevancy of each individual item. It can also be used to de-duplicate the returned items if desired.

In most cases you will only need to specify relevancy properties for the **primary** relevancy block. However, if your Spice is capable of dealing with different types of queries though, where different relevancy checks are necessary, you can supply additional relevancy blocks. 

For example, the Quixey Spice (app search) handles two distinct types of app searches: 

- **Categorical** searches, such as "social networking apps"
- **Named** searches such as "free angry birds apps" 

When dealing with **categorical** searches, the name of the app doesn't need to be checked against the query for relevancy. However, the app's category *does* need to be checked and so two separate relevancy blocks, `primary` and `category`, are used to define the different relevancy constraints.

Sample code from [quixey.js](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/quixey/quixey.js#L129):

```javascript
relevancy: {
    ...
    category: [
        { required: 'icon_url' },
        { key: 'short_desc' },
        { key: 'name' },
        { key: 'custom.features.category', match: category_match_regexp, strict:false } // strict means this key has to contain a category phrase or we reject
    ],

    primary: [
        { required: 'icon_url' }, // would like to add  alt: 'platforms.0.icon_url'
        { key: 'name', strict: false },
    ]
}
```

### Relevancy Blocks

A relevancy block is comprised of an array of simple objects. For each object, the properties are used to indicate certain constraints. The concept of a relevancy block is best explained with an example:

<!-- /summary -->

```javascript
// First we provide the name for the relevancy block, "primary"
primary: [

    { required: 'icon_url' },
    // "required" means the item must have a defined property matching the
    // given name, in this case and "icon_url" must be defined for each Quixey
    // item
    //
    // Note: "required" only ensures the presence of a property. It does NOT
    // perform a relevancy check

    { key: 'name', strict: true },
    // "key" indicates a property which will be checked for relevancy. If
    // the given key is determined to be relevant, the item as a whole is
    // considered relevant and no other keys in the relevancy block are
    // checked for the current item
    //
    // The "strict" key allows you to turn on the "strict mode"
    // for DDG.isRelevant

    { key: 'short_desc' }
    // this is an extra "key" which serves as a fallback. This means if
    // either the 'name' or 'short_desc' or relevant the item is considered
    // relevant
],

// here we provide a secondary relevancy block with a chosen name of "category"
category: [
    { required: 'icon_url' },
    { key: 'name' },
    { key: 'short_desc' },
    { key: 'custom.features.category', match: category_regexp }
    // the "match" key allows you to specify a regular expression which the
    // given "key" must match
    //
    // Note: "match" only ensures the property matches the given regex. It
    // does NOT perform a relevancy check
],
```

**Note:** The relevancy checking is done using the `DDG.isRelevant()` function.

- ### `type` *string*

    The name of the relevancy block to use. If no value is provided, the default `primary` block will be used.

    For example, the Quixey spice determines the `type` based on the query. If the query matches against our category regex (i.e. the query contains a category word), we set `type` to "category", otherwise we use "primary".

- ### `skip_words` *array*

    A list of words to ***skip*** when comparing the specified text against the current query. Generally these words should include any trigger words for your Spice. The skip words list is **not** dependent on the chosen relevancy block.

- ### `primary` *array* [required if using `relevancy`]

    The list of relevancy terms for this particular relevancy block

- ### `<additional_relevancy_block>` *array*

    An additional list of relevancy terms, using the same format as `primary`. This object (and other relevancy blocks) can be named arbitrarily.

- ### `dup` *string*

    This indicates which property should be used to check for de-duplication. The given string supports dot path formatting, e.g., "item.foo.bar"

------

## `sort_fields` *object* [optional]

In some cases, the order of the tiles is important (e.g., price, rating, popularity) and you can use the sorting properties to specify the default ordering of the tiles. You can also specify additional sorting fields that will allow users to re-order the tiles using a different sort method.

This object specifies sorting fields (e.g., name, price, rating, reviews) and their respective comparison functions, which will be passed along to JavaScript's `sort()` method.

Example:

```javascript
sort_fields: {
    name: function(a,b) {
        return a.name < b.name ? -1 : 1;
    },
    rating: function(a,b) {
        return a.rating < b.rating ? -1 : 1;
    }
}
```


### `sort_default` *string or object*

A string specifying the default `sort_field` to be used for initial sorting of the tiles.

```javascript
sort_default: 'name';
```

If you have used **more than one** relevancy block, `sort_default` can be given an `object` specifying the default `sort_field` for each relevancy block.

For example, if we had two relevancy blocks named `primary` and `category` our `default_sort` could look like this:

```javascript
//because we have two relevancy blocks...
sort_default: {
    primary: 'name',
    category: 'rating'
}
```

------

## `view` *string* [optional]

Typically you don't need to specify a view for Instant Answers unless you're using special functionality like the playable Audio tiles for the SoundCloud IA, or the Maps used in our Places IA.

Available Views:

- Audio
- Detail (default view for IAs with a single item)
- Images
- Map
- Places
- Tiles (default view for IAs with multiple items)
- TilesWithTopics
- Videos

------

## `model` *string* [optional]

Typically you don't need to specify a model for your Instant Answer. However, if your data is of a common type, we have pre-built models which accept data to displaying things like Lat/Lon for a Place or Dimensions for an Image. 

Also, to use some of our non-default *views*, such as Audio or Places, you will need to use a compatible data model.

Available models:

- Audio
- Image
- Place
- Product
- Video

More about using models and their properties can be found in their respective [template groups](https://duck.co/duckduckhack/spice_templates_overview#template-groups-reference).

------

## Events

If you need to fire off an event handler when a tile is clicked or when your Spice's tab initially opens, you can handle these events with a callback function.

- ### `onItemSelect` *function*

	This event occurs each time a tile is selected.

	Example:

	```javascript
	onItemSelected: function(item) {
	   player.play(item);
	}
	```

	**Note:** If a tile-view result returns a single result, this event will also fire when the tab is opened/clicked, so you don't need to use both `onItemSelected` and `onShow` to handle the case of a single-result tile view

- ### `onItemUnselect` *function*

	This event occurs each time a tile is unselected.

	**Note:** If a tile-view result returns a single result, this event will also fire when the tab is closed, so you don't need to use both `onItemSelected` and `onShow` to handle the case of a single-result tile view

- ### `onShow` *function*

	This event occurs when a Spice tab initially opens.

- ### `onHide` *function*

	This event occurs when a Spice tab is closed i.e. when another tab is selected.
