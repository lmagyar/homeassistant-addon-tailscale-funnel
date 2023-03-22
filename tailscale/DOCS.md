# Home Assistant Custom Add-on: Tailscale with Funnel support

![Warning][warning_stripe]

> This is a **fork** of the abandoned [community add-on][community_addon]!
>
> Use this [invite link](https://login.tailscale.com/admin/feature/rWXbjRuCEc9)
> to get access to the Funnel feature.
>
> This fork:
>   - Enables Tailscale's https Proxy, Funnel and SSH features
>   - Advertises all supported interfaces as Subnets
>   - Bumps Tailscale to 1.38.1
>   - Bumps base image to 13.1.3

![Warning][warning_stripe]

## Prerequisites

In order to use this add-on, you'll need a Tailscale account.

It is free to use for personal & hobby projects, up to 20 clients/devices on a
single user account. Sign up using your Google, Microsoft or GitHub account at
the following URL:

<https://login.tailscale.com/start>

## Tailscale Funnel prerequisites

With the Tailscale Funnel feature you can access your Home Assistant instance
from the wider internet using your Tailscale domain (like
`https://homeassistant.tail1234.ts.net`) even from devices **without installed
Tailscale VPN client** (eg. general phones, tablets, laptops).

> **Client** &#8658; _Internet_ &#8658; **Tailscale Funnel** (TCP proxy) &#8658;
  _VPN_ &#8658; **Tailscale Proxy** (https proxy) &#8594; **HA** (http
  web-server)

Without the Tailscale Funnel feature, you will be able to access your Home
Assistant instance only when your devices (eg. phones, tablets, laptops) are
connected to your Tailscale VPN, there will be no Internet &#8658; VPN TCP
proxying for https communication.

See [Tailscale Funnel][tailscale_info_funnel] for more information.

> **Note:** _This is an alpha feature that is invite only. Please go to
  Tailscale's [Settings / Feature Previews page][tailscale_feature], and under
  the **Funnel** section check that you are invited and have access to this
  feature._
  
To use the Tailscale Funnel and SSH features, you need to modify some of your
Home Assistant and Tailscale settings.

### Home Assistant configuration

You must configure Home Assistant to **not** use SSL certificates, to be
accessible through plain http connection. The Tailscale https Proxy will access
Home Assistant through `localhost` and will not accept a real certificate,
connection will be closed with `proxy error: x509: cannot validate certificate
for 127.0.0.1 because it doesn't contain any IP SANs`

If you still want to use another https connection to access Home Assistant
through another network, please use the **NGINX Home Assistant SSL proxy**
add-on.

So these lines have to be removed from your `/config/configuration.yaml`:

```yaml
http:
#  ssl_certificate: /ssl/fullchain.pem
#  ssl_key: /ssl/privkey.pem
```

Since Home Assistant by default blocks requests from proxies/reverse proxies,
you need to tell your instance to allow requests from the Tailscale add-on.
In order to do so, add the following lines to your `/config/configuration.yaml`
without changing anything:

```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 127.0.0.1
```

**Note**: _There is no need to adapt anything in these lines since the addon
runs on your host network._

### Tailscale configuration

1. [DNS page][tailscale_dns]: Choose a **Tailnet name** and click **Enable
   HTTPS** under HTTPS Certificates (see [Enabling HTTPS][tailscale_info_https]
   for more information)
1. [Access Controls page][tailscale_acls]: Add the below policy entries to the
   policy file (see [ACL tags][tailscale_info_acl], [Tailscale
   SSH][tailscale_info_ssh], [Tailscale Funnel][tailscale_info_funnel] for more
   information)

```json
{
  // (other tailnet policy entries here)
  "tagOwners": {
    "tag:funnel": ["<CHANGE-IT-TO-YOUR-TAILSCALE-LOGIN-EMAIL-ADDRESS>"],
    "tag:ssh":    ["<CHANGE-IT-TO-YOUR-TAILSCALE-LOGIN-EMAIL-ADDRESS>"],
  },
  "ssh": [
    {
      "action": "check",
      "src":    ["autogroup:members"],
      "dst":    ["tag:ssh"],
      "users":  ["autogroup:nonroot", "root"],
    },
  ],
  "nodeAttrs": [
    {
      "target": ["tag:funnel"],
      "attr":   ["funnel"],
    },
  ],
}
```

**Note**: _Replace \<CHANGE-IT-TO-YOUR-TAILSCALE-LOGIN-EMAIL-ADDRESS\> with your
email address!_

## Installation

1. Navigate in your Home Assistant frontend to **Settings** -> **Add-ons** ->
   **Add-on Store**.
1. In the **...** menu at the top right corner click **Repositories**, add
   `https://github.com/lmagyar/homeassistant-addon-tailscale` as repository.
1. Find the "Tailscale" add-on and click it. If it doesn't show up, wait until
   HA refreshes the information about the add-on, or click **Reload** in the
   **...** menu at the top right corner.
1. Click the "Install" button to install the add-on.
1. Start the "Tailscale" add-on.
1. Check the logs of the "Tailscale" add-on to see if everything went well.
1. Open the **Web UI** of the "Tailscale" add-on to complete authentication and
   couple your Home Assistant instance with your Tailscale account.

   **Note:** _Some browsers don't work with this step. It is recommended to
   complete this step on a desktop or laptop computer using the Chrome browser._

1. Done!

## Configuration

This add-on has almost no additional configuration options for the
add-on itself.

However, when logging in to Tailscale, you can configure your Tailscale
network right from their interface.

<https://login.tailscale.com/>

## Required Tailscale configuration

1. Find your Home Assistant instance in the [Machines tab][tailscale_machines]
1. Click on the **&hellip;** icon at the far right and select the **Edit ACL
   tags...** option
   - Add `tag:funnel` and `tag:ssh` to the list (see **Prerequisites** above if
     you can't find them)
   - Click Save to apply tags
   - Restart the add-on
   - Your Home Assistant instance should now be reachable under
     `https://<machine-name>.<tailnet-name>.ts.net` from devices on the internet
     without Tailscale VPN client 

   **Note**: _After initial set up it can take up to 10 minutes for the domain
   to be publicly available. You can use the `dig` command (Linux/MacOS) to
   regularly check if an A-record is already present for your domain (`dig
   <machine-name>.<tailnet-name>.ts.net +short` should return an IP address once
   the record is published)._

   **Note:** _You should not use any port number that you used previously to
   access Home Assistant from the local network._

   **Note:** _If you encounter strange browser behaviour or strange error
   messages, try to clear all site related cookies, clear all browser cache,
   restart browser_

## Optional Tailscale configuration

1. Find your Home Assistant instance in the [Machines tab][tailscale_machines]
1. Click on the **&hellip;** icon at the far right and select the **Edit route
   settings...** option
   - The add-on exposes **Exit Node** capabilities that you can enable from your
     Tailscale account
   - Additionally, if the Supervisor managed your network (which is the
     default), the add-on will also advertise routes to your **Subnets** on all
     supported interfaces, that you can enable from your Tailscale account
1. Click on the **&hellip;** icon at the far right and select the **Disable key
   expiry** option

## Add-on configuration

```yaml
tags:
  - tag:example
  - tag:homeassistant
log_level: info
```

### Option: `tags`

This option allows you to specify specific ACL tags for this Tailscale
instance. They need to start with `tag:`.

More information: [ACL tags][tailscale_info_acl]

### Option: `log_level`

Optionally enable tailscaled debug messages in the add-on's log. Turn it on only
in case you are troubleshooting, because Tailscale's daemon is quite chatty.

The `log_level` option controls the level of log output by the addon and can
be changed to be more or less verbose, which might be useful when you are
dealing with an unknown issue. Possible values are:

- `trace`: Show every detail, like all called internal functions.
- `debug`: Shows detailed debug information.
- `info`: Normal (usually) interesting events.
- `notice`: Normal but significant events.
- `warning`: Exceptional occurrences that are not errors.
- `error`: Runtime errors that do not require immediate action.
- `fatal`: Something went terribly wrong. Add-on becomes unusable.

Please note that each level automatically includes log messages from a
more severe level, e.g., `debug` also shows `info` messages. By default,
the `log_level` is set to `info`, which is the recommended setting unless
you are troubleshooting.

## Support

Got questions?

You have several options to get them answered:

- The [Home Assistant Discord chat server][discord].
- The Home Assistant [Community Forum][forum].
- Join the [Reddit subreddit][reddit] in [/r/homeassistant][reddit]

You could also [open an issue here with the original add-on][issue] or [open an
issue here with the forked add-on][issue_forked] on GitHub.

[discord]: https://discord.gg/c5DvZ4e
[forum]: https://community.home-assistant.io/
[issue]: https://github.com/hassio-addons/addon-tailscale/issues
[issue_forked]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/issues
[reddit]: https://reddit.com/r/homeassistant
[warning_stripe]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/raw/main/images/warning_stripe_wide.png
[community_addon]: https://github.com/hassio-addons/addon-tailscale
[tailscale_acls]: https://login.tailscale.com/admin/acls
[tailscale_dns]: https://login.tailscale.com/admin/dns
[tailscale_feature]: https://login.tailscale.com/admin/settings/features
[tailscale_info_acl]: https://tailscale.com/kb/1068/acl-tags/
[tailscale_info_funnel]: https://tailscale.com/kb/1223/tailscale-funnel/
[tailscale_info_https]: https://tailscale.com/kb/1153/enabling-https/
[tailscale_info_ssh]: https://tailscale.com/kb/1193/tailscale-ssh/
[tailscale_machines]: https://login.tailscale.com/admin/machines
