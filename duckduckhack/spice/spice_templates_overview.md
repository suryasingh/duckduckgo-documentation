# Spice Templates Overview

When your Instant Answer returns its awesome and delightful result(s), the information is rendered at the top of the DuckDuckGo search results page. The way your results appear and behave is decided by the Spice templates you choose.

![DuckDuckGo search for "garlic steak recipes"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Fgarlic_steak_recipes.png&f=1)

## Why Templates Are Great

Templates save a lot of work: they allow contributors to focus on great results. Many Instant Answer frontends can be created entirely by setting various display options and item data, with little or no HTML/CSS coding.

Additionally, Instant Answers that use templates are automatically compatible with future design improvements, with zero extra work.

## How Templates Work

Templates are handlebars files which render in the context of **one item** returned by the Instant Answer.

The Spice framework provides you with a wide choice of templates to use, as you will see below as well in the [reference](https://duck.co/duckduckhack/spice_templates_reference).

The built-in templates' options, variables, and [variants](https://duck.co/duckduckhack/spice_templates_reference#spice-variants) are documented in the [Spice Templates Reference](https://duck.co/duckduckhack/spice_templates_reference) section.

### Specifying `item` and `detail` Templates

Spice Instant Answers can return either a **single result** or **multiple results**. To provide the best experience, these two cases can be displayed with different templates.

On the [Spice frontend](https://duck.co/duckduckhack/spice_displaying) you can specify two separate templates:

- `item` template (multiple results)
- `detail` template (single results)

Below is an example of multiple results being returned. Each result is displayed using the template specified for `item`: 

![DuckDuckGo search for "seafood maui"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Fseafood_maui.png&f=1)

Below is an example of the same Instant Answer returning a single result. This uses the template specified for `detail`:

![DuckDuckGo search for "longhi's maui"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Flonghis_maui.png&f=1)

### Clicking on an Item

In the case of multiple items, clicking on a single item will show the `detail` template below the items. This is the default behavior. To display a template other than the one used for `detail`, specify an `item_detail` template.

This diagram shows what is displayed when an instant answer returns multiple items:

```
                                    INSTANT ANSWER RETURNS MULTIPLE ITEMS

+--------------+--------------+--------------+--------------+--------------+--------------+---------------+
|              |              |              |              |              |              |               |
|              |              |              |              |              |              |               |
|              |              |              |              |              |              |               |
|              |              |              |              |              |              |               |
|   `item`     |              |              |              |              |              |               |
|   template   |              |              |              |              |              |               |
|   clicked    |              |              |              |              |              |               |
|              |              |              |              |              |              |               |
|              |              |              |              |              |              |               |
|              |              |              |              |              |              |               |
+--------------+--------------+--------------+--------------+--------------+--------------+---------------+
|                                                                                                         |
|                                                                                                         |
|                                                                                                         |
|                                                                                                         |
|                                              `detail` Template                                          |
|                                                                                                         |
|                                              OR                                                         |
|                                                                                                         |
|                                              `item_detail` (when specified)                             |
|                                                                                                         |                                                                                                                       
+---------------------------------------------------------------------------------------------------------+
```

For example, the Amazon products search Instant Answer uses one template for single results (`detail`):

![DuckDuckGo search for "amazon pogs"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Famazon_pogs_detail.png&f=1)

However, the Amazon Instant Answer displays a different template when multiple items are returned, and one is clicked (`item_detail`):

![DuckDuckGo search for "amazon pogs"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Famazon_pogs_item_detail.png&f=1)

#### Disabling Detail Display on Click

To disable the display of a detail template when an item is clicked, set `detail: false`. A side effect of this is that single results will be displayed as tiles.

### When Each Template Is Shown

The Spice framework automatically chooses which template to display based on how many results there are to show and user behavior. Here is the default logic for showing templates:

```
          Instant Answer result      
                   +                     
                   |  
 multiple results  |  single result      
                   |  
         +---------+----------+          
         |                    |          
         |                    |   
         |                    |          
+--------v--------+  +--------v---------+
|                 |  |                  |
|      `item`     |  |      `detail`    |
|                 |  |                  |
+-------+---------+  +------------------+                              
        |                                
        | click an item                   
        |                                
+-------v---------+                      
|                 |                      
|   `detail`      |                      
|       or        |                      
|   `item_detail` |                      
|  (if specified) |
|                 |                     
+-----------------+                      

```

Of course, you can specify template options to modify this; for example, you may want to prevent a particular template from appearing. For example, you might set `detail: false` to make sure your Instant Answer always displays results with the `item` template.

## Template Groups (required)

Template groups are preset properties for `template` and its `options` that work well together. In most cases at least one group will be a good fit for your Instant Answer.

**We strongly recommend using a template group in your Instant Answer.** Of course, if you cannot use an available template for your Instant Answer, definitely let us know. E-mail us at open@duckduckgo.com and we'll help you. We may find that a custom template is necessary, and we'll work with you to create an awesome one. (Who knows, your idea may inspire the next official template group!)

### How Template Groups Work

Setting a template group automatically sets the `item` and `detail` templates for you. Some template groups also set an `item_detail` template and a few default `options`. 

You can easily customize the appearance of the template group by overriding the default `options` in your Spice frontend code. The appearance will also be affected by which data is returned with each item.

### Picking a Template Group

The best template group for your instant answer depends on what your Instant Answer is returning. Below are a few suggestions to help you narrow down your options.

A quick way to get a feel for the different template groups is to [browse the Instant Answer directory](https://duck.co/ia). You can filter by the template group used on the right of the page.

#### My Spice returns "things" where visuals are important 

The [Media](#media-template-group) template group works well when an image is a significant part of the display of an item, as might be a title and a rating. 

Examples that make a great fit for `media` include:

- TV shows/[Movies](https://duckduckgo.com/?q=currently+in+theaters)
- Games
- [Courses](https://duckduckgo.com/?q=computer+science+online+course)

#### My Spice returns detailed "lookup" information

The [Info](#info-template-group) template group is designed for Instant Answers that feature in-depth information about one item. It also provides an auxiliary section to display further detail in table or list format. 

Examples include:

- [Recipes](https://duckduckgo.com/?q=how+to+mix+a+tom+collins&ia=recipes)
- [Cards](https://duckduckgo.com/?q=mtg+Boros+Reckoner&ia=magicthegathering)
- [Books](https://duckduckgo.com/?q=book+reviews+moonwalking+with+einstein&ia=books)
- [Bands](https://duckduckgo.com/?q=weezer+band)
- [Addresses](https://duckduckgo.com/?q=1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa)
- [Characters](https://duckduckgo.com/?q=bulbasaur+pokemon)

The [List](#list-template-group) template group works well for lookups that don't need an image; this group is excellent for displaying attributes in just a list or table format. Take a look at the following example:

- [WhoIs](https://duckduckgo.com/?q=whois+google.com)

#### My Spice results are mainly text, with a possible icon or logo

The [Text](#text-template-group) and [Icon](#icon-template-group) template groups are simple templates for presenting text results. They both share the same `item` template, while the Icon group's `detail` template is better suited to displaying an icon image. 

These results fit this format well:

- [Software](https://duckduckgo.com/?q=alternative+to+notepad&ia=software)
- [Code blocks](https://duckduckgo.com/?q=Fizz+Buzz+in+C&ia=codesnippet)
- [Jobs](https://duckduckgo.com/?q=perl+jobs+in+san+francisco&ia=jobs)
- [Records](https://duckduckgo.com/?q=bugid+234&ia=launchpad)
- [Simple answers](https://duckduckgo.com/?q=namecheap+http%3A%2F%2Finstantanswerparty.com&ia=domain)
- [Long blocks of text](https://duckduckgo.com/?q=baconipsum+4&ia=baconipsum)

#### My Spice returns products with prices, ratings, and brands/authors/artists

The [Products](#product-template-group) template group is great for items characterized by a price, brand, and rating. This is a good template group where images are important. 

Examples of results that work well with the Products template group include:

- [Magazines issues](https://duckduckgo.com/?q=newint&ia=magazines)
- [Parts](https://duckduckgo.com/?q=ne555+specs&ia=parts)
- [Apps](https://duckduckgo.com/?q=tiny+piano+for+iphone&ia=apps)
- [Events](https://duckduckgo.com/?q=live+show+weezer&ia=concerts)
- [Books](https://duckduckgo.com/?q=amazon+ray+bradbury&ia=products)
- [Physical products](https://duckduckgo.com/?q=amazon+ninja+costume&ia=products)

#### My Spice returns location-based results

The [Places](#places-template-group) template group is perfect for results where location is an important aspect. This template group displays single and multiple items on a map.

Results that would make a good fit for the Places template group include:

- [Parking](https://duckduckgo.com/?q=parking+in+philadelphia&ia=parking)
- [Local businesses](https://duckduckgo.com/?q=restaurant+in+laguna+beach)
- Hiking trails
- Historical sites
- Surf spots
- Shark GPS locations

#### My Spice is amazingly unique and existing template groups won't meet my needs

We encourage you to think hard about using an existing template group. For example, many `detail` templates accept custom handlebars sub-templates. Additionally, many template features can be toggled. 

If working within existing template groups feels too constraining, we're happy to help you figure out how your vision could be accomplished using existing templates. **E-mail us at [open@duckduckgo.com](mailto:open@duckduckgo.com) and we'll work with you to find the best way to express your idea.**

In this context, the [Base](#base-template-group) template group is a minimal container template that accepts totally custom markup. Because of this, the Base template group is **a complete last-resort** because of the amount of work up front, and difficult maintenance over time. 

**Please hold off from using the Base template group until you've contacted us.** We hope to save you from ongoing, manual maintenance of your Instant Answer display. Additionally, knowing where our templates fall short helps us understand where we can improve our existing set of templates.

<!--
Examples of Instant Answers that do not fit into any template groups:

- [Ducksay](https://duckduckgo.com/?q=ducksay+quack!&ia=ducksay)
- [Flight itineraries](https://duckduckgo.com/?q=Jetblue+Boston+to+Los+Angeles&ia=route)
- [Site functionality](https://duckduckgo.com/?q=is+reddit.com+working%3F&ia=answer)
- [Nutrition facts](https://duck.co/ia/view/nutrition)
- [Webcomics](https://duck.co/ia/view/xkcd_display)
-->

------

## Template Groups Reference

This reference explains what each template group looks like, how it works, and what content works fits it best. Each template group is accompanied by live examples, layout diagrams, code links, and available features.

These are the currently available template groups:

- [Text](#text-template-group)
- [Info](#info-template-group)
- [Products](#products-template-group)
- [Media](#media-template-group)
- [Icon](#icon-template-group)
- [Base](#base-template-group)

<!-- /summary -->

### A Note on Default Template Options

When no `options` are specified and no template `group` has been selected, the `options` are implicitly set as follows:

```javascript
    options: {
        price: true,
        brand: true,
        priceAndBrand: true,
        rating: true,
        ratingText: true,
        moreAt: true,
        content: false
    }
```

------

## Important Notes

1. Before using these templates in your code, please familiarize yourself with the [Spice Displaying](https://duck.co/duckduckhack/spice_displaying) document to understand the **proper usage of both the `templates` block and the `options` block**.

2. For your desired template features to display correctly, each item's data must contain the corresponding properties. Generally these are set with the aid of a [`normalize` function](https://duck.co/duckduckhack/spice_displaying#codenormalizecode-function-optional), if they do not already exist in your [`data` object](https://duck.co/duckduckhack/spice_displaying#codedatacode-object-required).

Understanding these is crucial to implementing Spice templates properly and effectively.

------

## Text Template Group

This template group is best used for results which are mostly text, where any images are icons or small logos. This template offers a title, description and footer.

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'text'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'text'
// does this for you!
templates: {
    item: 'text_item',
    detail: 'text_detail'
}
```

#### Default templates used in the `text` group:

- [`text_item`](https://duck.co/duckduckhack/spice_templates_reference#codetextitemcode-template)
- [`text_detail`](https://duck.co/duckduckhack/spice_templates_reference#codetextdetailcode-template)

See the **[important notes](#important-notes)** for making this template display correctly.

### Example uses of the `text` template group

- ["github duckduckgo"](https://duckduckgo.com/?q=github+duckduckgo) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/github/github.js))
    ![DuckDuckGo search for "github duckduckgo"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fgithub_duckduckgo.png&f=1)

- ["what rhymes with awesome"](https://duckduckgo.com/?q=what+rhymes+with+awesome) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/rhymes/rhymes.js))
    ![DuckDuckGo search for "what rhymes with awesome"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fwhat_rhymes_with_awesome.png&f=1)

- ["reddit duckduckgo"](https://duckduckgo.com/?q=reddit+duckduckgo) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/reddit_search/reddit_search.js))
    ![DuckDuckGo search for "reddit duckduckgo"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Freddit_duckduckgo.png&f=1)


------

## Info Template Group

The `info` template group is designed for Instant Answers that feature in-depth information about one item. This includes an image, title, and a description or arbitrary content. It also provides an auxiliary section to display further detail in table or list format (to the right).

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'info'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'info'
// does this for you!
templates: {
    item: 'basic_image_item',
    detail: 'basic_info_detail',
    options: {
        moreAt: true,
        aux: false
    }
}
```

#### Default templates used in the `info` group:

- [`basic_image_item`](https://duck.co/duckduckhack/spice_templates_reference#codebasicimageitemcode-template)
- [`basic_info_detail`](https://duck.co/duckduckhack/spice_templates_reference#codebasicinfodetailcode-template)

See the **[important notes](#important-notes)** for making this template display correctly.

### Example uses of the `info` template group

- ["green day band"](https://duckduckgo.com/?q=green+day+band) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/lastfm/artist/lastfm_artist.js))
    ![DuckDuckGo search for "green day band"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fgreen_day_band.png&f=1)

- ["bitcoin price"](https://duckduckgo.com/?q=bitcoin+price) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/bitcoin/bitcoin.js))
    ![DuckDuckGo search for "bitcoin price"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fbitcoin_price.png&f=1)

- ["gravatar matt"](https://duckduckgo.com/?q=gravatar+matt) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/gravatar/gravatar.js))
    ![DuckDuckGo search for "gravatar matt"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fgravatar_matt.png&f=1)


------

## Products Template Group

Best used to showcase products with an image, rating, review, brand and/or price. An optional `buy` sub-template can be used to provide a compelling call-to-action (i.e. button).

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'products'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'products'
// does this for you!
templates: {
	item: 'products_item',
	detail: 'products_detail',
	item_detail: 'products_item_detail',
	wrap_detail: 'base_detail',
	options: {
	    rating: true,
	    price: true,
	    brand: true
	}
}
```

#### Default templates used in the `products` group:

- [`products_item`](https://duck.co/duckduckhack/spice_templates_reference#codeproductsitemcode-template)
- [`products_detail`](https://duck.co/duckduckhack/spice_templates_reference#codeproductsdetailcode-template)
- [`products_item_detail`](https://duck.co/duckduckhack/spice_templates_reference#codeproductsitemdetailcode-template)
- [`base_detail`](https://duck.co/duckduckhack/spice_templates_reference#codebasedetailcode-template)

See the **[important notes](#important-notes)** for making this template display correctly.

### Example uses of the `products` template group

- ["buy batman lego"](https://duckduckgo.com/?q=buy+batman+lego) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/amazon/amazon.js))
    ![DuckDuckGo search for "buy batman lego"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fbuy_batman_lego.png&f=1)

- ["flight tracking apps"](https://duckduckgo.com/?q=flight+tracking+apps) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/quixey/quixey.js))
    ![DuckDuckGo search for "flight tracking apps"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fflight_tracking_apps.png&f=1)

- ["octopart 1770019-2"](https://duckduckgo.com/?q=octopart%201770019-2) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/octopart/octopart.js))
    ![DuckDuckGo search for "octopart 1770019-2"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Foctopart_1770019-2.png&f=1)


------

## Media Template Group

The `media` template group works well when an image is a significant part of the display of an item. Items can also have a title and a rating. 

The `item` template is essentially a simplified version of the Products template group. It also uses the same `detail` template as the Products template group.

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'media'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'media'
// does this for you!
templates: {
    item: 'basic_image_item',
    detail: 'products_detail',
    item_detail: 'products_item_detail',
    wrap_detail: 'base_detail',
    options: {
	    price: false,
	    brand: false,
	    rating: false,
	    ratingText: true
    }
}
```

#### Default templates used in the `media` group:

- [`basic_image_item`](https://duck.co/duckduckhack/spice_templates_reference#codebasicimageitemcode-template)
- [`products_detail`](https://duck.co/duckduckhack/spice_templates_reference#codeproductsdetailcode-template)
- [`products_item_detail`](https://duck.co/duckduckhack/spice_templates_reference#codeproductsitemdetailcode-template)
- [`base_detail`](https://duck.co/duckduckhack/spice_templates_reference#codebasedetailcode-template)

See the **[important notes](#important-notes)** for making this template display correctly.

### Example uses of the `media` template group

- ["lord of the rings movie"](https://duckduckgo.com/?q=lord+of+the+rings+movie) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/movie/movie.js))
    ![DuckDuckGo search for "lord of the rings movie"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Flord_of_the_rings_movie.png&f=1)

- ["movies"](https://duckduckgo.com/?q=movies) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/in_theaters/in_theaters.js))
    ![DuckDuckGo search for "movies"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fmovies.png&f=1)

- ["BBC schedule"](https://duckduckgo.com/?q=BBC+schedule) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/bbc/bbc.js))
    ![DuckDuckGo search for "BBC schedule"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fbbc_schedule.png&f=1)


------

## Icon Template Group

This template is similar to the **text** group, since it uses the same `item` template. However, the detail view is better suited to displaying an icon image.

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'icon'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'icon'
// does this for you!
templates: {
    item: 'text_item',
    detail: 'basic_icon_detail',
    item_detail: 'products_item_detail'
}
```

#### Default templates used in the `icon` group:

- [`text_item`](https://duck.co/duckduckhack/spice_templates_reference#codetextitemcode-template)
- [`products_detail`](https://duck.co/duckduckhack/spice_templates_reference#codeproductsdetailcode-template)
- [`products_item_detail`](https://duck.co/duckduckhack/spice_templates_reference#codeproductsitemdetailcode-template)

See the **[important notes](#important-notes)** for making this template display correctly.

### Example uses of the `icon` template group

- ["alternative to photoshop"](https://duckduckgo.com/?q=alternative+to+photoshop) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/alternative_to/alternative_to.js))
    ![DuckDuckGo search for "alternative to photoshop"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Falternative_to_photoshop.png&f=1)


------
## Places Template Group

This template group is ideal for displaying results where location is an important factor to the searcher. The Places template group makes it easy for searchers to view a map showing all results, or highlighting a particular item without leaving the search page.

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'places'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'places'
// does this for you!
templates: {
    item: 'places_item',
    detail: 'places_detail'
}
```

#### Default templates used in the `places` group:

- [`places_item`](https://duck.co/duckduckhack/spice_templates_reference#codeplacesitemcode-template)
- [`places_detail`](https://duck.co/duckduckhack/spice_templates_reference#codeplacesdetailcode-template)

#### Places Model and View

The Places template group works together with the Places **model** and Places **view**. The Places model and view enable special map functionality and behaviors that make instant answers using Places valuable and delightful.

The model and view are specified alongside the template group property when you call `Spice.add()`. You can see how this is done under the [Model and View section](https://duck.co/duckduckhack/spice_displaying#views) of the [Spice Displaying](https://duck.co/duckduckhack/spice_displaying) page.

To work correctly, the places model requires **additional values** passed that do not appear directly in the templates. Make sure that each item includes the attributes required by the places model. Generally these are set by your `normalize` function if they do not already exist in your `api_result`.

The available attributes for the Places Model are:

- `id` *string* 
	Unique identifier for the location
- `name` *string*
	Name of the location
- `address` *string*
	Display address of the location
- `city` *string*
- `neighborhood` *string*
	If neighborhood and city are both passed in, it will use neighborhood for the tile and fall back to the city when it's not there
- `image` *string* 
	Path to image thumbnail to be used for the location, will use default marker image if none is provided
- `polygonPoints` *array*
	If the location represents a region, array of lat/lon coordinates that create the shaded outline in queries like 'Paris map'
- `lat` *number* 
	Latitude of the location
- `lon` *number* 
	Longitude of the location
- `rating` *number* 
	Number from [0-5], supports half's, i.e. `3.5`
- `ratingImageURL` *string*
	Optional, use custom rating image URL (i.e. Yelp)
- `reviews` *number* 
	Number of reviews
- `price` *number* 
	Integer from [0-4], will be converted to up to four `$` symbols, for example `$$$$`
- `hours` *object* 
	Hash where three-char days are the keys and the values are a string of hours for that day, i.e.: `{ 'Mon': '8am - 5pm', 'Thu': '1pm - 5pm' }`
- `phone` *string*

Below are examples of the objects passed to the `data` property in a call to [`Spice.add()`](https://duck.co/duckduckhack/spice_templates_reference). These might be directly found in your `api_result` or created by defining a [`normalize`](https://duck.co/duckduckhack/spice_displaying#codenormalizecode-function-optional) function.

```javascript
Spice.add({
    id: 'landmarks',
    name: 'Landmarks',
    model: 'Place',
    view: 'Map',
    templates: {
        group: 'places'
    },
    meta: {
        sourceName: 'Wikipedia',
        sourceUrl: 'https://wikipedia.org'
    },
    data: [
        {
        	id: 'uniqueid-1',
	        name: 'Empire State Building',
	        url: 'https://en.wikipedia.org/wiki/Empire_State_Building',
	        image: 'https://upload.wikimedia.org/...480px-Empire_State_Building_by_David_Shankbone.jpg',
	        rating: '3.5',
	        address: '350 Fifth Ave',
	        neighborhood: 'Midtown',
	        city: 'New York City',
	        price: 3,
	        lat: 40.7484324,
	        lon: -73.98566413,
	        hours: {
	            Thu: '8am - 5pm'
	        }
	    }
    ]
});
```

Example of rendering multiple locations as tiles with an expandable map:

```javascript
Spice.add({
    id: 'landmarks',
    name: 'Landmarks',
    model: 'Place',
    view: 'Places',
    templates: {
        group: 'places'
    },
    meta: {
        type: 'Landmarks',
        sourceName: 'Wikipedia',
        sourceUrl: 'https://wikipedia.org'
    },
    data: [
        	{
	        	id: 'uniqueid-1',
		        name: 'Empire State Building',
		        url: 'https://en.wikipedia.org/wiki/Empire_State_Building',
		        image: 'https://upload.wikimedia.org/...480px-Empire_State_Building_by_David_Shankbone.jpg',
		        rating: '3.5',
		        address: '350 Fifth Ave',
		        neighborhood: 'Midtown',
		        city: 'New York City',
		        price: 3,
		        lat: 40.7484324,
		        lon: -73.98566413,
		        hours: {
		            Thu: '8am - 5pm'
		        }
            }, 
            {
	        	id: 'uniqueid-2',
		        name: 'Central Park',
		        url: 'https://en.wikipedia.org/wiki/Central_Park',
		        image: 'https://upload.wikimedia.org/...240px-Southwest_corner_of_Central_Park%2C_looking_east%2C_NYC.jpg',
		        rating: 5,
		        address: '86th Street, Traverse Road',
		        neighborhood: 'Midtown',
		        city: 'New York City',
		        lat: "40.78257765",
		        lon: "-73.9654435614027",
		        phone: '867-5309',
		        reviews: 327,
		        polygonPoints: [
		            ["-73.9812971", "40.7685782"],
		            ["-73.9812422", "40.7686381"],
		            ["-73.9808211", "40.7692536"],
		            ["-73.9582984", "40.8002063"],
		            ["-73.958121", "40.8002149"],
		            ["-73.9580084", "40.8002271"],
		            ["-73.9578827", "40.8002631"],
		            ["-73.957767", "40.8003367"],
		            ["-73.9577187", "40.8003814"],
		            ["-73.9497096", "40.7969917"],
		            ["-73.9497163", "40.7969425"],
		            ["-73.9497167", "40.7968837"],
		            ["-73.9497187", "40.796838"],
		            ["-73.9497076", "40.7967616"],
		            ["-73.9496758", "40.7966888"],
		            ["-73.9496293", "40.7966412"],
		            ["-73.9495571", "40.79659"],
		            ["-73.9724949", "40.7650918"],
		            ["-73.9732368", "40.7653987"],
		            ["-73.9737105", "40.7647739"],
		            ["-73.9793669", "40.767163"],
		            ["-73.9810178", "40.7678443"],
		            ["-73.9809825", "40.7678988"],
		            ["-73.9809709", "40.7679534"],
		            ["-73.9809572", "40.7680187"],
		            ["-73.9808933", "40.7680086"],
		            ["-73.9808902", "40.7680668"],
		            ["-73.980888", "40.7681073"],
		            ["-73.9809093", "40.7682121"],
		            ["-73.9808827", "40.7682181"],
		            ["-73.980908", "40.7682967"],
		            ["-73.9809213", "40.7682936"],
		            ["-73.9809652", "40.7684004"],
		            ["-73.9810569", "40.7683631"],
		            ["-73.9810729", "40.7683834"],
		            ["-73.9811234", "40.7684477"],
		            ["-73.9811992", "40.7685112"],
		            ["-73.9812971", "40.7685782"]
		        ]
	        }
        ]
});
```

### Example use of the 'places' template group

- ["parking panda"](https://duckduckgo.com/?q=parking+in+philadelphia) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/parking/parking.js))
    ![DuckDuckGo search for "parking in philadelphia"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fparking_panda.png&f=1)

------

## List Template Group

The List template group was designed for displaying in-depth attributes of one item. These attributes can be displayed as either a bulleted list of properties, *or* a table of key-value pairs. 

Multiple items are displayed using the same `text_item` template used by Text and Icon template groups. As a result, this template group is mainly useful for instant answers designed to **return a single item** with detailed properties.

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'list'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'list'
// does this for you!
templates: {
    item: 'text_item',
    detail: 'list_detail'
}
```

#### Default templates used in the `list` group:

- [`text_item`](https://duck.co/duckduckhack/spice_templates_reference#codetextitemcode-template)
- [`list_detail`](https://duck.co/duckduckhack/spice_templates_reference#codelistdetailcode-template)

See the **[important notes](#important-notes)** for making this template display correctly.

### Example use of the `list` template group

- ["whois"](https://duckduckgo.com/?q=whois+mozilla.org) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/whois/whois.js))
	![DuckDuckGo search for "whois mozilla.org"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fwhois_results.png&f=1)

------

## Base Template Group

This is the most rudimentary template group. It provides a minimal container template that accepts totally custom markup.

Using this template group requires a *large amount of work* up front, and *difficult maintenance* over time. As a result, **this template should be a last resort.**

If you feel that other template groups do not meet your needs, please **contact us at [open@duckduckgo.com](mailto:open@duckduckgo.com) *before* using the Base Template Group**. We'll work with you to find the best way to express your idea and avoid ongoing, manual maintenance for your Instant Answer.

### Usage

Using this template requires that that you set the `group` property of the `templates` block like so:

```javascript
templates: {
    group: 'base'
}
```

<!-- /summary -->

When you specify this template group, it is equivalent to setting the properties of the `templates` block as follows:

```javascript
// setting the template group to: 'base'
// does this for you!
templates: {
    item: 'base_item',
	detail: 'base_detail',
	options: {
	    price: false,
	    brand: false,
	    rating: false,
	    ratingText: false,
	    rowHighlight: false,
	    keySpacing: false,
	    moreAt: false
	}
}
```

#### Default templates used in the `base` group:

- [`base_item`](https://duck.co/duckduckhack/spice_templates_reference#codebaseitemcode-template)
- [`base_detail`](https://duck.co/duckduckhack/spice_templates_reference#codebasedetailcode-template)

See the **[important notes](#important-notes)** for making this template display correctly.

### Example uses of the `base` template group

- ["gandhi quote"](https://duckduckgo.com/?q=gandhi+quote) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/brainy_quote/brainy_quote.js))
    ![DuckDuckGo search for "gandhi quote"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fgandhi_quote.png&f=1)

- ["cpan App::cpanminus"](https://duckduckgo.com/?q=cpan+App::cpanminus) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/meta_cpan/meta_cpan.js))
    ![DuckDuckGo search for "cpan App::cpanminus"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fcpan_app_cpanminus.png&f=1)

- ["define indelible"](https://duckduckgo.com/?q=define+indelible) ([code](https://github.com/duckduckgo/zeroclickinfo-spice/blob/master/share/spice/dictionary/definition/dictionary_definition.js))
    ![DuckDuckGo search for "define indelible"](https://images.duckduckgo.com/iu/?u=https%3A%2F%2Fraw.githubusercontent.com%2Fduckduckgo%2Fduckduckgo-documentation%2Fmaster%2Fduckduckhack%2Fassets%2Ftemplate_groups%2Fdefine_indelible.png&f=1)
