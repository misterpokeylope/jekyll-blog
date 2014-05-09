---
layout: post
title: Configuring iptables
date: 2014-5-9
categories: posts linux 
---

With the recent fixing of my "network issues", I have been putting more time
and effort in to making sure everything, from the website to the Raspberry Pi,
is cleaned out after it's Winter hibernation. Upon reading some of journald's
log files (which I'd noticed grew somewhat quickly), I discovered a flood of
failed remote login attempts, clustered at different times of the day, and all
attempting to login from high numbered ports, with users that don't exist on the
system. A quick `whois` lookup on the IPs revealed that they're locations from 
all over the world, so most likely bots looking for easy access to insecure 
systems.

Before the downtime I had not bothered to setup any sort of protection on the
Pi since I figured that my domain was still relatively new and unknown. While
it's still pretty unkown, I have it posted in public places now, and have it
indexed by Google, so this most likely allows intrepid spam-bots to find my
domain relatively easily. While programs like [fail2ban][fail2ban] or disabling
ssh all together could have remedied the problem, I went for a much less
passive approach, and decided to set up a firewall.

Linux has plenty of software firewall solutions, but naturally I decided to go 
with [iptables][iptables], the Linux kernel's built-in firewall. I'm fully
aware of [nftables][nftables], the sucessor to iptables, but since Arch Linux
ARM currently has the 3.10 Kernel as it's "stable" Kernel version this option 
is not available to me yet. I plan on using it however, once it fully 
replaces iptables in the kernel.

While it's a powerful tool, iptables is not the easiest to setup for those
inexperienced with firewalls, like me. Fortunately the fantastic Arch Wiki has
[an entire article for setting up a stateful firewall][ipwiki] with iptables. 

Admittedly it didn't go as smoothly as I thought it would, entirely due to
PEBKAC issues, and setting it up over an ssh connection. As one could
imagine setting the `INPUT DROP` policy before configuring the SSH
related chain will quickly lead to an unproductive session, especially when one
doesn't have immediate physical access to the Raspberry Pi.

Luckily for me the article in question has a helpful explination of what each
command did at each step, so at least I wouldn't feel too bad about the fact 
I mainly copy and pasted the commands that I felt I needed. While I still don't
know a significant amount about iptables yet, I feel I now at least have a 
basic understanding, that suits a machine that only servers out a simple 
webserver.

For those interested, I threw together a quick bash script that more or less 
matches the rules and chains I have in place. This must be run as root, so 
_use with caution_.

```bash
#!/usr/bin/bash

# User defined chains
echo "Setting user defined Chains"
iptables -N TCP
iptables -N UDP
echo "Done"

# Set default policies
echo "Setting Policies"
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
echo "Done"

# Set rules
echo "Setting rules"
iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
iptables -A INPUT -p icmp --icmp-type 8 -m conntrack --ctstate NEW -j ACCEPT
echo "Done"

# Attach TCP/UDP chains to INPUT
echo "Attaching TCP/UDP chains to INPUT"
iptables -A INPUT -p udp -m conntrack --ctstate NEW -j UDP
iptables -A INPUT -p tcp --syn -m conntrack --ctstate NEW -j TCP
echo "Done"

# Open ports for services
echo "Opening ports for HTTP, HTTPS, DNS, and SSH"
iptables -A TCP -p tcp --dport 80 -j ACCEPT
iptables -A TCP -p tcp --dport 443 -j ACCEPT
iptables -A UDP -p udp --dport 53 -j ACCEPT
iptables -A TCP -p tcp --dport 22 -j ACCEPT
echo "Done"

# Reject everything else
iptables -P INPUT DROP

# Save Settings
echo "Saving settings to /etc/iptables/iptables.rules"
iptables-save /etc/iptables/iptables.rules
echo "Done"
```

Of course, there is more I can do, such as disabling logins for SSH, and only
allowing access via SSH keys. That being said, there's nothing particularly
interesting/useful/valuable on my Raspberry Pi in the first place.

[fail2ban]: http://www.fail2ban.org/wiki/index.php/Main_Page
[iptables]: http://en.wikipedia.org/wiki/Iptables
[nftables]: https://wiki.archlinux.org/index.php/Nftables
[ipwiki]: https://wiki.archlinux.org/index.php/Simple_stateful_firewall
