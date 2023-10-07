## Troubleshooting

- Check the status of the tunnel on the [SERVER] using `sudo wg` to see if the [PROXY] shows up as a connected peer.
- Ping the [PROXY] from the [SERVER] using `ping 10.0.0.1` and the [SERVER] from the [PROXY] using `ping 10.0.0.2`, respectively, to see if the tunnel is working.
- Make sure your [PROXY] can reach the internet and your [SERVER].
- Make sure you've allowed traffic on your `<<< ENDPOINT PORT >>>` for **TCP and UDP** traffic in the firewall rules for your [PROXY] and [SERVER], respectively.
- Make sure your source machine is reachable by the [PROXY].
