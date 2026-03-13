# Canva MCP config reference

The Canva MCP server connects via the Canva Connect API. It requires an OAuth access token with the appropriate scopes for the tools you plan to use.

## Required scopes

The following scopes may be needed depending on which tools you use:

- `design:content:read` — read design content, pages, metadata
- `design:content:write` — edit designs, commit transactions
- `design:permission:read` — list comments and replies
- `design:permission:write` — add comments and replies
- `folder:read` — browse folders and list items
- `folder:write` — create folders, move items
- `asset:write` — upload assets from URL
- `brandkit:read` — list brand kits
- `brand_template:content:read` — search brand templates, get dataset schemas
- `brand_template:meta:read` — read brand template metadata

If a tool returns `Missing scopes: [...]`, disconnect and reconnect the Canva connector to generate a new token with the required scope.

## Setup

1. Obtain a Canva Connect API OAuth token from your Canva integration settings.
2. Register the MCP server in your agent configuration, providing the token for authentication.
3. Restart your agent/IDE after updating config and environment variables.

## Verification checklist

- Confirm the MCP server is reachable by listing available tools or calling `search-designs` with a simple query.
- Verify that `search-brand-templates` returns results if your account has brand templates.
- Test `get-design` with a known design ID to confirm read access.

## Troubleshooting

- **Token not picked up**: Ensure the OAuth token is available in the environment that launches your agent.
- **Scope errors**: If a tool call fails with a missing scope error, regenerate the token with the needed scopes.
- **Design not found**: Verify the design ID is exactly 11 characters and starts with `D`. If using a shortlink, resolve it with `resolve-shortlink` first.
- **Export fails**: Check `get-export-formats` to confirm the design supports the requested format.

## Design ID extraction

Canva design URLs follow this pattern:
```
https://www.canva.com/design/{design_id}/...
```

The `design_id` is the 11-character string starting with `D` (e.g., `DAGxxxxxxxxx`). Extract it from URLs before passing to tools like `get-design`, `start-editing-transaction`, or `export-design`.

For shortlinks (`https://canva.link/abc123`), use `resolve-shortlink` with the shortlink ID to get the full URL first.
