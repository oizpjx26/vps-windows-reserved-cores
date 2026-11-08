# Dedicated VPS Windows: What Reserved Cores Cost, How the Licence Is Billed, and How Many RDP Users You Get

A dedicated Windows VPS is a virtual machine with CPU cores and RAM reserved for your workload, and its monthly invoice has three parts: the reserved resources, the Windows licence, and every RDP user past the first two. Skip the second and third when you compare plans and a $19 server turns into a $70 one at checkout.

SpinServers sits at the far end of "dedicated". It rents single-tenant bare-metal machines in Dallas and San Jose with Windows, Linux or ESXi installed by an automated provisioner, starting at $79 a month for a 4-core Xeon with 32 GB RAM, 1 TB NVMe and 30 TB of traffic on dual 10 Gbps ports. That is not a slice of a host with cores set aside for you. It is the whole box, which changes what you pay and what happens when a fan or a DIMM dies.

What follows is the sizing decision (shared versus reserved), the licence arithmetic, the per-user RDP maths, and the configurations currently published for SpinServers.

> **Quick pick.** If one Windows application has to run on hardware nobody else touches, the Xeon E3-1280 v5 tier at $79/mo covers most single-app deployments: 4 cores, 32 GB RAM, 1 TB NVMe, 30 TB of transfer. 👉 [See the current SpinServers Dallas and San Jose configurations](https://bit.ly/Spinservers).

## SpinServers plans on the price list

SpinServers' catalogue is built around dedicated hardware, and the published tiers scale from a single-socket 4-core machine to a quad-socket 768 GB box. Prices below are the starting figures listed for these configurations as fact-checked in September 2026. SpinServers prices vary by location, hardware and stock, so the configurator total is the number that counts.

| Configuration | CPU / RAM | Storage | Bandwidth | Price (monthly) | Order |
| --- | --- | --- | --- | --- | --- |
| Xeon E3-1280 v5 | 4 cores / 32 GB | 1 TB NVMe | 30 TB, 2×10 Gbps | $79 | [Configure](https://bit.ly/Spinservers) |
| Starter Package | Xeon E5 / 32 GB DDR4 | 1 TB M.2 NVMe | 10 TB, 10 Gbps | $99 | [Configure](https://bit.ly/Spinservers) |
| Growth Package | 2× Xeon E5-2630L v3 / 256 GB | 2× 1.6 TB SSD, RAID-1 | 20 TB, 10 Gbps | $149 | [Configure](https://bit.ly/Spinservers) |
| High Performance | 4× Xeon E5-4640 v2 / 768 GB | 8× 2 TB SSD, RAID-10 | 40 TB, 10 Gbps | $679 | [Configure](https://bit.ly/Spinservers) |

Three things sit outside those prices. Windows is offered as an auto-install option alongside Linux and ESXi, but the published configurations do not itemise a licence line, so ask whether the quote includes Windows Server or you bring your own key. Remote management runs through IPMI or iKVM, which matters the day a firewall rule locks you out of RDP. And month-to-month service generally carries no refund policy, so the first invoice is not a trial.

Deal listings for the same brand show smaller VPS-style configurations at promotional rates that change month to month, which is why two sources can quote $49 and $79 for what looks like the same class of machine. Treat any figure you find outside the configurator as a starting point and confirm the renewal price before you commit.

## Shared cores versus reserved cores: when the extra money buys nothing

On a shared VPS, a neighbour's backup job or a compile can take a few seconds of CPU away from you. For a remote desktop running an accounting package, or a small IIS site, you will never notice. For SQL Server, IIS under constant load, three or more people working over RDP at once, or an emulator and SEO farm pinning cores at 100% for hours, those same few seconds surface as slow queries, dropped requests and sessions that stutter mid-click.

That is the entire case for paying more. Reserved cores and RAM remove the contention; they do not make the CPU faster. A 4-core reserved plan and a 4-core shared plan run the same silicon.

So the rule is unglamorous. A single remote desktop, a brochure site, a bit of accounting software or a handful of bots: buy the cheapest shared plan and put the savings into RAM. A database that other people query, a site that gets traffic while you sleep, a team of three or more over RDP, or software that runs flat out for hours: pay for reserved cores, because the alternative is explaining to your users why the screen froze.

## The trade-off nobody mentions: reserved VM or entire machine

A Windows dedicated VPS is still a virtual machine, and that is a feature rather than a compromise. When a physical node fails, the cluster restarts your server on another node within seconds. A single bare-metal box cannot do that; it waits for a technician.

Bare metal buys you something else. No hypervisor layer, no noisy neighbour at all, your own kernel-level tuning, and full control of the storage layout. SpinServers pairs that with a DDoS-protected, multihomed network, private Layer 2 networking between your own servers, and dedicated IPv4 plus an IPv6 /64 block, which is the sort of thing you want when the machine is one of several rather than the only one.

Pick the reserved VM when uptime matters more than raw control and you want the provider to move you off failing hardware automatically. Pick bare metal when you need the whole machine, custom partitioning, or a workload that a hypervisor only gets in the way of.

## Windows licensing: three ways it lands on your invoice

This is where Windows hosting quotes stop being comparable, because providers handle the licence in three different ways.

**The 180-day evaluation.** Many Windows Server templates, 2016 through 2025, ship as Microsoft's evaluation build at no licence cost. It has to be activated online within the first 10 days, and after day 180 the server shuts down every hour until it is licensed. Fine for a migration test, unacceptable for anything customer-facing.

**A per-core licence add-on.** The common commercial model charges per vCPU per month, shown as its own line in the cart. One provider publishes $6.31 per vCPU per month, which comes to $12.62 for a 2-vCPU plan and $50.48 for an 8-vCPU plan. A licence priced per core behaves very differently from a flat fee when you scale up: doubling cores doubles the licence.

**Bring your own key on desktop Windows.** Windows 10 Pro and Windows 11 Pro templates install unactivated and you enter your own product key after first login, because hosting providers cannot rent desktop Windows licences. Desktop editions also allow a single interactive session, so they are not a cheap route to multi-user RDP. Windows 10 reached end of support on 14 October 2025 and no longer receives security updates, so new builds belong on Windows 11 Pro or Windows Server. Windows 11 Pro needs at least 2 GB of RAM, which pushes the smallest usable configuration up a tier.

Watch the wording when you compare vendors. One provider states that a VPS with any operating system costs the same, with no Windows premium, while another lists the licence separately at a per-core rate. Both can be true: the first is talking about the base resource price, the second about the Microsoft licence fee. Read the cart, not the headline.

Here is why it matters. Take an 8-vCPU reserved plan with the licence added and three extra RDP users, at the rates cited above: $50.48 for the licence plus $26.70 for the seats comes to $77.18 a month before you have paid for a single core of CPU. 👉 [Check what the SpinServers configuration you want costs with Windows installed](https://bit.ly/Spinservers).

## RDP users: the second hidden line

Every Windows Server install ships with remote desktop enabled and two administrator sessions. Two people can work at the same time; a third needs a paid seat. The published add-on rate in this market is $8.90 per user per month, so the arithmetic runs like this:

| People working concurrently | Extra seats needed | Monthly add-on |
| --- | --- | --- |
| 2 | 0 | $0 |
| 3 | 1 | $8.90 |
| 5 | 3 | $26.70 |
| 10 | 8 | $71.20 |

RAM usually runs out before the seat count does. Each interactive session carries its own browser, mail client and line-of-business app, and the per-user RAM question is the one that decides whether a 16 GB machine serves five people or eight.

Keep an out-of-band console in reach. SpinServers provides IPMI or iKVM access; other hosts offer a KVM console in the control panel. Either one is how you get back in after a bad firewall rule or a botched RDP setting, and it is worth testing on day one rather than during an outage.

## How other providers handle the same job

A short cross-check, since Windows pricing models differ enough to make one provider's cheap plan and another's look unrelated. All figures are the vendors' own published rates.

**Kamatera** guarantees 99.95% uptime on Intel Xeon Gold/Ice Lake processors at 2.7 GHz and above, with resources you can scale up or down at any time and 24/7 human support. Its 30-day trial allows one server configured up to $100 of cost plus 1 TB of traffic in and out, requires a valid card, and charges nothing if you stay inside those limits and terminate before the trial ends.

**Contabo** advertises 99.9% uptime and preinstalled Windows Server across its line. Its CLOUD VPS 6 with 8 vCPU and 24 GB RAM is $29.40 a month, or $27.60 for the first 24 months; CLOUD VPS 12 with 12 vCPU and 48 GB is $93.60, or $87.60 for the first 24 months. Those introductory prices revert, so budget for the standard rate.

**BareServer** lists Windows Server 2016 through 2025 on dedicated hardware with automatic deployment within 40 minutes and a choice of managed or unmanaged support.

**MassiveGRID** separates its Windows VPS on shared vCPU from a reserved-core Windows Dedicated VPS, from $3.99 and $18.84 a month respectively, and publishes the licence and RDP add-on rates quoted above.

None of these is the subject of this article; they are here so you can see how the same three cost components get packaged differently.

## FAQ

**Is a Windows dedicated VPS a physical server?**
No. It is a virtual machine with reserved cores and RAM. The upside is automatic restart on another node within seconds when hardware fails; the downside is that you are still sharing a hypervisor with other tenants, just not the CPU time.

**What happens when the 180-day evaluation runs out?**
The server shuts down every hour until a licence is applied. Activate the evaluation within the first 10 days of install so the clock starts cleanly, and decide on licensing before you migrate anything real onto it.

**Can two people share the default RDP sessions?**
Yes, two concurrent administrator sessions are included. A third simultaneous user needs a paid seat at the published per-user rate, and each extra user also adds RAM pressure.

**Can I ask the host to rent me desktop Windows?**
No. Microsoft does not let providers rent desktop Windows licences, which is why Windows 10 and 11 templates install unactivated and run on your own key, with a single interactive session.

**Should I still deploy Windows 10?**
Not on new hardware. Support ended on 14 October 2025 and security updates stopped. Use Windows 11 Pro if a desktop-tested application demands it, or Windows Server otherwise.

## Which one to buy

Match the workload, not the spec sheet.

- One remote desktop, a small IIS site, accounting software, a few bots: the cheapest shared plan that has enough RAM. Reserved cores are wasted money here.
- SQL Server, IIS under constant load, three or more people over RDP, emulator or SEO farms: reserved cores and RAM, every time.
- Production Windows with SQL and nobody on staff to patch it: a fully managed tier rather than a self-managed server.
- Full control, custom storage, several machines talking over a private network: bare metal, which is what SpinServers sells, with IPMI access and 10 Gbps ports on the upper tiers.

Before you pay, confirm three things in writing: whether the Windows licence is inside the quoted price or billed per core on top, the per-user rate for RDP seats past the first two, and the renewal price with the billing term. On month-to-month terms with no refund policy, those three answers are your entire safety net.

👉 [Configure a SpinServers Windows server for your workload](https://bit.ly/Spinservers).
