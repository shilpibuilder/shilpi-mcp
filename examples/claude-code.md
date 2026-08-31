# Claude Code

Add the server with the CLI, replacing example.com with your site:

```bash
claude mcp add --transport http shilpi https://example.com/wp-json/mcp/shilpi-oauth
```

Claude Code opens a browser window for the OAuth sign-in on first use.

Then, at the start of a build, ask the agent to load the site's own conventions
before it writes anything:

```
Read the site with shilpi-site-info, load the shilpi-theme skill,
then list the existing pages and global classes before you build.
```
