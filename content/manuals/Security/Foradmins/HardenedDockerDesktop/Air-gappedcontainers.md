+++
title = "Air-gapped containers"
date = 2024-10-23T14:54:40+08:00
weight = 40
type = "docs"
description = ""
isCJKLanguage = true
draft = false
+++

> 原文: [https://docs.docker.com/security/for-admins/hardened-desktop/air-gapped-containers/](https://docs.docker.com/security/for-admins/hardened-desktop/air-gapped-containers/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# Air-gapped containers

Introduced in Docker Desktop version [4.29.0](https://docs.docker.com/desktop/release-notes/#4290)

Air-Gapped Containers allows administrators to restrict containers from accessing network resources, limiting where data can be uploaded to or downloaded from.

Docker Desktop can apply a custom set of proxy rules to network traffic from containers. The proxy can be configured to:

- Allow network connections
- Reject network connections
- Tunnel through an HTTP or SOCKS proxy

You can choose:

- Which outgoing TCP ports the policy applies to. For example, only certain ports, `80`, `443` or all with `*`.
- Whether to forward to a single HTTP or SOCKS proxy, or to have a policy per destination via a Proxy Auto-Configuration (PAC) file.

## [Configuration](https://docs.docker.com/security/for-admins/hardened-desktop/air-gapped-containers/#configuration)

Assuming [enforced sign-in](https://docs.docker.com/security/for-admins/enforce-sign-in/) and [Settings Management](https://docs.docker.com/security/for-admins/hardened-desktop/settings-management/) are enabled, add the new proxy configuration to the `admin-settings.json` file. For example:



```json
{
  "configurationFileVersion": 2,
  "containersProxy": {
    "locked": true,
    "mode": "manual",
    "http": "",
    "https": "",
    "exclude": "",
    "pac": "http://192.168.1.16:62039/proxy.pac",
    "transparentPorts": "*"
  }
}
```

The `containersProxy` setting describes the policy which is applied to traffic from containers. The valid fields are:

- `locked`: If true, it is not possible for developers to override these settings. If false the settings are interpreted as default values which the developer can change.
- `mode`: Same meaning as with the existing `proxy` setting. Possible values are `system` and `manual`.
- `http`, `https`, `exclude`: Same meaning as with the `proxy` setting. Only takes effect if `mode` is set to `manual`.
- `pac` : URL for a PAC file. Only takes effect if `mode` is `manual`, and is considered higher priority than `http`, `https`, `exclude`.
- `transparentPorts`: A comma-separated list of ports (e.g. `"80,443,8080"`) or a wildcard (`*`) indicating which ports should be proxied.

> **Important**
>
> 
>
> Any existing `proxy` setting in the `admin-settings.json` file continues to apply to traffic from the app on the host.

## [Example PAC file](https://docs.docker.com/security/for-admins/hardened-desktop/air-gapped-containers/#example-pac-file)

For general information about PAC files, see the [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Proxy_servers_and_tunneling/Proxy_Auto-Configuration_PAC_file).

The following is an example PAC file:



```javascript
function FindProxyForURL(url, host) {
	if (localHostOrDomainIs(host, 'internal.corp')) {
		return "PROXY 10.0.0.1:3128";
	}
	if (isInNet(host, "192.168.0.0", "255.255.255.0")) {
	    return "DIRECT";
	}
    return "PROXY reject.docker.internal:1234";
}
```

The `url` parameter is either `http://host_or_ip:port` or `https://host_or_ip:port`.

The hostname is normally available for outgoing requests on port `80` and `443`, but for other cases there is only an IP address.

The `FindProxyForURL` can return the following values:

- `PROXY host_or_ip:port`: Tunnels this request through the HTTP proxy `host_or_ip:port`
- `SOCKS5 host_or_ip:port`: Tunnels this request through the SOCKS proxy `host_or_ip:port`
- `DIRECT`: Allows this request to go direct, without a proxy
- `PROXY reject.docker.internal:any_port`: Rejects this request

In this particular example, HTTP and HTTPS requests for `internal.corp` are sent via the HTTP proxy `10.0.0.1:3128`. Requests to connect to IPs on the subnet `192.168.0.0/24` connect directly. All other requests are blocked.

To restrict traffic connecting to ports on the developers local machine, [match the special hostname `host.docker.internal`](https://docs.docker.com/desktop/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host).

[Edit this page](https://github.com/docker/docs/edit/main/content/manuals/security/for-admins/hardened-desktop/air-gapped-containers.md)

[Request changes](https://github.com/docker/docs/issues/new?template=doc_issue.yml&location=https%3a%2f%2fdocs.docker.com%2fsecurity%2ffor-admins%2fhardened-desktop%2fair-gapped-containers%2f&labels=status%2Ftriage)

Table of contents

[Configuration](https://docs.docker.com/security/for-admins/hardened-desktop/air-gapped-containers/#configuration)[Example PAC file](https://docs.docker.com/security/for-admins/hardened-desktop/air-gapped-containers/#example-pac-file)
