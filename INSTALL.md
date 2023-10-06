## Prepare Local Machine [PROXY]

1. Install Alpine Linux
2. Install required packages `apk add wireguard-tools-wg-quick socat`
3. Enable IP forwarding `sysctl -w net.ipv4.ip_forward=1`
4. Create the WireGuard configuration file `nano /etc/wireguard/wg0.conf` using the contents from [wg0.conf](proxy/wg0.conf)


## Prepare Server [SERVER]

1. Install Ubuntu
2. Install required packages `apt install wireguard socat`
3. Create the WireGuard configuration file `nano /etc/wireguard/wg0.conf` using the contents from [wg0.conf](server/wg0.conf)


## Generate Keys and Configure [PROXY & SERVER]

1. Generate one private and corresponding public key for the [PROXY] `wg genkey | tee private.key | wg pubkey > public.key`
2. Generate one private and corresponding public key for the [SERVER] `wg genkey | tee private.key | wg pubkey > public.key`
3. Note these down for later use in a secure location
4. Populate the fields `<<< [PROXY/SERVER] [PUBLIC/PRIVATE] KEY >>>` with the **generated keys** in the `/etc/wireguard/wg0.conf` files on the [PROXY] **and** [SERVER] respectively
5. Populate the field `<<< ENDPOINT >>>` with the **public ip of your [SERVER]** and the fields `<<< ENDPOINT PORT >>>` with a **one unused port of your choice** _(for example **51820**)_ in the `/etc/wireguard/wg0.conf` files on the [PROXY] and [SERVER] respectively

## (OPTIONAL) Test the tunnel [PROXY & SERVER]

1. Start the WireGuard tunnel on the [PROXY] and [SERVER] `wg-quick up wg0` respectively
2. Check the status of the tunnel on the [SERVER] using `sudo wg` to see, if the [PROXY] shows up as a connected peer
3. Ping the [PROXY] from the [SERVER] using `ping 10.0.0.1` and the [SERVER] from the [PROXY] using `ping 10.0.0.2` respectively to see, if the tunnel is working

## Enable persistence [PROXY]

1. Create a init.d service that ensures the WireGuard tunnel gets started at system startup at `nano /etc/wireguard/wg0.conf` using the contents from [wireguard-quick](proxy/wireguard-quick)
2. Enable the service using `rc-update add wireguard-quick`
3. Start the service using `rc-service wireguard-quick start`

## Enable persistence [SERVER]

1. Enable the service using `systemctl enable wg-quick@wg0`
2. Start the service using `systemctl start wg-quick@wg0`

## (REPEATABLE) Forwarding a port [PROXY & SERVER]

1. Make sure you have the required information :
	- `<<< FORWARD NAME >>>`, the **name** for this port forwarding "rule"
	- `<<< FORWARD PORT >>>`, the **port** that is **free and reachable** on the [SERVER] from the outside
	- `<<< SOURCE HOST >>>`, the **hostname** or **ip** of the machine on your local network that should be forwarded
	- `<<< SOURCE PORT >>>`, the **port** of the machine on your local network that should be forwarded
	- `<<< PROTOCOL >>>`, the **protocol** that should be used to forward the traffic (typically **TCP or UDP**) (TIP: The same port can be forwarded twice with **TCP and UDP** respectively, should both protocols be needed)

2. Create the forwarder service on the [PROXY] at `nano /etc/init.d/socat-forwarder-<<< FORWARD NAME >>>` using the contents from [socat-forwarder-base](proxy/socat-forwarder-base)
3. Populate the variables defined previously in the file
4. Make the service executable using `chmod +x /etc/init.d/socat-forwarder-<<< FORWARD NAME >>>`
5. Enable the service using `rc-update add socat-forwarder-<<< FORWARD NAME >>>`
6. Start the service using `rc-service socat-forwarder-<<< FORWARD NAME >>> start`

7. Create the forwarder service on the [SERVER] at `/etc/systemd/system/socat-forwarder-<<< FORWARD NAME >>>.service` using the contents from [socat-forwarder-base.service](server/socat-forwarder-base.service)
8. Populate the variables defined previously in the file
9. Enable the service using `systemctl enable socat-forwarder-<<< FORWARD NAME >>>`
10. Start the service using `systemctl start socat-forwarder-<<< FORWARD NAME >>>`