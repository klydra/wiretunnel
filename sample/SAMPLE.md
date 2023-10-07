## Introduction

I developed this setup when I was seeking a way to access my home network's Wireguard entrypoint, even though I couldn't open the port with my newly provisioned router.<br/>
Since my home server runs TrueNAS Core, I would have preferred to run the home "proxy" within a FreeBSD jail. Unfortunately, WireGuard is tricky to get working in such an environment, especially when I can't use VNET but need to bind to a shared physical NIC.<br/>
After a few weeks of operation, however, this solution has proven to be just as good, if not better.<br/>
An Alpine VM will do just fine, even with limited allocated resources, and I've experienced no hiccups so far.<br/><br/>
As mentioned, I'm using an **AWS t2.micro VPS** as part of the **free tier**, which works quite well for this use case as well.<br/>
If you want to set up a system similar to this sample, just be sure to configure your **VPC inbound and outbound rules** correctly.<br/>
Ensure that you open the port for **both TCP and UDP traffic** because WireGuard will connect just fine with only a TCP connection, but handshakes and data transfer will **probably not work or be degraded** if UDP is unavailable!<br/><br/>

## Usage

I've provided preconfigured files in this subfolder, so the process of following the [Install Guide](INSTALL.md) should be much more streamlined.<br/>
The sample is configured to use the port `51821` for the **outer [SERVER] to [PROXY] tunnel** and forwarding the port `51820` to the **inner local network entrypoint**.<br/>
I also strongly recommend setting up a **DNS A Record** that points to the public IP of your [SERVER] and using that as the `<<< ENDPOINT >>>` variable. WireGuard will handle it just fine, and should your public IP change, you just need to change one DNS record, and not update the values in the [PROXY] and all your clients by hand