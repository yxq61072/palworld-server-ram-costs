# palworld dedicated server hosting: real RAM requirements, what managed hosts charge, and when a budget VPS is the smarter buy

Palworld dedicated server hosting comes in two shapes, and the difference matters more than any single price tag. Managed game hosts rent you a pre-installed server with a web panel, scheduled restarts and backups included. VPS providers such as RackNerd rent you raw hardware — same free server software, full root access, everything else on you. Both can run the same world for the same friends; they just split the work differently.

Before comparing either, the official specs do most of the filtering for you: Pocketpair recommends a 4-core CPU with 16 GB of RAM, SSD storage, and one open UDP port. Any plan that misses those numbers will disappoint, at any price.

> **Quick picks**
>
> - **Cheapest managed 16 GB:** HostedGG's Standard plan — 16 GB for **€14.99/mo**, dropping to €11.99/mo on yearly billing, with a 3-day free trial that needs no credit card.
> - **Managed, exact-spec:** Humbleservers' 16 GB plan at **$32/mo** matches Pocketpair's recommended specification directly and doesn't cap player slots.
> - **DIY on a VPS:** if someone in your group is comfortable in a terminal, the same free server software runs on a budget KVM VPS — which is where providers like RackNerd undercut the panel hosts. 👉 [see RackNerd's current KVM VPS plans and pricing](https://bit.ly/my-rn)

## Co-op, self-hosted, rented: what you're actually choosing between

Co-op holds 4 players and dies the moment the host logs off. A dedicated server holds 32 by default and runs whether you're online or not — and the server software itself costs nothing.

The in-game co-op mode uses a setting called `CoopPlayerMaxNum`, set to 4. Your world lives on the host's machine and their session: when they close the game, nobody can play and nothing at your bases progresses. The dedicated server software raises that to 32 players by default (`ServerPlayerMaxNum`), and it's a **free download on Steam** — you don't need a second copy of the game, and it runs on Windows or Linux.

So what does renting actually buy? The same software on somebody else's hardware: an always-on machine, an address your friends can reach, backups, and a panel. That's worth money the moment your group plays at different hours — which describes most groups. Self-hosting on a gaming PC, meanwhile, is a rough fit for this specific game, because the server process can consume 16–32 GB of RAM on its own while the game itself is running.

## The official requirements — and the spec no host quotes

Pocketpair's own documentation lists 4+ cores, 8 GB minimum RAM, 16 GB recommended, SSD storage, a 64-bit OS, and UDP port 8211. Here's the table straight from the official dedicated-server docs:

| Component | Official figure |
| --- | --- |
| CPU | 4 cores or more |
| RAM | 8 GB minimum — the docs flag out-of-memory crash risk at this level; **16 GB recommended**; more than 32 GB for large servers |
| Storage | SSD — the docs warn that slow storage *may corrupt saved data* |
| OS | Windows 64-bit or Linux 64-bit (Ubuntu, AlmaLinux and similar) |
| Network | UDP port 8211 by default, changeable |
| Player limit | 32 by default via the `-players=32` launch argument, adjustable |

Two things in those docs deserve more attention than they usually get. First, multithreading is an opt-in flag, and the official documentation notes that leaving it unset may improve performance in v1.0 and later — meaning single-thread CPU speed matters more than a big core count. Second, the storage warning is unusually blunt: this isn't a "loading might be slow" caveat, it's a save-corruption warning. Whatever you rent, confirm the storage is actually SSD or NVMe-backed.

And a third thing the docs deliberately don't contain: a RAM-per-player formula. There is no official one — any host quoting "1 GB per player" invented that number.

## What actually loads a Palworld server: bases, not headcount

Player count is what hosts sell on. Bases and working Pals are what actually push the hardware.

Out of the box, the settings file lets each guild run 4 base camps with 15 working Pals each — 60 continuously simulated Pals per guild before anyone touches a config. Pocketpair's own notes on the worker cap say raising it is the usual cause of base-camp lag. The practical consequence: a four-player server where everyone builds sprawling factories works the machine harder than a sixteen-player server where nobody builds anything. Size your plan for how your group builds.

RAM also drifts upward over time. A fresh install starts around 4–6 GB but can climb to 16–32 GB over a long session with several players and large bases — HostedGG describes this pattern on its own page and sells scheduled restarts as the fix. On a self-managed server, a cron job does the same thing for free.

On headcount: the engine officially allows 32, but Renzom caps its servers at 16 players and argues anything claiming more is overselling, citing TPS drops, pal pathfinding bugs and base desync past that point. You don't have to take one operator's word as gospel, but 16 is a reasonable planning number for a smooth experience, with 32 as the documented ceiling.

## What managed Palworld hosts charge right now

Entry-level managed plans run roughly €4–20 per month, with 16 GB tiers between about €15 and €40. The bigger differences are in slot pricing, trial policies and backup retention. These figures come from each host's own Palworld page:

| Host | Cheapest listed plan | ~16 GB tier | Slots | Trial / refund |
| --- | --- | --- | --- | --- |
| HostedGG | €4.49/mo (8 GB) | €14.99/mo | 1–4 / 4–16 / 16–32 by plan | 3-day free trial, no card |
| Humbleservers | $16/mo (8 GB) | $32/mo | unlimited | not stated |
| WinterNode | $11.94/mo (6 GB) | priced via configurator (sample quote $31.84/mo) | unlimited | 48-hour money-back on first server |
| Renzom | €19.92/mo (8 GB) | €39.84/mo | capped at 16 | monthly cancellation |
| Indifferent Broccoli | $5.99/mo (1 player) | n/a — plans claim uncapped RAM | pay by player count | 2-day trial + 7-day refund |
| GPORTAL | €4.40 for 3 days | — | per slot: 10 slots €13.70/30 days, 32 slots €28.00/30 days | not stated |

A few things worth pulling out of that table. Humbleservers is the only one naming its actual CPUs (Ryzen 7 7700 in Germany, EPYC 4244P in the USA), which matters given Palworld's single-thread appetite. WinterNode keeps automated backups twice daily for 45 days — the longest retention in this list — and its pricing scales through a RAM configurator rather than fixed tiers. GPORTAL's per-slot model gets expensive fast at 32 players. Prices move, so treat the table as a snapshot and confirm on the order page.

If those panel conveniences are what you want, any of the above will run the game. The interesting question is whether you need them at all.

## The VPS route: running Palworld yourself on RackNerd

RackNerd doesn't sell a Palworld panel. It sells budget KVM VPS plans with full root access — and that's precisely what the DIY route needs, provided someone in your group is willing to be the sysadmin.

What you get is a virtual machine that stays on 24/7, your choice of Linux distribution, and root over everything. RackNerd is best known for low-cost annual KVM VPS deals, with datacenter coverage weighted toward the US. What you then do yourself: install the free dedicated server through SteamCMD, open UDP 8211, tune `PalWorldSettings.ini`, set a scheduled restart to bleed off leaked memory, and script backups to somewhere off the box. None of it is hard; all of it is yours.

Use Pocketpair's official numbers as your filter when picking a plan:

| Official requirement | What to verify in the plan |
| --- | --- |
| 4+ cores | vCPU core count on the plan page |
| 16 GB recommended, 8 GB floor | RAM allocation — don't plan a group server below 16 GB |
| SSD storage | NVMe/SSD-backed storage, given the save-corruption warning |
| Linux 64-bit | pick Ubuntu or a similar supported distro at order time |
| UDP 8211 | root access means full firewall control |

Two honest caveats. Budget VPS lines don't always publish CPU clock speeds, and since Palworld leans on single-thread performance, it's worth checking what silicon sits behind a plan before ordering. And every convenience from the managed column — patch-day auto-updates, one-click restores, panel config editors — becomes your evening instead. RackNerd also rotates its promotions regularly, so judge by the current lineup rather than an old review. 👉 [browse RackNerd's VPS lineup and match a plan to the checklist](https://bit.ly/my-rn)

Who this suits: groups where one person enjoys tinkering, anyone optimizing for the lowest long-term cost, and players who want full file and config control for mods or experimental settings. Who it doesn't: the person who wants to press one button and go catch Pals. There's no shame in that — it's exactly what the managed premium pays for.

## Managed panel or VPS: which one fits your group

Pick managed if nobody wants to admin a server; pick a VPS if someone actively enjoys it and cost is the priority.

- **Your group plays at scattered hours, includes console players, and has no admin appetite** → a managed 16 GB plan. HostedGG is the cheapest verified option at €14.99/mo; Humbleservers matches the official spec exactly at $32/mo; WinterNode offers the longest backup retention.
- **Four to six friends who play together with light bases** → in-game co-op is free and holds 4 players. Don't rent anything yet.
- **Someone's comfortable with Linux and you'd rather pay VPS prices** → a 16 GB-class KVM VPS from RackNerd, set up per the checklist above.
- **Heavily modded or a large community** → the 20–32 GB tier on either route, and expect the official docs' ">32 GB for large servers" guidance to apply.

One buying-safety note: the managed hosts here have verified trials or refunds (HostedGG 3 days free, WinterNode 48-hour money-back, Indifferent Broccoli 2-day trial plus 7-day refund), which makes testing cheap. For the VPS route, check the refund terms on the order page before committing, and get your backup script running before you invite anyone — the official save-corruption warning is reason enough.

## Frequently asked questions

**How much RAM does a Palworld dedicated server need?**
16 GB is the standard answer. Pocketpair's docs list 8 GB as minimum but explicitly warn about out-of-memory crashes at that level, recommend 16 GB, and suggest more than 32 GB for large servers. Since RAM use grows with bases and time online rather than headcount, err high if your group builds big.

**How many players can join a Palworld dedicated server?**
32 by default, set through the `-players=32` launch argument and adjustable. In practice, operators report stability degrading past 16 players, so treat 16 as the comfortable target and 32 as the ceiling.

**Can friends on Xbox, PS5 and Mac join the same server?**
Yes. Palworld supports crossplay across Steam, Xbox (console and PC Game Pass), PlayStation 5 and Mac. Console players connect through the in-game server browser using an invite code; Steam and Mac players can also connect by direct IP. Managed hosts often register your server on the community list automatically; on a self-managed box you'll handle the relevant settings yourself.

**Can I move my co-op world to a dedicated server?**
Yes. Your world, bases and Pals live in save files that upload to a dedicated server. The one catch is the co-op host's character, which is tied to a local ID and may need a remapping step; every other player's character transfers normally.

**Do I need to buy Palworld again to run a server?**
No. The dedicated server software is a free download on Steam and doesn't require a second copy of the game.

**Which port does the server use?**
UDP 8211 by default, changeable through the `PublicPort` setting. Make sure whatever you host on — game host or VPS — leaves that port reachable.

## Before you click order

Confirm three things on any plan, from any provider: at least 16 GB of RAM, genuinely SSD/NVMe-backed storage, and a clear answer on where backups come from. Then it's a pure preference call. If you want the panel, pick from the managed table above based on trial terms and backup retention. If your group has a willing admin and you'd rather keep the monthly bill closer to pocket change, 👉 [check RackNerd's latest VPS deals before your group's next session](https://bit.ly/my-rn) — and remember that the server software you'll install on it is free.
