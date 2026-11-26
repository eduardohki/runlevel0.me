# runlevel0.me

My personal website built with [Zola](https://www.getzola.org/) 0.23.

## Local Development

```sh
zola serve   # live preview at http://127.0.0.1:1111
zola build   # writes the site to public/
```

- `content/`: pages and blog posts in Markdown. The CV page is generated from `data/cv.yaml`.
- `templates/`: Tera templates; `partials/style.html` holds the site-wide styles.
- `static/`: files copied as-is (images, favicons, the PDF CV).

Pushes to `main` deploy to GitHub Pages via `.github/workflows/pages.yml`.
Feeds: RSS (`index.xml`) for the whole site, the blog, and each tag and category.

## License

Copyright Eduardo Hernacki.

The content under the `blog/` section of this website is licensed under the  [Creative Commons Attribution-NoDerivs 4.0 International](https://creativecommons.org/licenses/by-nd/4.0/) license, and the underlying source code used to format and display that content is licensed under the MIT license.
