# Developer documentation for nRF Connect for Desktop

This repository documents how to develop using the nRF Connect for Desktop framework.

Read this documentation if you want to create a new nRF Connect for Desktop app, modify an existing
app or modify the core of nRF Connect for Desktop itself.

This documentation can [also be read online](https://nordicsemi.github.io/pc-nrfconnect-docs/).

## Modifying the documentation

If you want to preview changes locally before submitting them to the repo, set up Jekyll locally.

Read and follow the [instructions on how to set up GitHub Pages site locally with Jekyll](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll). Here is a summary of steps:

1. Make sure you have a recent ruby.
2. If you have not already bundler installed, run `gem install bundler`.
3. In this repo folder, run `bundle install`.
4. Run `bundle exec jekyll serve` to serve the site.
5. Go to `http://localhost:4000` to view the site.

Each time you want to view your local files, run `bundle exec jekyll serve` again.

## Gotcha

When creating a new Markdown file, you should include the Jekyll’s “Front matter” at the beginning
of the file. The front matter is composed of two lines with three dashes:

```

    ---
    ---

```

Sometimes Jekyll does not process the Markdown files correctly without this front matter and
they are also required as per [GitHub Pages documentation](https://help.github.com/en/articles/configuring-jekyll#front-matter-is-required).

## Contributing

See the
[infos on contributing](https://nordicsemi.github.io/pc-nrfconnect-docs/contributing)
for details.
