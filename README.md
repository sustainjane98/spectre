# Spectre

[![en](https://img.shields.io/badge/lang-en-red.svg)](README.md)

![Header Graphic](/assets/readme-header.png)

A specter is roaming Europe. This is its theme. 👻 A [Ghost](https://github.com/TryGhost/Ghost) theme for politicians and local chapters of the German party Die Linke.

# Demo

- [spectre.hutt.io](https://spectre.hutt.io)  
- [Ines Schwerdtner](https://inesschwerdtner.de)  
- [BAG Betrieb und Gewerkschaft](https://betriebundgewerkschaft.de)  

# Mockup & Screenshots

![Theme Mockup](/assets/mockup-inesschwerdtner.de.png)

| [Live Demo](https://spectre.hutt.io/) | [Download](https://github.com/hutt/spectre/releases/) |
|---|---|

# First Time Using a Ghost Theme?

Ghost uses a simple templating language called [Handlebars](http://handlebarsjs.com/) for its themes.

**The main files are:**

- `default.hbs` – the base template that contains the global header and footer  
- `home.hbs` – the homepage  
- `index.hbs` – the main template for listing posts  
- `post.hbs` – the template for displaying individual posts  
- `page.hbs` – used for standalone pages  
- `tag.hbs` – used for tag archives, e.g. all posts with the tag `news`  
- `author.hbs` – used for author archives, e.g. all posts by Jamie  

You can also create custom templates by adding a page slug to the template filename. For example:

- `page-about.hbs` – custom template for an `/about/` page  
- `tag-news.hbs` – custom template for the `/tag/news/` archive  
- `author-ines.hbs` – custom template for the `/author/ines/` archive  

# Routes

This theme supports a static homepage as well as a blog index. To set a static homepage at `/`, you must customize the [routes.yaml](routes.yaml) file.

For example (the slug for the homepage in this example is `start`, hence `data: page.start`):

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
  author: /author/{slug}/
```

**Note:** Collections must be listed in the correct order. Content that matches an earlier collection’s filter (e.g. posts tagged `#termin` in the routes file) won’t appear in later collections (e.g. `/blog/`). If `/blog/` were defined before `/termine/archiv/`, the Termine collection would be empty because those posts would already belong to `/blog/`.

More about collections in the [official documentation](https://ghost.org/tutorials/content-collections/).

# Events and Press Releases

As shown above, this theme also supports custom pages for events and press releases, so these post types don’t appear alongside blog articles in the main index.

## Events

### Add an Events Collection to routes.yaml

Add the following under `collections:` in `routes.yaml`:

```
collections:
  /termine/archiv/:
    permalink: /termine/{year}/{slug}/
    template: search-termine
    filter: 'tag:hash-termin'
```

### Create an Events Page

In Ghost Admin, create a new page titled “Termine” (slug `termine`), add your content, then select the `Termine` template in the page settings sidebar. The page will display the five most recent events below the main content.

### Add a New Event

Create a new post, include event details in the title (e.g. `31.12.2024: New Year’s Eve Celebration`), and tag it with `#termin`. This ensures it appears only on the Events page and archive (`/termine/archiv`), not in the main blog index.

## Press Releases

### Add a Press Releases Collection to routes.yaml

Under `collections:` in `routes.yaml`:

```
collections:
  /presse/mitteilungen/:
    permalink: /presse/mitteilungen/{year}/{slug}/
    template: search-pressemitteilungen
    filter: 'tag:hash-pressemitteilung'
```

### Create a Press Releases Page

Create a page titled “Presse” (slug `presse`), add contact info or press photos, then choose the `Presse` template in the page settings. It will list the five latest press releases below the page content.

### Add a New Press Release

Create a new post, tag it with `#pressemitteilung`, and it will appear only on the Press page and archive (`/presse/mitteilungen`), not in the main blog.

# Privacy-Friendly YouTube Embeds

This theme enables GDPR-compliant YouTube embeds using [light-yt.js](https://www.labnol.org/internet/light-youtube-embeds/27941/) by Amit Agarwal.

## Embed a YouTube Video

1. Copy the YouTube video ID (e.g. for `https://www.youtube.com/watch?v=dQw4w9WgXcQ` the ID is `dQw4w9WgXcQ`).  
2. In the editor, add an HTML block where the video should appear.  
3. Insert:

   ```
   <div class="youtube-player" data-id="VideoID"></div>
   ```

   replacing `VideoID` with your copied ID.

Save this block as a snippet to reuse it easily.

# Google News Sitemap

This theme can generate [Google News–compatible sitemaps](https://developers.google.com/search/docs/crawling-indexing/sitemaps/news-sitemap). Add a route:

```
routes:
  /sitemap/:
    template: sitemap
    content_type: text/html
```

Then submit `https://your-site.com/sitemap/` in Google Search Console.

# Automatic Logo Generation

For best performance, upload your logo as an SVG. If none is provided, Spectre generates a text-based logo from your site title.

# Development

Spectre is built with Gulp/PostCSS. You need [Node](https://nodejs.org/), [Yarn](https://yarnpkg.com/), and [Gulp](https://gulpjs.com/) installed globally. From the theme root:

```
# Install dependencies
yarn install

# Start dev server
yarn dev
```

Changes to `/assets/css/` automatically compile into `/assets/built/`. To package the theme:

```
# Create zip archive
yarn zip
```

# PostCSS Features

- [Autoprefixer](https://github.com/postcss/autoprefixer) – automatically adds vendor prefixes for CSS features.

# Performance Optimizations

Spectre uses resource preloading, concatenated and minified CSS/JS, critical inline CSS, and inline SVG icons instead of icon fonts.

## Critical Inline CSS

To speed up [First Contentful Paint](https://web.dev/articles/fcp), Spectre uses [penthouse](https://github.com/pocketjoso/penthouse) to generate critical CSS for post, page, tag, and index templates, injected via Handlebars partials (`default.hbs` and `partials/components/inline-css.hbs`). Run `yarn critical` to regenerate.

## SVG Icons

All icons live in `/partials/icons`. Include an icon with:

```
{{> "icons/rss"}}
```

Additional SVGs can be added to the folder.

# Eliminating Third-Party Requests

By default, both [Ghost](https://github.com/TryGhost/Ghost/blob/2f09dd888024f143d28a0d81bede1b53a6db9557/PRIVACY.md) and light-yt.js make third-party requests. While they’re generally safe, you can avoid them.

## JSDelivr Requests (Ghost)

Ghost loads Portal, Search, and Comments scripts from JSDelivr. You can override these URLs in `config.[env].json` (see [docs](https://ghost.org/docs/config/#privacy)).

> [!TIP]
> Eliminate third-party requests with a simple nginx caching proxy: [hutt/spectre-docker-compose](https://github.com/hutt/spectre-docker-compose)

Alternatively, deploy a Cloudflare Worker to proxy and cache JSDelivr under your domain. Update your config to use `/cdn-jsdelivr/` instead of `/proxy/`.

## YouTube Requests (light-yt.js)

light-yt.js makes two requests: one to `youtube-nocookie.com` for JSON data and another to `i.ytimg.com` for the thumbnail. Proxy these via Nginx or a Cloudflare Worker and set:

```
<script>
  const YT_DATA_URL_PREFIX = "/proxy/youtube/data";
  const YT_THUMBNAIL_URL_PREFIX = "/proxy/youtube/thumbnail";
</script>
```

# Copyright & License

Copyright (c) 2013–2023 [Ghost Foundation](https://ghost.org);  
2023–2024 [Jannis Hutt](https://hutt.io).  
Based on [Source](https://github.com/TryGhost/Source) by the Ghost Foundation. Released under the [MIT License](LICENSE).