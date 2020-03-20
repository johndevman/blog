+++
title = "Hugo and Images"
date = 2020-02-03T16:00:00+01:00
author = "John Svensson"
description = "Adding images to your posts in Hugo"
+++

{{< image "image.jpg" "Cape Town, South Africa" >}}

Photo by [Tobias Reich](https://unsplash.com/@electerious) on [Unsplash](https://unsplash.com/)

## How I did it

First I reorganized the content (folder) to using [Page Bundles](https://gohugo.io/content-management/page-bundles/).

From

```
content/
├── posts
│   ├── validating-objects-with-symfony-validator-component.md
├── _index.md
```

To

```
content/
├── posts
│   ├── validating-objects-with-symfony-validator-component
│   │   ├── index.md
│   ├── hugo-and-images
│   │   ├── index.md
│   │   ├── image.jpg
├── _index.md
```

And then I created a new shortcode.

image.html:

```
{{ $resource := .Page.Resources.GetMatch (printf "*%s*" (.Get 0)) }}

{{ $image := $resource.Resize "680x" }}
{{ $image2 := $resource.Resize "1360x" }}

<img src="{{ $image.RelPermalink }}" srcset="{{ $image.RelPermalink }} 1x, {{ $image2.RelPermalink }} 2x" alt="{{ .Get 1 }}">
```

I can now use that shortcode in my post.

```
{{</* image "image.jpg" "Alternative text" */>}}
```

Next up is responsive images.
