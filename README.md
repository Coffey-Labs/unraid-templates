# Coffey Labs Unraid templates

Docker templates for [Unraid Community Applications](https://ca.unraid.net),
one XML file per app under `templates/`. `ca_profile.xml` at the root is what
Community Applications reads for the repository listing.

| App | Template | Image | Licence of the app |
| --- | --- | --- | --- |
| [ihasmail](https://ihasmail.org) — immutable webmail for Stalwart Mail Server | [`templates/ihasmail.xml`](templates/ihasmail.xml) | `ghcr.io/coffey-labs/ihasmail:latest` (amd64, arm64) | AGPL-3.0-or-later |

## Installing

Once the repository is accepted into Community Applications, search for the
app under **Apps** and install it from there.

Until then, or if you prefer to skip the store, copy the template onto the
flash drive from the Unraid terminal and it appears under **User templates**
in the **Template** drop-down of **Docker → Add Container**:

```sh
wget -P /boot/config/plugins/dockerMan/templates-user/ \
  https://raw.githubusercontent.com/Coffey-Labs/unraid-templates/main/templates/ihasmail.xml
```

## ihasmail

ihasmail is a mail client, not a mail server. It needs a
[Stalwart Mail Server](https://stalw.art) 0.16 or newer that the container can
reach, and nothing else: no database, no appdata to migrate, no IMAP or SMTP
settings. Everything durable lives in Stalwart.

The template runs the container read-only, as Unraid's `nobody` user, with a
`/tmp` in memory. The one optional write is `sessions.json` under
`/mnt/user/appdata/ihasmail`, kept so nobody is signed out when the container
restarts or updates. Clear that path, blank `SESSION_FILE` and set
`IMMUTABLE=1` to run with no writable path at all, which is how
[ihasmail.com](https://ihasmail.com) runs it.

Two things to set on first install:

- **Stalwart URL** — scheme and host, no path, e.g. `https://mail.example.com`.
- **App secret** — a long random string. In the Unraid terminal:
  `openssl rand -base64 48`.

Put it behind a reverse proxy with HTTPS before exposing it beyond your LAN.
Every other setting is documented at
[docs.ihasmail.org/configure](https://docs.ihasmail.org/configure/).

Problems with the app go to
[Coffey-Labs/ihasmail issues](https://github.com/Coffey-Labs/ihasmail/issues);
problems with the template go to the issues on this repository.

## Licence

The templates in this repository are MIT licensed. Each app keeps its own
licence, shown in the table above.
