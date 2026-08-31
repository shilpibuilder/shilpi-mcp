# Shilpi MCP server

[![Listed on mcpservers.org](https://mcpservers.org/badge.svg)](https://mcpservers.org/servers/shilpibuilder-com-docs)

Connect Claude, ChatGPT or any MCP-capable agent to a WordPress site running the
[Shilpi](https://shilpibuilder.com/) theme, and let it build and edit that site
through scoped abilities rather than server access.

This repository holds the public interface: the registry manifest, the tool
list, the authentication guide and example client configurations. The server
itself ships inside the Shilpi theme and runs on your own WordPress install, so
there is no service to sign up for and no third party sits between the agent and
your site.

- Product and pricing: <https://shilpibuilder.com/>
- Documentation: <https://shilpibuilder.com/docs/>
- Permission model, explained: <https://shilpibuilder.com/abilities-not-access/>

## Why this exists

Most WordPress page builders store a layout in their own format. An agent asked
to edit such a page has to learn a proprietary widget tree, guess at its
invariants and hope the vendor does not change them.

Shilpi stores a page as three plain fields: body HTML, a real stylesheet, and
focused JavaScript. An agent editing a Shilpi page is editing markup it already
understands. What it needs from the site is not raw access but vocabulary: which
global classes exist, which design variables are defined, and what the house
rules are. That is what these tools provide.

## Endpoints

The server runs on your own site. Replace `example.com` with your domain.

| Purpose | URL |
| --- | --- |
| Streamable HTTP, OAuth | `https://example.com/wp-json/mcp/shilpi-oauth` |
| Streamable HTTP, application password | `https://example.com/wp-json/shilpi/v1/mcp` |
| OAuth protected-resource metadata | `https://example.com/wp-json/shilpi/v1/oauth/protected-resource` |
| Site summary for agents | `https://example.com/llms.txt` |

Both endpoints require an active licence. Without one they return 404, and no
credential will authenticate. Switching the server off in
**Shilpi → Dashboard → Connect an AI agent** removes both endpoints, which makes
it a genuine kill switch rather than a setting.

## Requirements

- WordPress 6.4 or newer
- PHP 8.0 or newer
- HTTPS, required for OAuth
- The Shilpi theme, active, with a valid licence

## Getting connected

See [docs/authentication.md](docs/authentication.md) for the OAuth flow and the
application-password fallback, and [examples/](examples/) for ready-to-paste
client configuration.

## Abilities

Every tool is scoped. There is no filesystem access, no SSH, and no route to
`wp-config.php` or to theme and plugin files. Writing PHP snippets is a
temporary permission the site owner grants at **Shilpi → Security** and revokes
in one click.

The list below is generated from a live install running Shilpi 1.9.4. Counts
change between releases, and the WooCommerce Kit plugin adds its own tools when
it is active.

### Pages, posts and templates

| Tool | What it does |
| --- | --- |
| `shilpi-site-info` | Site identity, versions, inventory, breakpoints, limits, template routing |
| `shilpi-list-pages` | List pages, posts and templates with ids, slugs and permalinks |
| `shilpi-get-page` | Read one document: body HTML, page CSS, page JS, header and footer |
| `shilpi-save-page` | Create or update a page, post or template, idempotent by slug |
| `shilpi-set-page-code` | Set a document's page CSS and JavaScript |
| `shilpi-delete-page` | Remove a document |
| `shilpi-import-page` | Import one static HTML file as a Shilpi document |
| `shilpi-import-site` | Import a folder of static HTML: templates, CSS, images and menu |

### Design system

| Tool | What it does |
| --- | --- |
| `shilpi-manage-variables` | List and upsert design tokens, with per-breakpoint values |
| `shilpi-manage-classes` | List and manage global CSS classes and their states |
| `shilpi-get-globals` | Read global CSS and JS, and the default header, footer and front page |
| `shilpi-set-globals` | Write those global settings |

### Structure and content

| Tool | What it does |
| --- | --- |
| `shilpi-manage-menu` | Create and order navigation menus, assign them to locations |
| `shilpi-manage-taxonomy` | Manage categories, tags and custom taxonomies |
| `shilpi-get-seo` | Read title, description, canonical, robots and social fields |
| `shilpi-set-seo` | Write them through whichever SEO plugin the site runs |
| `shilpi-upload-media` | Sideload an image into the media library, idempotent by source URL |
| `shilpi-search-icons` | Search the installed icon libraries |
| `shilpi-manage-icon-libraries` | Add and remove icon libraries |

### Code and assets

| Tool | What it does |
| --- | --- |
| `shilpi-list-php-snippets` | List Code Hub snippets with their enabled and error state |
| `shilpi-get-php-snippet` | Read one snippet and its content hash |
| `shilpi-save-php-snippet` | Create or update a snippet, syntax-checked, hash-guarded |
| `shilpi-configure-php-snippet` | Enable or disable a snippet, test-run before enabling |
| `shilpi-delete-php-snippet` | Remove a snippet |
| `shilpi-list-script-files` | List managed JavaScript files |
| `shilpi-upload-script-file` | Upload a trusted library or bundle |
| `shilpi-configure-script-file` | Set enabled state, load priority and jQuery dependency |
| `shilpi-list-project-files` | List project JSON and asset files |
| `shilpi-upload-project-file` | Upload generated JSON, returns a same-origin URL |
| `shilpi-delete-project-file` | Remove a project or script file |
| `shilpi-set-site-manifest` | Activate a web-app manifest |
| `shilpi-list-fonts` | List self-hosted font faces |
| `shilpi-upload-font` | Upload a WOFF2, WOFF, TTF or OTF face and register it |
| `shilpi-delete-font` | Remove a font face |

### Agent instructions

| Tool | What it does |
| --- | --- |
| `shilpi-get-skill` | Load a Markdown skill: `shilpi-theme` or `wordpress-core` |

Skills are how the site tells the agent its own conventions. You can upload your
own Markdown skill to add house rules, and the agent reads it before writing.

## What the server cannot do

This is the part worth reading before you connect anything.

- No filesystem access. It cannot read or write theme or plugin files.
- No shell, no SSH, no `wp-config.php`.
- No customers, orders, addresses or payment data through the WooCommerce tools.
- Writing PHP is off by default and is a permission with an off switch, not a
  setting you enable and forget.
- Uploading JavaScript requires an account with `unfiltered_html`.

## Licence

The contents of this repository (manifest, docs and examples) are released under
the MIT licence. The Shilpi theme itself is commercial software; see
<https://shilpibuilder.com/pricing/>.
