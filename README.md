# Spectre

[!de](README.de.md)

![Header Graphic](/assets/readme-header.png)

A specter is haunting Europe. This is its theme. 👻 A [Ghost](https://github.com/TryGhost/Ghost) theme for politicians and local chapters of the German Left Party.

# Demo

- [spectre.hutt.io](https://spectre.hutt.io)  
- [Ines Schwerdtner](https://inesschwerdtner.de)  
- [BAG Betrieb und Gewerkschaft](https://betriebundgewerkschaft.de)

# Mockup & Screenshots

![Theme Mockup](/assets/mockup-inesschwerdtner.de.png)

| [Live Demo](https://spectre.hutt.io/) | [Download](https://github.com/hutt/spectre/releases/) |
| --- | --- |

# First Time Using a Ghost Theme?

Ghost uses a simple templating language called [Handlebars](http://handlebarsjs.com/) for its themes.

**The main files are:**

- `default.hbs` – The parent template containing the global header/footer  
- `home.hbs` – The homepage  
- `index.hbs` – The primary template for generating a list of posts  
- `post.hbs` – The template for displaying individual posts  
- `page.hbs` – Used for standalone pages  
- `tag.hbs` – Used for tag archives, e.g., “all posts tagged `news`”  
- `author.hbs` – Used for author archives, e.g., “all posts by Jamie”

You can also create custom templates by adding the page slug to the template filename. For example:

- `page-about.hbs` – Custom template for an `/about/` page  
- `tag-news.hbs` – Custom template for the `/tag/news/` archive  
- `author-ines.hbs` – Custom template for the `/author/ines/` archive

# Routes

This theme supports a static homepage as well as a blog index page. To set a static homepage at `/`, you need to adjust the [routes.yaml file](routes.yaml).

For example (the slug of the homepage is `start`, so we use `data: page.start`):

```
routes:
  /:
    template: page
    data: page.start
  /sitemap/:
    template: sitemap
    content_type: text/html

collections:
  /presse/mitteilungen/:
    permalink: /presse/mitteilungen/{year}/{slug}/
    template: search-pressemitteilungen
    filter: 'tag:hash-pressemitteilung'
  /termine/archiv/:
    permalink: /termine/{year}/{slug}/
    template: search-termine
    filter: 'tag:hash-termin'
  /blog/:
    permalink: /blog/{slug}/
    template: home

taxonomies:
  tag: /tag/{slug}/
  author: /autor_in/{slug}/
```

⚠️ **Collections must be listed in the correct order.** Posts assigned by a filter in an earlier collection (e.g., those tagged `#termin`) cannot appear in subsequent collections (e.g., `/blog/`), so `/blog/` should not include a filter to exclude press releases or events.

Example: If the `/blog/` collection is defined before `/termine/archiv/` in the routes file, the events collection will be empty because those posts have already been assigned to `/blog/`.

Learn more about collections in the [official Ghost documentation](https://ghost.org/tutorials/content-collections/).

# Events and Press Releases

As shown in the `routes.yaml` example above, this theme also supports custom pages for events and press releases so they don’t appear alongside regular posts in the blog index.

## Events

### Add the Events Collection to routes.yaml

Add the following lines under the `collections:` block in `routes.yaml`:

```
collections:
  /termine/archiv/:
    permalink: /termine/{year}/{slug}/
    template: search-termine
    filter: 'tag:hash-termin'
```

### Create an Events Page

Create a page in the editor, name it (e.g., “Termine” with slug `termine`), and fill in the content. Then select the `Termine` template in the page settings sidebar. The page will now display the five most recent events below its main content.

### Add a New Event

To add an event, create a new post and include the event details. It’s recommended to put the event date in the post title (e.g., `31.12.2024: Silvesterfeier`). Tag the post with `#termin` to ensure it appears only on the Events page (and the Events archive at `/termine/archiv`), not in the main blog index.

## Press Releases

### Add the Press Releases Collection to routes.yaml

Add the following lines under the `collections:` block in `routes.yaml`:

```
collections:
  /presse/mitteilungen/:
    permalink: /presse/mitteilungen/{year}/{slug}/
    template: search-pressemitteilungen
    filter: 'tag:hash-pressemitteilung'
```

### Create a Press Releases Page

Create a page, name it (e.g., “Presse” with slug `presse`), and add content such as press contacts or photos. Then select the `Presse` template in the page settings sidebar. The page will now display the five most recent press releases below its main content.

### Add a New Press Release

To add a press release, create a new post, tag it with `#pressemitteilung`, and it will appear only on the Press Releases page (and `/presse/mitteilungen` archive), not in the main blog index.

# Privacy-Friendly YouTube Embeds

This theme enables GDPR-compliant YouTube embeds using [light-yt.js](https://www.labnol.org/internet/light-youtube-embeds/27941/) by [Amit Agarwal](https://github.com/labnol).

## Embedding a YouTube Video

1. Copy the YouTube video ID (from `https://www.youtube.com/watch?v=dQw4w9WgXcQ`, the ID is `dQw4w9WgXcQ`).  
2. In the Ghost editor, add an HTML block where the video should appear.  
3. Insert the following code, replacing `VideoID` with your copied ID:

   ```
   <div class="youtube-player" data-id="VideoID"></div>
   ```

   (Example: `<div class="youtube-player" data-id="dQw4w9WgXcQ"></div>`)

Save this block as a snippet in Ghost so you don’t have to retype it each time.

# Google News–Compatible Sitemap

This theme can generate [Google News–compatible sitemaps](https://developers.google.com/search/docs/crawling-indexing/sitemaps/news-sitemap). Just define a route with the sitemap template as shown:

```
routes:
  /sitemap/:
    template: sitemap
    content_type: text/html
```

Then submit the sitemap URL (`https://your-site.com/sitemap/`) in Google Search Console.

# Automatic Logo Generation

For faster load times, upload your logo in SVG format. If you don’t, Spectre will generate a logo from your site title.

# Development

Spectre themes are compiled with Gulp/PostCSS. You need [Node](https://nodejs.org/), [Yarn](https://yarnpkg.com/), and [Gulp](https://gulpjs.com) installed globally. Then run from the theme’s root:

```
# Install dependencies
yarn install

# Start development server
yarn dev
```

Any changes in `/assets/css/` will automatically compile into `/assets/built/`. To package the theme:

```
# Create zip archive
yarn zip
```

# PostCSS Features

- [Autoprefixer](https://github.com/postcss/autoprefixer) – Automatically handles vendor prefixes for CSS features.

# Performance Optimizations

Spectre uses several techniques to speed up page loads: preloading key assets, bundling and minifying CSS/JS, critical inline CSS, and embedding icons as SVGs instead of icon fonts.

## Critical Inline CSS

To improve the time to first contentful paint, Spectre uses [penthouse](https://github.com/pocketjoso/penthouse) to generate critical CSS for post, page, tag, and index templates. These styles are included via Handlebars partials (`default.hbs` and `partials/components/inline-css.hbs`). Run `yarn critical` to regenerate.

## SVG Icons

Spectre includes inline SVG icons in `/partials/icons`. Use an icon with:

```
{{> "icons/rss"}}
```

Add new icons by placing them in `/partials/icons`.

# Avoiding All Third-Party Requests

By default, Ghost and light-yt.js make requests to external services. While usually privacy-safe, you can avoid them entirely.

## Using a Caching Proxy

Adjust this `docker-compose.yml` to use [hutt/spectre-docker-compose](https://github.com/hutt/spectre-docker-compose), then enable YouTube proxying with:

```
<script>
  const YT_DATA_URL_PREFIX = "/proxy/youtube/data";
  const YT_THUMBNAIL_URL_PREFIX = "/proxy/youtube/thumbnail";
</script>
```

## Using a Cloudflare Worker

### JSDelivr for Ghost

Override portal, search, and comments URLs in `config.[env].json` or deploy a Cloudflare Worker to proxy and cache JSDelivr requests under `/cdn-jsdelivr/*`. Update:

```
{
  "portal": {
    "url": "/cdn-jsdelivr/npm/@tryghost/portal@~{version}/umd/portal.min.js"
  },
  ...
}
```

### YouTube via Worker

light-yt.js fetches metadata from `youtube-nocookie.com` and thumbnails from `i.ytimg.com`. You can proxy these via an nginx cache or Cloudflare Worker at `/yt-proxy/*` and configure:

```
<script>
  const YT_DATA_URL_PREFIX = "/yt-proxy/data";
  const YT_THUMBNAIL_URL_PREFIX = "/yt-proxy/thumbnail";
</script>
```

# Copyright & License

Copyright (c) 2013–2023 [Ghost Foundation](https://ghost.org); 2023–2025 [Jannis Hutt](https://hutt.io).  
Based on [Source](https://github.com/TryGhost/Source) by Ghost Foundation. Licensed under the MIT License.  