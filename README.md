# Welcome to Darcy's Docs Repository

Here you can contribute to the Darcy Docs Site, which lives at [docs.darcy.ai](https://docs.darcy.ai).

# Overview

The docs site is generated from markdown files stored in `/content` by the [Hugo](https://gohugo.io)
site generator. The Hugo theme is a fork of [Doks](https://getdoks.org).

When changes are merged to the `master` branch, a CI [pipeline](https://github.com/darcyai/docs/actions) kicks off, which publishes the
content to [Netlify](https://www.netlify.com).


## Pre-requisites

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/) — latest LTS version or newer

> The Doks theme uses npm (included with Node.js) to centralize dependency management,
> making it [easy to update](https://getdoks.org/docs/help/how-to-update/) resources, build tooling,
> plugins, and build scripts.


## Get started

- Clone the repo: `git clone https://github.com/darcyai/docs.git`
- Change to the repo dir: `cd docs`
- Install dependencies: `npm install`
- Start a local server: `npm run start`
- View the website locally: [http://localhost:1313](http://localhost:1313)

> NOTE: when the local server is running, you can generally make edits and they will
> appear in your browser. However, for some types of changes, you may need to restart
> the local server.

## Contributing

If you find issues with the content, you can:

- Ask a question or open a discussion in [GitHub Discussions](https://github.com/darcyai/docs/discussions)
- Open a [GitHub issue](https://github.com/darcyai/docs/issues/new/choose)
- Submit a [Pull Request](https://github.com/darcyai/docs/pulls). If submitting a PR:
  - Verify that your changes build locally. Passes:
    - `npm run test`
    - `npm run build:preview` followed by `npm run lint`
  - Create a [Pull Request](https://github.com/darcyai/docs/pulls) (follow the [GitHub flow](https://guides.github.com/introduction/flow/) and [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/))
  - Supports all screen sizes (if relevant)
  - Supports both light and dark mode (if relevant)

## Development

The site is generated using Hugo, based off the Doks theme. Start with
the [Hugo docs](https://gohugo.io/getting-started/) and then the [Doks theme docs](https://getdoks.org/docs/overview/introduction/).

Content is stored in [`/content`](/content).

### Staging branch

If you merge changes to branch `staging`, those changes will show up at [https://staging--darcydocs.netlify.app](https://staging--darcydocs.netlify.app).


### Templates

The [`/layouts`](/layouts) dir contains the templates for rendering. The most interesting parts are:

- `/layouts/_default`: the core template files
- `/layouts/partials`: templates for partial (e.g. footer, header, sidebar etc)
- `/layouts/docs`: templates for the `docs` content type (i.e. most of the content)

### CSS

Styling is controlled via the SCSS files in `/assets/scss`. Note that the theme
supports both normal and "dark" mode, so be sure to also update `/assets/scss/common/_dark.scss`.

> For one-off HTML components (e.g. for [raw html](#Raw HTML)), put that CSS into `/assets/scss/custom.scss`.


## Branches

If you merge a change to `master`, this will trigger the build pipeline, and ultimately result in
your changes being published to prod (meaning [https://docs.darcy.ai](https://docs.darcy.ai)).

We've also enabled a `staging` branch. If you merge a change to the `staging` branch, this will also
trigger a pipeline, and your changes will be visible at [https://staging--darcydocs.netlify.app](https://staging--darcydocs.netlify.app).

Obviously, that `staging` branch needs to be merged to `master` for that content to take effect on the live site.

## Content

### Front matter

Content is generated from markdown files stored under [`/content`](/content).

Markdown files must have [front matter](https://gohugo.io/content-management/front-matter/), which
is basically metadata. For example:

```markdown
---
title : "About Darcy Cloud"
weight: 100
linkTitle: "About"
---

CONTENT GOES HERE
```

Every content directory must have an `_index.md` file (which is basically the equivalent
of an `index.html`). Other files should be in `lower-kebab-case.md` format.
Note that `title` is required. If `linkTitle` is also present, that value is used to generate
link values in preference to `title`.


#### Slugs & Aliases
Hugo will generate the page slug (URL path) based on the filename and the slug of
the file's parents.

For example:

```text
+ doc                               /doc
    + ai
        - _index.md                 /doc/ai
        - getting-started.md        /doc/getting-started
```

If you want to set a specific slug for a page, use the `slug` field in the front matter.

```text
---
title : "About Darcy Cloud"
weight: 100
slug: "how-to-get-started"
---
```
This results in path `/doc/how-to-get-started`.

Relatedly you can also set one or more aliases in the front matter. An alias is a full path
which will be redirected to the current content.

```markdown
---
title : "About Darcy Cloud"
weight: 100
slug: "how-to-get-started"
aliases:
    - /doc/get-started/
    - /guide/how-to-get-started/
---
```

### Ordering (weight)

The order in which pages appear is determined by the `weight` value in the page's front matter.
For a sub-directory, its order amongst its siblings is determined by the `weight` value in
the front matter of the `_index.md` in the sub-directory.

Within a directory, the `_index.md` file should have the smallest weight. The other markdown
files should have a weight greater than the weight of `_index.md`.


### Images & other assets

To reference an image from markdown, add the image to the [`/static/images`](/static/images) dir. For example,
add `/static/images/my-image.png`, then reference the image in markdown using `![My Image](/images/my-image.png)`.

You can do the same for other files, e.g. add `/static/pdfs/my.pdf` and reference in markdown
with `![My PDF](/pdfs/my.pdf)`.

> NOTE: Use descriptive file names for uploaded images. *DO NOT* upload a file
> named `screen-shot-2021-10-08-at-5.59.22-pm.png`; instead upload `portal-projects-overview.png`.

### Alerts

You can use the `alert` [shortcode](https://gohugo.io/content-management/shortcodes/) to generate
[Bootstrap-style alerts](https://getbootstrap.com/docs/4.0/components/alerts/).

```markdown
{{<alert>}}
  Here's the info you're looking for!
{{</alert>}}
```

The default alert style is `info`, but you can explicitly set a style (`success`, `info`, `warning`, `danger`).

```markdown
{{<alert style="warning">}}
  If you do not have any of the above hardware, you can create a VM and run
  the node installation script.
{{</alert>}}
```

### Links

When referring to other content (markdown files), use the [Hugo ref shortcode](https://gohugo.io/content-management/cross-references/)
instead of a traditional markdown link. For example, if you're in `/docs/cloud/a.md` and you want to refer
to `/docs/cloud/b.md`, use this:

```markdown
I am talking about [the b thing]({{<ref "b.md">}}).
```

Yes, it's more long-winded than the traditional markdown (`[the b thing](b.md)`), but the shortcode
mechanism causes Hugo to verify the link, and Hugo will fail if it cannot resolve the referenced document.
This is a significant benefit.

If you are linking outside the current dir:
- If you're linking to a file below the current dir, typically use a relative path, e.g. `[the below thing]({{<ref "./c/d.md">}})`.
- If you're linking to a file above the current dir, typically use absolute an absolute path, e.g. `[the above thing]({{<ref "/docs/cloud/e/f.md">}})`

### Raw HTML

From time to time it may be pragmatic to include raw HTML in the markdown. For example, if
there's a one-off HTML structure on a landing page. To do so, use the `rawhtml` shortcode:

```text
# Normal markdown here...

{{< rawhtml >}}
<div class="hello">
  <p>hello</p>
</div>
{{< /rawhtml >}}

## Normal markdown continues here...
```

## Linting

We have several lint targets, although some of them are still WIP and not fully integrated
into the pipeline for various reasons.

### Script & Markdown Linting

This runs the linter against scripts and the markdown files. It does not check links. Example
below. All lint errors must be fixed.

```text
$ npm run lint

> @hyas/doks@0.4.2 lint
> npm run -s lint:scripts && npm run -s lint:styles && npm run -s lint:markdown

markdownlint-cli2 v0.4.0 (markdownlint v0.25.1)
Finding: *.md content/**/*.md !node_modules !CHANGELOG.md !README.md
Linting: 54 file(s)
Summary: 87 error(s)
content/en/docs/ai/build.md:99 MD040/fenced-code-language Fenced code blocks should have a language specified [Context: "```"]
content/en/docs/ai/build.md:119 MD040/fenced-code-language Fenced code blocks should have a language specified [Context: "```"]
```

> See GitHub [RULES.md](https://github.com/markdownlint/markdownlint/blob/master/docs/RULES.md) for markdown linting rules.

### Link Checking

There's a separate linter for checking links. At the moment it's a two-step process:

- Start the local server (`npm run start`). You should verify that the server is
  not reporting any errors to the terminal.
- In a separate terminal, run `npm run lint:links`.

> You may see the following error: this means that the local server is not running.
>
> ```
>    [0] http://localhost:1313/
>    ERROR: Detected 1 broken links. Scanned 1 links in 0.007 seconds.
> ```


The output can be a bit confusing. First, each of the broken links are listed. Then, more
usefully, a section is displayed for each page URL, listing the problems beneath that
page URL. Here's a snippet of the output:

```text
http://localhost:1313/docs/ai/
  [404] http://localhost:1313/docs/ai/docs
  [404] http://localhost:1313/docs/ai/docs.sh
  [0] http://localhost:8000/
  [404] http://localhost:1313/docs/ai/examples
  [404] http://localhost:1313/docs/ai/examples/audio_analysis
```

> Note that `http://localhost:1313/docs/ai/` is built from `/content/en/docs/ai/_index.md`

In the output above, we can see that there are several broken links, which need to be fixed. You
would go to `/content/en/docs/ai/_index.md` to find and fix those links. Remember to use
the [link shortcode](#Links), which looks like `[my link]({{<ref "doc.md">}}`.

If you have created new content (a new `.md` file) or have moved content around, the `npm run lint:links`
target may return an error pointing to GitHub:

```text
http://localhost:1313/docs/ai/terminology/
  [404] https://github.com/darcyai/docs/blob/master/content/en/docs/ai/terminology.md
```

This is due to the _Edit this page on GitHub_ auto-generated link. Currently that link always
points at the `master` branch, thus if your `.md` file has not been added to `master` (or has
been moved), the link checker will return an error until it has been merged. We can probably
improve the behavior of the linter or the link generator at some point, but you can generally
ignore those errors.

> Note that we provide two special npm targets for checking the deployed `staging` and `prod`
> environments:
>
> - `npm run staging:lint:links`
> - `npm run prod:lint:links`
