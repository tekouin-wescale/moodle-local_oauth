# moodle-local_oauth

Campusna-maintained Moodle plugin that turns Moodle into an **OAuth2 provider**, so external apps can let users sign in with their Moodle account.

| Field | Value |
| --- | --- |
| Component | `local_oauth` |
| Install path | `moodle/local/oauth` |
| Org | `tekouin-wescale` |
| Status | **maintained** (Campusna / Jupiter Moodle) |
| Role | Public Moodle plugin fork (OAuth2 provider) |
| Upstream | Fork of [`projectestac/moodle-local_oauth`](https://github.com/projectestac/moodle-local_oauth) (lineage from cognitivabrasil / Estac) |
| Docs | keep README as primary install guide |

## Why this fork exists

The original public plugin is no longer a reliable source of truth for current Moodle. Campusna keeps this fork updated for production Moodle (including work tested against **Moodle 4.5.1 / Jupiter**). Prefer this repository over abandoned clones when deploying for Campusna.

OAuth2 library: [bshaffer/oauth2-server-php](https://github.com/bshaffer/oauth2-server-php).

## Requirements

- Moodle 2.8 or higher (Campusna target: Moodle 4.5.x)
- Moodle admin account for install and client registration

## Installation

1. Clone into Moodle as `local/oauth`:

```bash
cd /path/to/moodle/local
git clone https://github.com/tekouin-wescale/moodle-local_oauth.git oauth
```

2. Or install via Site administration > Plugins > Install plugins (zip of a folder named `oauth`).

3. Complete the Moodle upgrade / plugin install prompts.

4. Ensure `moodle/local/` is writable during install if Moodle copies files itself.

5. Go to **Site administration > Server > OAuth provider settings**.

6. **Add new client**, set Client Identifier, redirect URL, then note the Client Secret.

## How to use

1. Redirect the user to:

`https://moodledomain.com/local/oauth/login.php?client_id=EXAMPLE&response_type=code`

Replace domain and `EXAMPLE` with your Client Identifier.

2. User logs in to Moodle and authorizes the application.

3. Moodle redirects to your Redirect URL with a `code` query parameter.

4. Exchange the code with a POST to `https://moodledomain.com/local/oauth/token.php`:

```text
code, client_id, client_secret, grant_type=authorization_code, scope=user_info
```

Successful response (example):

```json
{
  "access_token": "...",
  "expires_in": 3600,
  "token_type": "Bearer",
  "scope": "user_info",
  "refresh_token": "..."
}
```

5. Fetch profile with a POST to `https://moodledomain.com/local/oauth/user_info.php` using `access_token`.

Example response:

```json
{
  "id": "22",
  "username": "foobar",
  "firstname": "Foo",
  "lastname": "Bar",
  "email": "foo@bar.com",
  "lang": "en"
}
```

## Maintenance notes

- This repo is the Campusna-supported copy for Tekouin / Jupiter Moodle SSO-style integrations.
- Report issues and send PRs here (`tekouin-wescale/moodle-local_oauth`), not to abandoned upstream mirrors.
- Historical testing mentioned Moodle 2.8 / 3.0; Campusna has since applied updates for Moodle 4.5.1.

## Contributors

Historical upstream contributors (non-exhaustive): people on the original repositories, including [igorpf](https://github.com/igorpf). Campusna maintainers continue updates in this org.
