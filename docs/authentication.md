# Connecting an agent

Two routes. OAuth is the one to use. The application-password route exists for
clients that cannot do a browser sign-in.

Before either, the server has to be on:

1. Activate a licence at **Shilpi → Dashboard → Licence**. The MCP server does
   not answer without one.
2. Open **Shilpi → Dashboard → Connect an AI agent** and confirm the server is
   on.
3. Press **Test server**. It performs a real authenticated MCP handshake rather
   than pinging a URL, so a pass here means a client will connect.

## OAuth, recommended

The agent signs in through the browser and no password is copied into a config
file. HTTPS is required.

Endpoint:

```
https://example.com/wp-json/mcp/shilpi-oauth
```

Discovery metadata, if your client asks for it:

```
https://example.com/wp-json/shilpi/v1/oauth/protected-resource
https://example.com/wp-json/shilpi/v1/oauth/authorization-server
```

Dynamic client registration is supported at
`/wp-json/shilpi/v1/oauth/register`, so most clients need only the first URL.

## Application password, fallback

For a client that cannot complete an OAuth flow.

1. In WordPress, go to **Users → Profile → Application Passwords**.
2. Create one named after the client, for example `claude-desktop`.
3. Copy the generated password. WordPress shows it once.

Endpoint:

```
https://example.com/wp-json/shilpi/v1/mcp
```

Authenticate with HTTP Basic, using your WordPress username and the application
password. Treat it as a credential: it is not scoped to Shilpi, and revoking it
from the same screen cuts the connection immediately.

## Turning it off

**Shilpi → Dashboard → Connect an AI agent** has an off switch. Turning it off
removes both endpoints, so existing credentials stop working rather than merely
being asked not to. A lapsed licence has the same effect. The state is
remembered, so activating a key brings the server straight back.

Separately, **Shilpi → Security** lists every temporary permission that is
currently open, PHP snippet access among them, and closes all of them with one
button. Closing a permission never deletes anything that was already created.

## Skills

Two Markdown skills ship with the theme and can be downloaded from
**Shilpi → Skills**:

- `shilpi-theme`, everything the theme owns: storage model, styling contract,
  dynamic tokens, templates, forms, sliders, PHP snippets, assets.
- `wordpress-core`, native posts, pages, media, menus, users and settings.

Load one with `shilpi-get-skill` at the start of a session. You can upload your
own Markdown skill to add house conventions; uploading the same slug again
updates it.
