# Moonshake



A no-nonsense static blog builder for people who like to write and/or tinker.

Named after [the Can song](https://open.spotify.com/track/6ACXIPu0jhLDriV5sjrWtb).

<a href="https://scottfromny.com/blog" target="_blank" rel="noopener"><img width="1155" height="825" alt="Screenshot 2026-04-24 at 4 41 09 PM" src="https://github.com/user-attachments/assets/d5438f2d-7b8f-4c7f-94cd-a021bb380477" /></a>

**[See it live at scottfromny.com/blog](https://www.scottfromny.com/blog/)**

---

## Why Moonshake

If you work in marketing or communications, you've probably lost hours of your life to WordPress plugins, themes, Squarespace limitations, or some agency's proprietary CMS that requires a support ticket to change a comma. (I am currently experiencing this, and this project was made partially out of such frustrations.)

Moonshake is what I built instead: a single Python script that turns a folder of Markdown files into a complete, deployable blog. No dashboard, no database, no monthly fees. You write in whatever editor you like (I use [Obsidian](https://obsidian.md/)), run one command, and get a static site ready to upload.

Hosting is free. Because it's just HTML files, you can deploy to [Cloudflare Pages](https://pages.cloudflare.com/), Netlify, GitHub Pages, or any static host. I use Cloudflare Pages and pay nothing. Big ups to Cloudflare Pages!

---

## What it does

- Converts Markdown posts to clean, SEO-ready HTML. (Also AEO/GEO, if that's your steez.)
- Handles images automatically — drop them anywhere in your posts folder, Moonshake finds, copies, and rewrites the paths
- Builds a paginated blog index, RSS feed, sitemap.xml, and llms.txt
- Generates full Open Graph, Twitter Card, and Schema.org structured data on every post
- Optional newsletter signup form (via [Buttondown](https://buttondown.com/))
- Configurable navigation links
- `moonshake init` scaffolds a complete new project in seconds, including a starter stylesheet
- `--dry` mode to preview every change before writing a single file
- Auto-generates a timestamped zip of your site after every build, ready to upload
- Draft support — set `draft: true` in any post to hide it from builds
- Works great with Obsidian (including `![[wiki image]]` syntax) but only requires Markdown files — use any editor you like
- Honestly, it just lets you focus on writing instead of worrying about bits and bobs. But if you wanna worry about bits and bobs, there's plenty to worry about.

  <figure>
    <img width="1027" height="745" alt="image" src="https://github.com/user-attachments/assets/c2fdffcd-6106-45be-b10d-ca25dab5296f" />
    <figcaption><i>Above: What a Moonshake post looks like when written in Obsidian</i></figcaption>
  </figure>


---

## Requirements

- Python 3.11+ (or Python 3.9–3.10 with `pip install tomli`)
- `pip install pyyaml markdown`

---

## Installation

Download the script and make it executable:

```bash
curl -o moonshake https://raw.githubusercontent.com/scottsteinhardt/moonshake/main/moonshake
chmod +x moonshake
mv moonshake /usr/local/bin/
```

Install Python dependencies:

```bash
pip install pyyaml markdown
```

Verify it works:

```bash
moonshake --help
```

---

## Quick start

```bash
mkdir my-blog && cd my-blog
moonshake init
```

This creates:

```
my-blog/
├── moonshake.toml          ← your configuration
├── posts/
│   └── hello-world.md      ← your first sample post
└── site/
    └── blog/
        └── assets/
            └── style.css   ← starter stylesheet
```

Open `moonshake.toml` and fill in your name, site URL, and any nav links you want. Then:

```bash
moonshake --dry    # preview what would be built
moonshake          # build your site
```

Your site is now in the `site/` folder, and a ready-to-upload zip has been created in the parent directory.

---

## Writing posts

Posts are Markdown files in your `posts/` folder. Each file starts with a YAML frontmatter block:

```markdown
---
title: My Post Title
date: 2026-01-15
description: A short summary shown on the blog index and in search results.
feature_image: cover.jpg    # optional hero image
tags: [writing, ideas]      # optional
draft: true                 # hides the post from builds
---

Your post content goes here. Standard Markdown — headings, links, code blocks, tables, footnotes, all supported.
```

**Frontmatter fields:**

| Field | Required | Notes |
|-------|----------|-------|
| `title` | yes | Becomes the page `<title>` and post heading |
| `date` | no | `YYYY-MM-DD`. Uses file modification time if omitted |
| `slug` | no | URL slug. Auto-generated from filename if omitted |
| `description` | no | Meta description. First paragraph used if omitted |
| `feature_image` | no | Path to a hero image. Appears at the top of the post and as og:image |
| `tags` | no | List of tags, used in structured data |
| `draft` | no | `true` skips the post entirely |

**Images:** Drop images anywhere in your posts folder (same directory, a subfolder, anywhere). Reference them by filename:

```markdown
![A caption for the image](my-photo.jpg)
```

Moonshake finds the file, copies it to the right place in your site, and rewrites the path. You don't have to think about it.

If you use Obsidian, `![[image.png]]` wiki-image syntax works too.

---

## Configuration

Your `moonshake.toml` controls everything. Here's the full reference — see `moonshake.example.toml` for a ready-to-copy template with comments.

### `[paths]`

| Key | Description |
|-----|-------------|
| `posts` | Path to your Markdown posts directory |
| `site` | Path to the output directory for your built site |

### `[blog]`

| Key | Description |
|-----|-------------|
| `base_url` | Your site's root URL, no trailing slash |
| `title` | Your name or blog name |
| `subtitle` | Short tagline shown under the title on the index page |
| `description` | Blog index meta description |
| `rss_description` | RSS feed description (falls back to `description`) |
| `author` | Your name, used in bylines and copyright |
| `email` | Your email, used in structured data |
| `locale` | Defaults to `en_US` |
| `css_url` | URL path to your stylesheet (must exist in the site directory) |
| `default_og_image` | Fallback social sharing image, relative to `base_url` |
| `excluded_slugs` | List of slugs in `/blog/` that are not posts |

### `[author]`

Used for Schema.org structured data. All fields optional.

| Key | Description |
|-----|-------------|
| `location_city` | Your city |
| `location_region` | Your state or region |
| `location_country` | Defaults to `US` |
| `linkedin` | Full URL |
| `github` | Full URL |
| `bluesky` | Full URL |
| `twitter` | Full URL |
| `mastodon` | Full URL |

### `[newsletter]` *(optional)*

Remove this section entirely to disable the signup form. Currently supports [Buttondown](https://buttondown.com/).

| Key | Description |
|-----|-------------|
| `username` | Your Buttondown username |
| `heading` | Form heading text |
| `blurb` | Short description above the email field |

### `[[nav]]` *(repeatable)*

Add one `[[nav]]` block per link. They appear in the order listed.

| Key | Description |
|-----|-------------|
| `label` | Link text |
| `href` | URL |
| `css_class` | CSS class, defaults to `nav-link` |
| `post_only` | `true` → only shows on individual post pages |
| `index_only` | `true` → only shows on the blog index |
| `external` | `true` → adds `target="_blank" rel="noopener"` |

---

## Design

When you run `moonshake init`, a complete starter stylesheet is written to `site/blog/assets/style.css`. It's a dark-mode design using [Lora](https://fonts.google.com/specimen/Lora) (serif, for headings and post titles) and [Inter](https://fonts.google.com/specimen/Inter) (sans-serif, for body and UI).

Customize it freely — it's yours. Change the colors, swap the fonts, adjust the layout. The CSS uses custom properties (`--color-accent`, `--font-serif`, etc.) so most changes are a one-line edit at the top of the file.

---

## Deploying to Cloudflare Pages

Cloudflare Pages hosts static sites for free with no traffic limits. Every time you run `moonshake`, it generates a zip file in the parent directory of your site named something like `UPLOAD THIS AS OF 2026-04-24 2:18 PM.zip`. That's your deploy artifact.

**First time setup:**

1. Go to [Cloudflare Pages](https://pages.cloudflare.com/) and sign up for a free account
2. Click **Create a project** → **Direct Upload**
3. Name your project (this becomes your subdomain: `yourproject.pages.dev`)
4. Upload the zip file Moonshake generated
5. Click **Deploy**

Your blog is live. Cloudflare gives you a free `*.pages.dev` subdomain instantly, and you can connect a custom domain in the dashboard for free.

**Subsequent deploys:**

1. Run `moonshake` — it builds the site and generates a new zip
2. Go to your Cloudflare Pages project → **Deployments** → **Upload**
3. Drop in the new zip

That's it. The whole deploy takes about 30 seconds.

**Custom domain:**

In your Cloudflare Pages project, go to **Custom domains** and add your domain. If your domain is already on Cloudflare (DNS), it connects automatically. If not, follow Cloudflare's instructions to update your nameservers — it's a one-time step.

---

## Commands

```
moonshake init           Set up a new project in the current directory
moonshake                Build your site
moonshake --dry          Preview what would be built without writing anything
moonshake --config PATH  Use a specific config file instead of moonshake.toml
```

---

## Migrating from an existing blog

If you have existing blog posts as HTML files (migrated from WordPress, Ghost, or another system), Moonshake can incorporate them. Place the existing HTML in your site's `/blog/` directory, one folder per post (e.g., `site/blog/my-old-post/index.html`). Moonshake reads their metadata from the HTML and includes them in the index, feed, and sitemap alongside your new Markdown posts. If you write a new Markdown post with the same slug as an existing HTML post, the Markdown version wins.

---

## License

MIT
