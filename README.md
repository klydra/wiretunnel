# wiretunnel

A quick and simple yet quite efficient method of port forwarding via a WireGuard reverse VPN.<br/>
Intended use case : Forwarding ports simply from a network behind a CGNAT to a public IP.<br/>
<br/>

## Setup

- **(Virtual-)Machine on the local network**
- **Publicly accessible server** (for example the AWS free tier VPS)
<br/>

- For it being lightweigth and simple to handle, I chose **Alpine Linux** for the local proxy
- Because of availability, I chose **Ubuntu** for the server
<br/>

- **Tunneling** the ports is done through **WireGuard**
- **Forwarding** the ports is done via **socat**
<br/>

## Install

- Check out the [Install Guide](INSTALL.md) for a step-by-step guide on how to set this up
<br/>

## Sample

- If you by any chance want to use this make a WireGuard endpoint on your local network accessible to the outside, you might want to checkout the [Sample Setup](sample/SAMPLE.md)
<br/>

## Troubleshooting

- If you're facing issues while installing, you might want to look at the [Troubleshooting Section](TROUBLESHOOTING.md)
- Should you still run into issues, feel free to [open a new Issue](https://github.com/klydra/wiretunnel/issues/new)
