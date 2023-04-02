# Changelog

## vNext (forked)

- Enable Tailscale's SOCKS5 and HTTP outbound proxy

## 0.9.0.1 (forked)

- Move Tailscale Proxy and Funnel functionality into standalone oneshot S6 service
- Remove SSH feature
- Merge changes from original add-on
  - Advertise all supported interfaces as Tailscale Subnets
  - Suppress tailscaled logs after 200 lines
  - Bump Tailscale to 1.38.3
  - Bump base image to 13.2.0

## 0.8.0.1 (forked)

- Merge PR modifications
- Merge changes from original add-on
  - Migrate old-style S6 scripts to s6-rc.d
  - Bump base image to 13.1.4

## 0.7.0.13 (forked)

- Bump Tailscale to 1.38.2

## 0.7.0.12 (forked)

- Bugfix: use new tailscale serve command syntax

## 0.7.0.11 (forked)

- Bump Tailscale to 1.38.1
- Bump base image to 13.1.3

## 0.7.0.10 (forked)

- Bump Tailscale to 1.36.2
- Bump base image to 13.1.2

## ~~0.7.0.9 (forked)~~

_This version number is skipped, just to be in sync with the [non-Funnel version repo](https://github.com/lmagyar/homeassistant-addon-tailscale)._

## 0.7.0.8 (forked)

- Bump Tailscale to 1.36.1

## 0.7.0.7 (forked)

- Use `log_level` configuration option for tailscaled debug messages
- Use only info messages if the add-on's configuration is incomplete

## 0.7.0.6 (forked)

- Bump Tailscale to 1.36.0
- Bump base image to 13.1.1

## 0.7.0.5 (forked)

- Only optionally enable tailscaled debug messages in the add-on's log

## 0.7.0.4 (forked)

- Bump base image to 13.1.0

## 0.7.0.3 (forked)

- Bugfix

## 0.7.0.2 (forked)

- Enable Tailscale's SSH feature

## 0.7.0.1 (forked)

- Enable Tailscale's Funnel feature
- Enable Tailscale's Proxy feature

## 0.7.0.0 (forked)

- Advertise all supported interfaces as Subnets
- Bump Tailscale to 1.34.2
- Bump base image to 13.0.1
- Fork of the original v0.7.0

For previous changelog see the original add-on's [release history](https://github.com/hassio-addons/addon-tailscale/releases).
