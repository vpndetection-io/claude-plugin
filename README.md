# [<img src="https://s3.vpndetection.io/vpndetection-public/brand/mark.svg" alt="VPNDetection" height="28"/>](https://vpndetection.io/) VPNDetection Claude Plugin

[![version](https://img.shields.io/github/package-json/v/vpndetection-io/claude-plugin?filename=.claude-plugin%2Fplugin.json)](CHANGELOG.md)
[![license](https://img.shields.io/github/license/vpndetection-io/claude-plugin)](LICENSE)

The official [VPNDetection](https://vpndetection.io) plugin for [Claude](https://claude.com).

Ask Claude whether an IP address belongs to a VPN, a residential, datacenter or mobile proxy, a Tor node, a public relay, a hosting provider or a CDN, and look into the VPNDetection databases your organization is licensed for. It works in Claude on the web, desktop and mobile, in Cowork and in Claude Code.

## Getting Started

On claude.ai, in the desktop app or in Cowork, add VPNDetection from the directory, then connect it on the plugin's Connectors tab.

In Claude Code:

```console
/plugin marketplace add vpndetection-io/claude-plugin
/plugin install vpndetection@vpndetection
```

The first time a tool runs, you'll be asked to sign in with your VPNDetection account and pick the API key Claude should use. Claude never sees the key: our MCP server uses it on your behalf, and requests count against that key's plan like any other API call. You need a VPNDetection account for this; the free plan works, and returns `ip` and `is_vpn`.

To disconnect Claude, remove it under Connected applications at https://app.vpndetection.io/settings/account/sessions. Access ends within about a minute.

## What's in it

- The VPNDetection MCP server at `https://mcp.vpndetection.io/mcp`, with seven read-only tools: `lookup_ip`, `lookup_ips`, `my_entitlement`, `list_databases`, `database_metadata`, `database_checksum` and `list_downloads`.
- Four skills that tell Claude how to use them well:
  - `check-ips` checks one address or a list, and reads the result's `coverage` so a field your plan doesn't include is never reported as a "no".
  - `review-traffic` screens a signup, login or order export and keeps each verdict next to the record it came from.
  - `database-files` answers which databases you hold, how big they are, whether your copy is intact and why a download failed.
  - `analyze-database` explains a database's columns and sample rows, and in Claude Code analyzes a downloaded copy.

## Try it

- "Is 45.83.91.1 a VPN?"
- "Here's yesterday's signup export. Which accounts came from proxies or Tor?"
- "Which databases are we licensed for, and how big is the latest VPN build?"

## What it sends

The plugin runs nothing on your machine. Claude sends the IP addresses and database ids you ask about to `https://mcp.vpndetection.io/mcp`, which calls the VPNDetection API with the key you picked and returns the answer. Each request is logged against that key as any API call is. Our privacy policy is at https://vpndetection.io/privacy, and questions go to support@vpndetection.io.

## Other Libraries

There are official VPNDetection client libraries available for many languages including PHP, Python, Go, Java, Ruby, and many popular frameworks such as Django, Rails, and Laravel. See our GitHub at https://github.com/vpndetection-io for more.

## About VPNDetection

VPN Detection API: Accurate anonymity detection identifying VPNs, residential proxies, hosting servers, Tor nodes, CDNs, relays and more.

[<img src="https://s3.vpndetection.io/vpndetection-public/brand/mark.svg" alt="VPNDetection" height="64"/>](https://vpndetection.io/)

## License

This project is licensed under the [MIT License](LICENSE).
