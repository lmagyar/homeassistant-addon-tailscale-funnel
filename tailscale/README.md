# Home Assistant Custom Add-on: Tailscale

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

Zero config VPN for building secure networks.

[![GitHub Release][releases-shield]][releases]
[![Last Updated][updated-shield]][updated]
![Reported Installations][installations-shield]
![Project Stage][project-stage-shield]
[![License][license-shield]][licence]

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports armhf Architecture][armhf-shield]
![Supports armv7 Architecture][armv7-shield]
![Supports i386 Architecture][i386-shield]

[![Github Actions][github-actions-shield]][github-actions]
![Project Maintenance][maintenance-shield]
[![GitHub Activity][commits-shield]][commits]

## About

Tailscale is a zero config VPN, which installs on any device in minutes,
including your Home Assistant instance.

Create a secure network between your servers, computers, and cloud instances.
Even when separated by firewalls or subnets, Tailscale just works. Tailscale
manages firewall rules for you, and works from anywhere you are.

[:books: Read the full add-on documentation][docs]

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[commits-shield]: https://img.shields.io/github/commit-activity/y/lmagyar/homeassistant-addon-tailscale-funnel.svg
[commits]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/commits/main
[docs]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/blob/main/tailscale/DOCS.md
[github-actions-shield]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/workflows/Publish/badge.svg
[github-actions]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/actions
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg
[installations-shield]: https://img.shields.io/badge/dynamic/json?label=reported%20installations&query=$[%2788fc4088_tailscale%27].total&url=https%3A%2F%2Fanalytics.home-assistant.io%2Faddons.json
[license-shield]: https://img.shields.io/github/license/lmagyar/homeassistant-addon-tailscale-funnel.svg
[licence]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/blob/main/LICENSE
[maintenance-shield]: https://img.shields.io/maintenance/yes/2023.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-alpha-orange.svg
[releases-shield]: https://img.shields.io/github/tag/lmagyar/homeassistant-addon-tailscale-funnel.svg?label=release
[releases]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/tags
[updated-shield]: https://img.shields.io/github/last-commit/lmagyar/homeassistant-addon-tailscale-funnel/main?label=updated
[updated]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/commits/main
[warning_stripe]: https://github.com/lmagyar/homeassistant-addon-tailscale-funnel/raw/main/images/warning_stripe_wide.png
[community_addon]: https://github.com/hassio-addons/addon-tailscale
