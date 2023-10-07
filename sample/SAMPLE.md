## Introduction

I came up with this setup when I was looking for a way to access my home network wireguard entrypoint although I could not open the port with my new provisioned router.<br/>
As my home server runs TrueNAS Core, I would have preffered a way to run the home "proxy" under a FreeBSD jail, but sadly, WireGuard is tricky to get working in such an environment, especially when I can't use VNET but rather need to bind to a shared physical NIC.<br/>
After a few weeks of operation though, this solution has turned out to be just as good, if not even better.<br/>
An Alpine VM will do just fine, even with limited resources allocated and I've experienced no hickups so far.<br/><br/>
As mentioned, I'm using a AWS t2.micro VPS as part of the free tier, which works quite good for this usecase as well.<br/>
If you want to setup a system similar to this sample, just watch out that you need configure your VPC inbound / outbound rules correctly.<br/>
Ensure that you open the port for **both TCP and UDP traffic**, because WireGuard will connect just fine with only a TCP connection, but handshakes and data transfer will **probably not work or be degredaded** if UDP is unavailable!

## Usage

I've provided preconfigured files in this subfolder, so the process of following the [Install Guide](INSTALL.md) should be much more streamlined.<br/>
The sample is configured to use the port `51821` for the **outer [SERVER] to [PROXY] tunnel** and forwarding the port `51820` to the **inner local network entrypoint**.<br/>
I also strongly recommend setting up a **DNS A Record** that points to the public ip of your [SERVER] and using that as the `<<< ENDPOINT >>>` variable. WireGuard will handle it just fine, and should your public ip change, you just need to change one DNS record, and not update the values in the [PROXY] and all your clients by hand.