# Bare Metal Server Guide: What You Get, When It Beats a VM, and Which Configuration to Order (Full Spec Table, Bandwidth and Contract Details)

A bare metal server is one entire physical machine rented to a single customer. No hypervisor is preinstalled, the operating system runs straight on the hardware, and every core, gigabyte of RAM and IOPS belongs to you. That buys predictable disk and network throughput, hardware-level isolation and control down to BIOS and firmware settings. It costs you elasticity: the machine has to be built, which takes anywhere from minutes to hours, and growth means adding or swapping physical components.

This guide covers which workloads justify the spend, how bare metal actually differs from a virtual machine and from the older dedicated-server model, the configurations Spin Servers currently lists in Dallas and San Jose, and the questions worth settling before you pay an invoice.

**Keep reading if** you run databases, game servers, AI or GPU jobs, or anything that arrives with a compliance audit attached, and you can plan capacity more than a week ahead. **Skim to the VM section and leave** if your load is small and spiky, or if nobody on the team wants to own an operating system patch cycle.

> **Quick pick:** the San Jose 16-core / 256 GB build with 2 × 1.6 TB SSD on a hardware RAID controller is the one worth pricing first if you're consolidating several VMs or running an in-memory database. Every San Jose configuration ships with dual 10 Gbps ports and 30 TB of monthly transfer, and is marked for instant provisioning on the configuration page.
>
> 👉 [See the full Spin Servers bare metal configuration list](https://bit.ly/Spinservers)

## What Spin Servers currently lists

Two locations: Dallas, Texas and San Jose, California. Third-party write-ups consistently describe Dallas as the cheaper of the two, with San Jose carrying a latency advantage for West Coast and APAC traffic. Configurations split along an obvious axis once you read them closely. Below roughly 64 GB of RAM the plans are general-purpose; above that they're built for people who need addressable memory more than they need clock speed, which is the opposite of how most cloud instances are priced.

The full current list, as published:

| Configuration (location) | Cores / threads | RAM | Storage | Order |
| --- | --- | --- | --- | --- |
| Dallas · Intel Xeon E3-1280 v5, 3.7 GHz turbo | 4C / 8T | 32 GB DDR4 ECC | 1 TB M.2 NVMe | [ Check this build](https://bit.ly/Spinservers) |
| San Jose · 2.6 GHz base, 3.1 GHz turbo | 12C / 24T | 64 GB DDR3 ECC | 1 × 240 GB SSD | [ Check this build](https://bit.ly/Spinservers) |
| San Jose · 3.7 GHz base, 4.0 GHz turbo | 4C / 8T | 32 GB DDR4 ECC | 1 × 1 TB M.2 NVMe | [ Check this build](https://bit.ly/Spinservers) |
| San Jose · 1.8 GHz base, 2.9 GHz turbo | 16C / 32T | 64 GB DDR4 ECC | 1 × 1.6 TB SSD | [ Check this build](https://bit.ly/Spinservers) |
| San Jose · 1.8 GHz base, 2.9 GHz turbo | 16C / 32T | 64 GB DDR4 ECC | 1 × 1.6 TB NVMe SSD | [ Check this build](https://bit.ly/Spinservers) |
| San Jose · 1.8 GHz base, 2.5 GHz turbo | 24C / 48T | 64 GB DDR4 ECC | 1 × 1.6 TB SSD | [ Check this build](https://bit.ly/Spinservers) |
| San Jose · 1.8 GHz base, 2.9 GHz turbo | 16C / 32T | 256 GB DDR4 ECC | 2 × 1.6 TB SSD, hardware RAID with cache | [ Check this build](https://bit.ly/Spinservers) |
| San Jose · 2.1 GHz base, 3.0 GHz turbo | 32C / 64T | 512 GB DDR4 ECC | 2 × 3.84 TB SSD, hardware RAID with battery-backed cache | [ Check this build](https://bit.ly/Spinservers) |

Networking and provisioning, which the spec table above flattens:

- San Jose configurations list **2 × 10 Gbps ports**; the Dallas build lists a single 10 Gbps port. Both quote **30 TB of monthly transfer**.
- One IPv4 address is included on most builds; IPv6 allocation is reported as a /64 by a third-party review rather than on the configuration page, so confirm it at order time if you need it.
- Instant provisioning is flagged on the San Jose stock; the Dallas E3-1280 v5 is marked "Ready To Go - Auto-Provisions." Custom builds that aren't in stock take longer, which is normal for this tier.
- Remote management over IPMI or iKVM is reported by two independent sources, and matters more than it sounds: without out-of-band console access, a botched kernel upgrade means a support ticket instead of a five-minute fix.

## What it costs, and how the billing works

Pricing is attached to the individual configuration at checkout rather than published as a flat rate card, and it moves with location, hardware and stock. Third-party roundups published in late 2025 and 2026 quote the Dallas 4-core / 32 GB build at $79 per month and a dual-Xeon 16-core / 256 GB build at $149 per month. Those figures are a useful anchor, not a quotation, so confirm the number on the order page before you commit.

Billing is month-to-month with no setup fee, per the same write-ups. The trade-off is documented too: iTechGuides, reviewing the service in October 2026, notes that month-to-month plans generally carry no refund policy. Plan the first month as a paid trial, and don't prepay a year to chase a discount that hasn't been published.

👉 [Compare Spin Servers configurations and confirm current pricing](https://bit.ly/Spinservers)

## Bare metal vs. virtual machine vs. traditional dedicated server

The three get conflated constantly, and the differences that matter aren't the ones in the marketing copy.

|  | Bare metal (cloud-delivered) | Virtual machine | Traditional dedicated server |
| --- | --- | --- | --- |
| **Resource allocation** | 100% of the machine to one tenant | Shared slices of a host | 100% of the machine to one tenant |
| **Performance consistency** | Predictable, no hypervisor overhead | Varies with neighbour activity | Predictable, but often on older hardware |
| **Control level** | Root, BIOS, firmware, kernel | Guest OS settings only | Root, sometimes limited by the provider |
| **Isolation** | Single-tenant hardware | Multi-tenant environment | Single-tenant hardware |
| **Provisioning** | Minutes to hours | Seconds to minutes from a template | Days to weeks |
| **Scaling** | Vertical: add or swap hardware, or automate via API | Elastic, horizontal and vertical | Vertical, manual, often slow |
| **Billing** | Commonly hourly or monthly | Hourly or monthly | Monthly or annual contracts |

The bare metal versus dedicated distinction is the one most buying guides skip. The hardware is similar; the delivery model is not. Dedicated servers historically meant a long provisioning wait, month or year contracts and frequently low-end or aged components. Bare metal providers sell dedicated hardware through something close to a cloud model: provisioning in minutes, billing by the hour at many providers, and a hardware range that runs from cheap to top-of-the-line including GPUs. If two quotes for "a dedicated server" differ by a factor of three, the gap is usually the delivery model and the generation of the silicon, not the CPU badge.

There's a second layer worth knowing. Modern bare metal platforms let you orchestrate a group of servers with private networking and storage attached, rather than handing you a set of isolated boxes you have to wire together yourself. Spin Servers' private Layer 2 networking between servers, reported as a no-cost feature by both iTechGuides and independent write-ups, is that idea in miniature: two machines on the same private segment at 10 Gbps without buying a switch.

**No hypervisor is the default, not a law.** You can install your own Type 1 hypervisor, run Proxmox or ESXi, or build a Kubernetes cluster on the box. Some providers' bare metal cloud products even keep a thin virtualization or container layer for their own maintenance. So if your reason for buying is "I want every last percent of performance," check whether the build ships with anything running underneath your OS.

👉 [Check which install images Spin Servers supports, including ESXi and Windows](https://bit.ly/Spinservers)

## Workloads that justify the premium

- **Databases and analytics.** Stable IOPS is the whole point. A hypervisor adds a scheduling layer between your queries and the NVMe drives, and neighbour activity adds variance you can't tune away.
- **HPC, big data and financial workloads.** These want the full memory bandwidth and cache of a physical socket, not a slice of it.
- **Game servers, VoIP, video and AR/VR.** Low, consistent latency to the player or caller is the product. Noisy-neighbour jitter shows up as complaints.
- **AI and GPU work.** Training and inference need direct access to GPUs, high memory bandwidth and fast NICs, and bare metal lets you tune GPU, storage and network configuration to the job rather than to a template.
- **Steady-state enterprise systems.** ERP, CRM and supply chain platforms with predictable resource demand are a poor fit for elastic pricing and a good fit for a fixed monthly box.
- **Regulated environments.** HIPAA, PCI-DSS and FedRAMP work gets easier when the hardware is single-tenant: audits are simpler, and custom firewalls or HSMs are yours to place.

The isolation argument is stronger than "we don't like sharing." Hypervisors provide weaker separation than separate machines, attackers keep finding vulnerabilities in the isolation layer itself, and shared hardware is exposed to Rowhammer-class and speculative-execution flaws. Covert channels are impractical to close without physical separation. Server cost has also fallen as a share of total ownership cost relative to the labour of managing it, which makes the old answer of throwing hardware at the problem affordable again.

Compliance doesn't come free with the hardware, though. Spin Servers advertises PCI-compliant facilities; the compliance work, scanning and documentation remain yours. Buyers who want managed PCI support should look at a managed host instead.

👉 [See the single-tenant configurations available now](https://bit.ly/Spinservers)

## When bare metal is the wrong purchase

- **Small, spiky load.** A blog, a staging environment, a SaaS product before product-market fit. A VM at a fraction of the cost handles it, and you can resize it tonight.
- **You need elasticity inside the hour.** Bare metal scales vertically: more RAM, more drives, a bigger CPU, one physical change at a time. Capacity planning has to be done against peak, not average, and you should leave headroom you're paying for.
- **Nobody wants to run an OS.** Kernel patching, backups, monitoring, TLS, the whole stack is yours. A PaaS or a managed container platform removes that work, and for a two-person team the saved hours are usually worth more than the performance delta.
- **Budget is the binding constraint and you can wait.** Older dedicated-server providers still undercut bare metal on price if a two-week delivery window and a yearly contract are acceptable. You're buying hardware generation and delivery speed, and you can decline both.

## Six questions to settle before you order

1. **How long until the machine is online?** "Instant" applies to stock configurations only; anything customized has a build queue, and your migration plan depends on which side you're on.
2. **What's the billing granularity and the minimum term?** Hourly billing lets you test and walk away; month-to-month with no refund means the first invoice is your trial.
3. **What happens after 30 TB?** Ask for the overage rate or the throttle policy in writing. Bandwidth overage is the line item that turns a cheap server into an expensive month.
4. **How many IPv4 addresses, and is DDoS mitigation included?** Addresses are scarce and mitigation is sometimes a paid add-on; both change the real monthly cost.
5. **Is IPMI or iKVM access included?** Without out-of-band console access you cannot reinstall an OS or fix a broken bootloader without opening a ticket.
6. **What's the hardware replacement window, and can I upgrade later?** A failed DIMM at 2 a.m. is a business event; a published replacement SLA and a clear path to add RAM or disks later both belong in the decision.

## FAQ

### Is a bare metal server the same as a dedicated server?

Not quite. A bare metal server is a dedicated machine, but the term usually implies cloud-style delivery: fast provisioning, hourly or monthly billing, and a hardware range that goes up to GPU-equipped top-tier builds. "Dedicated server" often still means a longer setup and a contract.

### Does bare metal have a hypervisor?

Not by default. That's the definition. You can install one yourself, and some providers keep a virtualization or container layer in their bare metal cloud products, so confirm what sits beneath your OS if maximum performance is the reason you're buying.

### Can I install Windows or ESXi?

Yes. Spin Servers lists Linux, Windows and ESXi auto-install options alongside remote management, so you can pick the OS at provisioning rather than filing a ticket.

### Is hourly billing more expensive than monthly?

Per-hour rates usually look higher than the monthly equivalent, and that's expected: you're paying for the option to leave. If you'll keep the machine for months, ask whether a monthly rate exists; if you're testing, hourly is what you want and the premium is the price of an exit.

### Can I run Kubernetes on a bare metal server?

Yes, and it's a common use. A physical node gives the cluster predictable CPU and disk throughput, and private Layer 2 networking between nodes removes the overlay overhead you'd otherwise pay for.

### Does single-tenant hardware make me compliant?

It removes one category of risk, the shared-infrastructure kind. It does not produce audit evidence, scanning or documentation. Budget for the compliance work separately from the server.

## How to decide

If you're consolidating several VMs, running an in-memory database, or need more than a few terabytes of fast local storage, go straight to the 256 GB and 512 GB San Jose builds; the RAM is the reason those configurations exist and the cores are secondary. If you're hosting game servers, a VPN fleet or a handful of high-traffic sites, the 4-core and 12-core builds at 10 Gbps are the sensible entry point, and anything larger is money you won't recover. If latency to the West Coast matters, pay the San Jose premium; if it doesn't, Dallas is the cheaper room.

Before the card comes out, confirm two things on the order page: the actual monthly figure for the exact configuration you picked, and whether the out-of-band access and DDoS protection you're assuming are included or billed separately. Those two answers decide whether the invoice matches the plan.

👉 [Start with the Spin Servers configuration list and price the build you need](https://bit.ly/Spinservers)
