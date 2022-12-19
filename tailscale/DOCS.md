# Home Assistant Community Add-on: Tailscale

![Warning][warning_stripe]

> This is a **fork** of the [community add-on][community_addon]!
>
> **Installation:** Navigate in your Home Assistant frontend to **Settings** -> **Add-ons** -> **Add-on Store**, in the **...** menu at the top right corner click **Repositories**, add `https://github.com/lmagyar/homeassistant-addon-tailscale-funnel` as repository.
>
> This fork:
>   - Advertises all supported interfaces as Subnets
>   - Bumps Tailscale to 1.34.2
>   - Bumps base image to 13.0.1
>
> For more details, please see the Documentation below.

![Warning][warning_stripe]

Tailscale is a zero config VPN, which installs on any device in minutes,
including your Home Assistant instance.

Create a secure network between your servers, computers, and cloud instances.
Even when separated by firewalls or subnets, Tailscale just works. Tailscale
manages firewall rules for you, and works from anywhere you are.

## Prerequisites

In order to use this add-on, you'll need a Tailscale account.

It is free to use for personal & hobby projects, up to 20 clients/devices on a
single user account. Sign up using your Google, Microsoft or GitHub account at
the following URL:

<https://login.tailscale.com/start>

### Enabling Tailscale Funnel

With the Tailscale Funnel feature you can access your Home Assistant instance
from the wider internet using your Tailscale domain (like
`https://homeassistant.tail1234.ts.net`) even from devices **without installed
Tailscale VPN client** (eg. general phones, tablets, laptops).

Without the Tailscale Funnel feature, you still will be able to access your
Home Assistant instance while you are locally connected to your Tailscale VPN.

To use the Tailscale Funnel feature, you need to modify some of your Home
Assistant and Tailscale settings.

#### Home Assistant prerequisites

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

#### Tailscale prerequisites

1. Go to [DNS page](https://login.tailscale.com/admin/dns) in Tailscale's admin console 
  - Choose a **Tailnet name** and click **Enable HTTPS** under HTTPS Certificates (see [Enabling HTTPS](https://tailscale.com/kb/1153/enabling-https/) for more information)
1. Go to [Settings / Feature Previews page](https://login.tailscale.com/admin/settings/features)
  - Under the **Funnel** section check that you are invited and have access to this feature (see [Tailscale Funnel](https://tailscale.com/kb/1223/tailscale-funnel/) for more information)
1. Go to [Access Controls page](https://login.tailscale.com/admin/acls)
  - Add the below policy entries to the policy file (see [Server role accounts using ACL tags](https://tailscale.com/kb/1068/acl-tags/) for more information)

```json
{
  // (other tailnet policy entries here)
	"tagOwners": {
		"tag:funnel": ["<CHANGE-IT-TO-YOUR-LOGIN-EMAIL-ADDRESS>"],
	},
	"nodeAttrs": [
		{
			"target": ["tag:funnel"],
			"attr":   ["funnel"],
		},
	],
}
```

## Installation

1. Click the Home Assistant My button below to open the add-on on your Home
   Assistant instance.

   [![Open this add-on in your Home Assistant instance.][addon-badge]][addon]

1. Click the "Install" button to install the add-on.
1. Start the "Tailscale" add-on.
1. Check the logs of the "Tailscale" add-on to see if everything went well.
1. Open the Web UI of the "Tailscale" add-on to complete authentication and
   couple your Home Assistant instance with your Tailscale account.
   **Note:** Some browsers don't work with this step. It is recommended to
   complete this step on a desktop or laptop computer using the Chrome browser.
1. Done!

## Configuration

This add-on has almost no additional configuration options for the
add-on itself.

However, when logging in to Tailscale, you can configure your Tailscale
network right from their interface.

<https://login.tailscale.com/>

1. Find your Home-Assistant in the [Machines tab](https://login.tailscale.com/admin/machines) and note under which name your device is reachable
1. Click on the ellipsis icon at the far right and select the **Edit route settings...** option
  - The add-on exposes "Exit Node" capabilities that you can enable from your Tailscale account.
  - Additionally, if the Supervisor managed your network (which is the default), the add-on will also advertise routes to your subnets on all supported interfaces to Tailscale.
1. Click on the ellipsis icon at the far right and select the **Edit ACL tags....** option
  - Add `tag:funnel` to the list
  - Click Save to apply tags
1. Your device should now be reachable under `https://<machine-name>.<tailnet-name>.ts.net` (you should not use any port number you used previously to access Home Assistant) (if you encounter strange browser behaviour or strange error messages, try to clear all site related cookies, clear all browser cache, restart browser)

**Note**: _After initial set up it can take up to 10 minutes for the domain to
be publicly available. You can use the `dig` command (Linux/MacOS) to regulary
check if an A-record is already present for your domain
(`dig <machine-name>.<tailnet-name>.ts.net +short` should return an IP
address once the record is published)._

```yaml
tags:
  - tag:example
  - tag:homeassistant
```

### Option: `tags`

This option allows you to specify specific ACL tags for this Tailscale
instance. They need to start with `tag:`.

More information: <https://tailscale.com/kb/1068/acl-tags/>

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality.

Releases are based on [Semantic Versioning][semver], and use the format
of `MAJOR.MINOR.PATCH`. In a nutshell, the version will be incremented
based on the following:

- `MAJOR`: Incompatible or major changes.
- `MINOR`: Backwards-compatible new features and enhancements.
- `PATCH`: Backwards-compatible bugfixes and package updates.

## Support

Got questions?

You have several options to get them answered:

- The [Home Assistant Community Add-ons Discord chat server][discord] for add-on
  support and feature requests.
- The [Home Assistant Discord chat server][discord-ha] for general Home
  Assistant discussions and questions.
- The Home Assistant [Community Forum][forum].
- Join the [Reddit subreddit][reddit] in [/r/homeassistant][reddit]

You could also [open an issue here with the original add-on][issue] or [open an issue here with the forked add-on][issue_forked] GitHub.

## Authors & contributors

The original setup of this repository is by [Franck Nijhof][frenck].

For a full list of all authors and contributors,
check [the contributor's page][contributors].

## License

MIT License

Copyright (c) 2021-2023 Franck Nijhof

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[addon-badge]: https://my.home-assistant.io/badges/supervisor_addon.svg
[addon]: https://my.home-assistant.io/redirect/supervisor_addon/?addon=88fc4088_tailscale&repository_url=https%3A%2F%2Fgithub.com%2Flmagyar%2Fhomeassistant-addon-tailscale-funnel
[contributors]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/graphs/contributors
[discord-ha]: https://discord.gg/c5DvZ4e
[discord]: https://discord.me/hassioaddons
[forum]: https://community.home-assistant.io/?u=frenck
[frenck]: https://github.com/frenck
[issue]: https://github.com/hassio-addons/addon-tailscale/issues
[issue_forked]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/issues
[reddit]: https://reddit.com/r/homeassistant
[releases]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/releases
[semver]: http://semver.org/spec/v2.0.0.htm
[warning_stripe]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/raw/main/images/warning_stripe_wide.png
[community_addon]: https://github.com/hassio-addons/addon-tailscale
