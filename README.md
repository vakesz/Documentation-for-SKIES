# SKIES website

Source for [skies.vakesz.com](https://skies.vakesz.com), the website for the SKIES app. It's a plain [Hugo](https://gohugo.io/) site with no theme, no JavaScript and one stylesheet.

```sh
pnpm install   # installs Hugo
pnpm dev       # http://localhost:1313
pnpm build     # outputs to public/
```

| Path | Contents |
| --- | --- |
| `content/_index.md` | Home page copy |
| `content/docs/` | Features, roadmap, known issues, privacy policy, contact, SiteConfig reference |
| `content/changelog/` | One file per release; the newest by `date` is linked from the home page |
| `layouts/` | Page templates |
| `assets/main.css` | All styles |
| `hugo.toml` | Site settings, store links, current app version and navigation |

When releasing a new version, add `content/changelog/<version>.md` and update `params.appVersion` in `hugo.toml`.
