# Build configurations

Three ways to build the system in [README.md](README.md). Prices were checked in early October 2026 and move often. Anything marked **unverified** could not be confirmed from a manufacturer or retailer page.

## Summary

| | Build 1: all-in-one UPS | Build 2: component system | Build 3: ungrounded site |
|---|---|---|---|
| Core | BLUETTI Apex 300 | WattCycle 48 V 100 Ah + EG4 3000EHV-48 | Same as build 2, plus Mean Well NPB-1200-48 charger |
| Hardware cost | **~$1,800** | **~$2,650–2,800** (~$1,900 without panels) | **~$3,050** (~$2,300 without panels) |
| Continuous AC | 3.84 kW | 3.0 kW | 3.0 kW |
| Battery | 2.76 kWh | 5.12 kWh | 5.12 kWh |
| Solar | 810 W (2 × 405 W) | ~2.0 kW (5 × 405 W, series) | ~2.0 kW (5 × 405 W, series) |
| Mains loss behavior | 0 ms claimed (vendor) | Load already on inverter in SBU mode; 10 ms transfer only when the unit falls back to grid bypass | Nothing switches. Mains only charges the battery |
| Grounding needed | Grounded receptacle (mains passes through) | Grounded receptacle (mains passes through on bypass) | None from mains. Local bonding plus GFCI on the inverter output |

**Read this first.** If the receptacle tester says OPEN GROUND, builds 1 and 2 aren't suitable as they stand, because both pass mains straight to the PC in some modes. **The cleanest fix for every build is to have a licensed electrician add a real equipment-grounding conductor (NEC 250.130(C) allows retrofitting one) or run a new grounded circuit.** That costs less than the extra hardware in build 3, and then build 1 or 2 works as designed.

**Hold-up time.** An ATX PSU usually rides through about 16–17 ms at full load (**unverified** for your specific PSU). Transfer times of 10 ms or less are safe. 20 ms is marginal at 1.2 kW. Only build 3 has no transfer event at all.

---

## Build 1: all-in-one UPS, grounded site

### Candidates

| Unit | Capacity | Continuous AC | Solar input | UPS switchover | Price | Notes |
|---|---|---|---|---|---|---|
| **BLUETTI Apex 300** | 2,764 Wh LFP | 3,840 W | 2,400 W, 2 ports × 12–60 V, 20 A each | **0 ms** (vendor claim) | **$1,499** ([Outbound Power](https://outboundpower.com/products/bluetti-apex-300-portable-power-station-2764-8wh-3840w-2400w-solar-input)) | 6,000+ cycles, 5-yr warranty. Expands with B300K packs (2,764 Wh each, up to six). UPS marketed as a feature ([ESS News](https://www.ess-news.com/2025/05/19/bluetti-apex-300-with-2-7-kwh-battery-pricing-now-available-ahead-of-launch/)) |
| EcoFlow DELTA Pro 3 | 4,096 Wh LFP | 4,000 W | 2,600 W: HV 30–150 V/15 A/1,600 W + LV 11–60 V/20 A/1,000 W ([BackupPowerHub](https://backuppowerhub.com/ecoflow-delta-pro-3-review/)) | 10 ms per one review ([EnergySavvyHomes](https://www.energysavvyhomes.com/ecoflow-delta-pro-3-review/)); "sub-30 ms" per another ([BackupPowerHub](https://backuppowerhub.com/ecoflow-ups-mode-transfer-times/)). EcoFlow's own page gives no number (**unverified**) | $1,999 ([Off Grid Stores](https://offgridstores.com/products/ecoflow-delta-pro-3-portable-power-station)); $2,799 at [EcoFlow](https://us.ecoflow.com/products/delta-pro-3-portable-power-station) | Best solar input (150 V port). Over budget once panels are added |
| Anker SOLIX F3800 | 3,840 Wh LFP | 6,000 W (240 V split) | 2,400 W: 11–32 V/10 A + 32–60 V/25 A | 20 ms ([Outbound Power](https://outboundpower.com/products/anker-solix-f3800-portable-power-station-3840wh-6000w-2400w-solar-input)) | $1,980 (same source) | UPS only on three 120 V outlets. 1,800 W AC charging. 20 ms is marginal for a 1.2 kW PC |
| BLUETTI AC300 | — | 3,000 W | 2,400 W | — | — | Discontinued ([BLUETTI](https://www.bluettipower.com/pages/bluetti-discontinued-models)) |

**Pick: BLUETTI Apex 300.** It's the cheapest of the group, it's under the 1.2 kW load with room for GPU spikes, and it has the best claimed switchover. Weak points: only 2.76 kWh, and its PV inputs are low-voltage (60 V max), so each port takes just one 400 W-class panel. Whether the 0 ms holds on 120 V-only outlets without the home-panel accessory is **unverified**. Test it yourself by pulling the plug under full GPU load before you rely on it.

### Parts list

| Component | Model | Key specs | Approx. price | Why |
|---|---|---|---:|---|
| Power station | BLUETTI Apex 300 | 2,764 Wh LFP, 3,840 W, 2 × 1,200 W PV (12–60 V, 20 A), 0 ms UPS | $1,499 ([source](https://outboundpower.com/products/bluetti-apex-300-portable-power-station-2764-8wh-3840w-2400w-solar-input)) | See above |
| Solar panels (×2) | Lumina SolarSpace 405 W mono | 405 W; Voc ~37 V (**unverified**) | $145.80 each, $292 ([price survey, Signature Solar](https://ecobusinesslinks.com/solar-panels-price-survey/)) | One panel per port keeps Voc under 60 V. Two panels in series would exceed the input limit |
| PV cables / MC4 extensions | Generic 10 AWG PV wire | — | ~$50 (**unverified**) | — |
| **Total** | | | **~$1,840** | |

Solar sizing: 2 × 405 W = 810 W, which is the most this unit's inputs can take with standard panels. Each port allows 20 A, and a typical 400 W panel's Isc is about 13.8 A ([EcoFlow 400 W panel spec, via Solarcellz](https://store.solarcellzusa.com/products/ecoflow-400w-rigid-solar-panel)), so two panels in parallel on one port would exceed the limit. To get more solar, add BLUETTI's SolarX 4K controller (up to 6,400 W total per [ESS News](https://www.ess-news.com/2025/05/19/bluetti-apex-300-with-2-7-kwh-battery-pricing-now-available-ahead-of-launch/); price **unverified**).

### Topology

```text
grounded 120 V receptacle (3-wire, verified with tester)
        │
        ▼
┌──────────────────────── BLUETTI Apex 300 ───────────────────────┐
│ AC in ──► charger ──► 2.76 kWh LFP ──► 3.84 kW inverter ──► AC out ─┼──► workstation + monitors
│              ▲                                                      │
│ PV1 ◄── 405 W panel        PV2 ◄── 405 W panel                      │
└──────────────────────────────────────────────────────────────────────┘
(optional) B300K expansion battery via the expansion port
```

### Pros / cons

| Pros | Cons |
|---|---|
| Cheapest, no DIY DC wiring, single warranty | 2.76 kWh is small (about 2.3 h at 1.2 kW) |
| Fastest claimed switchover | Low-voltage PV inputs limit solar to about 810 W without an add-on |
| Expandable with B300K packs | Not suitable on an ungrounded circuit, because mains passes through to the PC |
| | Proprietary. Repair and parts depend on the vendor (the AC300 was dropped with no parts, per [DIY Solar Forum](https://diysolarforum.com/threads/bluetti-screw-you-ac300-dead-and-no-parts-to-fix-it-anymore-5-years-after-launch.122504/)) |
| | Set the AC charge limit so the AC input plus pass-through draw stays under about 12 A on the branch circuit |

---

## Build 2: assembled component system, grounded site

### Inverter/charger vs. separate charger

On a grounded site, **an all-in-one inverter/charger is better than the README's separate-charger setup.** One box gives you the inverter, the MPPT, the mains charger, and the transfer relay, and it costs less than buying them separately. Run it in battery-priority mode (SBU), so the PC normally runs from the inverter and mains only tops up the battery. A transfer only happens if the unit falls back to grid bypass. That fallback is 10 ms in UPS mode, which is safe.

| Option | Price | Transfer | Notes |
|---|---:|---|---|
| **EG4 3000EHV-48** | $699.99 ([Current Connected](https://www.currentconnected.com/product/eg4-3000-ehv-48v-inverter-charger)) | 10 ms in UPS mode ([EG4 spec sheet](https://eg4electronics.com/wp-content/uploads/2024/05/EG4-3000EHV-48-Spec-Sheet.pdf)) | 3 kW at 120 V, 2× surge for 5 s, built-in 5 kW MPPT (120–450 V operating, 500 V max), 60 A AC charging ([spec sheet](https://eg4electronics.com/wp-content/uploads/2024/04/EG4-3000EHV-48-Spec-Sheet.pdf)). Internal N-G bond screw, removable without voiding the warranty ([manual](https://eg4electronics.com/wp-content/uploads/2024/04/EG4-3000EHV-48-V2-Manual.pdf)) |
| Victron MultiPlus-II 48/3000/35-50 120V | ~$940–1,170 ([Wind and Solar](https://windandsolar.com/products/victron-energy-multiplus-ii-48-3000-35-50-120v), [Off Grid Stores](https://offgridstores.com/products/victron-multiplus-ii-48-3000-35-50-120v)) | <20 ms ([Victron](https://www.victronenergy.com/inverters-chargers/multiplus-ii)) | Better build quality and an internal ground relay. No MPPT, so add a SmartSolar 150/45 (~$230, [Solar-Electric](https://www.solar-electric.com/victron-energy-smartsolar-mppt-150-45-tr-charge-controller.html)). About $500 more in total |

**Pick: EG4 3000EHV-48.** Spend the extra on a MultiPlus-II only if you value Victron's reliability and support enough to justify it.

### Parts list

| Component | Model | Key specs | Approx. price | Why |
|---|---|---|---:|---|
| Battery | WattCycle 48 V 100 Ah server rack | 51.2 V, 5.12 kWh LFP, active balancing, Bluetooth. BMS continuous current **unverified** (check for ≥100 A) | $899.99 ([WattCycle](https://www.wattcycle.com/products/48v-100ah-server-racks-bluetooth-lithium-battery)); seen at $651 on sale ([Slickdeals](https://slickdeals.net/f/18887791-bf-sale-wattcycle-48v-100ah-server-rack-lifepo4-battery-651-ac-lowest-price-i-have-seen)) | The README candidate. Hits the ~$900 per 5 kWh target |
| Inverter/charger + MPPT | EG4 3000EHV-48 | See above | $699.99 | Inverter, MPPT, charger, and 10 ms transfer in one box |
| Solar (×5, series) | Lumina SolarSpace 405 W | 2,025 W. String Voc ~185 V, Vmp ~155 V (per-panel values **unverified**) | 5 × $145.80 = $729 ([survey](https://ecobusinesslinks.com/solar-panels-price-survey/)) | The MPPT needs ≥120 V operating. Four panels (~124 V Vmp) would drop below that on hot days, so use five |
| Class T fuse block | Blue Sea 5007100 | 110–200 A, insulating cover | ~$68–87 ([Off Grid Stores](https://offgridstores.com/products/blue-sea-5007100-class-t-fuse-block-w-insulating-cover-110-to-200a)) | Class T has a high DC interrupt rating, which an LFP short-circuit needs |
| Class T fuse | Blue Sea 125 A Class T | 125 A | ~$70 ([same search result](https://offgridstores.com/products/blue-sea-5007100-class-t-fuse-block-w-insulating-cover-110-to-200a), exact listing **unverified**) | Protects 2 AWG cable (see below) |
| DC breaker / disconnect | MidNite MNEDC125RT | 125 A, 125 VDC, panel mount | ~$50–55 ([Solar-Electric](https://www.solar-electric.com/midnite-solar-mnedc125rt-remote-trip-circuit-breaker.html)) | DC-rated battery disconnect. Needs a small enclosure (**unverified** cost) |
| Busbars | Pair of 250 A+ busbars (e.g. Victron Lynx or Blue Sea PowerBar) | — | ~$60–120 (**unverified**) | Only needed if more than one device lands on the battery. With just the inverter you can skip them |
| Battery cables | 2 AWG copper, fine-strand, crimped lugs, ~2 × 1.5 m | — | ~$60–80 (**unverified**) | See calculation below |
| PV disconnect | 2-pole DC PV disconnect rated ≥250 VDC (e.g. MidNite MNPV-series breaker in a combiner) | 15–20 A | ~$60–100 (**unverified**) | Required to isolate the array. A single string needs no PV fuse |
| PV wire / MC4 | 10 AWG PV wire | — | ~$60 (**unverified**) | — |
| **Total** | | | **~$2,650–2,800** (~$1,900–2,050 without panels) | |

Over budget with full solar. To trim it: buy the battery on sale ($651), skip the busbars, or start without panels (the system still works as a UPS plus buffer) and add the five-panel string later.

**DIY battery alternative:** 16 × 3.2 V ~100 Ah prismatic LFP cells with a 100 A 16S smart BMS (e.g. JK). Typically a bit cheaper per kWh, but you take on compression fixtures, busbar torque, top-balancing, an enclosure, and no system-level listing. Prices **unverified**. Not recommended unless you've built packs before.

### Cable sizing

- Worst-case continuous DC current is 3,000 W ÷ (44 V low battery × 0.92 efficiency) ≈ **74 A**. At the 1.2 kW design load it's about 30 A.
- Surge of 2× for 5 s is about 150 A. That's short enough for the cable to handle, and the Class T fuse's time-current curve rides through it.
- 2 AWG copper is rated 115 A at 75 °C (NEC Table 310.16). That's ≥1.25 × 74 A = 93 A ✅. A **125 A** fuse is the next standard size up (NEC 240.4(B)).
- Voltage drop at 74 A over a 3 m round trip of 2 AWG (0.156 Ω/1000 ft ≈ 0.51 mΩ/m) is 74 × 0.0015 ≈ **0.11 V (0.2 %)**. Fine.
- 4 AWG (85 A at 75 °C) also works with a 90–100 A fuse, but leaves less margin for spikes. Check the EG4 manual's recommended cable size and use the larger of the two.

### Topology

```text
grounded 120 V receptacle ──► EG4 3000EHV-48 AC IN
                              │  (SBU mode: battery first, grid bypass at low SOC; 10 ms transfer)
PV: 5 × 405 W in series (~185 Voc)
   └─► 2-pole PV disconnect ──► EG4 PV IN (MPPT)
                              │
                              │ DC (2 AWG, ≤1.5 m each way)
battery + ─► Class T 125 A ─► MNEDC125 breaker ─► EG4 BAT+
battery − ──────────────────────────────────────► EG4 BAT−
                              │
                         EG4 AC OUT ──► workstation + monitors
chassis: EG4 ground lug + battery case ──► AC input EGC (from receptacle)
N-G: remove EG4 internal bond screw (the grid is already bonded at the panel; keep one bond only)
```

The N-G screw: with grid input connected, the system already has an N-G bond at the main panel, so a second bond in the inverter is normally removed. There is a [known community debate](https://diysolarforum.com/threads/neutral-ground-bonding-on-3000ehv-48-inverters.60061/) about this unit, because it has no automatic bond relay. When it runs on battery with the bond removed, the output neutral isn't bonded. Have the installer confirm the configuration.

### Pros / cons

| Pros | Cons |
|---|---|
| 5.12 kWh plus ~2 kW of solar, well matched to the load | Over budget with full solar |
| Standard parts, each one repairable or replaceable | DIY DC wiring; needs care with torque, fusing, and layout |
| Transfer happens only on fallback, at 10 ms | EG4 has a fan and some idle draw (**unverified**: ~40–60 W per user reports) |
| Room to grow (more battery in parallel, more panels) | Needs a grounded circuit. The N-G bond needs a decision |

---

## Build 3: ungrounded site (no equipment ground)

### The charger: does a certified Class II ~1.2 kW 48 V LFP charger exist?

**No such product could be found.** Every certified charger at 1–1.2 kW that I found is Class I, meaning it needs a protective-earth connection:

| Charger | Power | Construction | Certification | Price | Verdict |
|---|---|---|---|---:|---|
| Mean Well NPB-1200-48 | 1,210 W, 57.6 V, 18 A, PFC, 94 % efficient | IEC320-C14 3-pole inlet, I/P-O/P 3 kVAC isolation (series datasheet, [via search of Mean Well spec](https://www.meanwell-web.com/data/uploads/documents/Datasheets/NPB/NPB-1700-spec.pdf)) → **Class I** | UL 62368-1 ([TRC](https://www.trcelectronics.com/products/mean-well-npb-1700-48), series) | $277 ([DigiKey](https://www.digikey.com/en/products/detail/mean-well-usa-inc/NPB-1200-48/22119131)) | Best certified, galvanically isolated option. Not Class II |
| Delta-Q IC1200 | 1,200 W, 48 V/25 A, lithium profiles | C14 inlet ([spec](https://www.master-instruments.com.au/products/67105/dqic1200-48-25can.html)) → Class I | UL 1564, CSA 107.2 | **unverified** | Rugged (IP66). Profile needs a dealer to program |
| Victron Skylla-i / Skylla-TG 48 V | 25–50 A | "Safety class I" ([Skylla-i manual](https://www.victronenergy.com/upload/documents/Manual-Skylla-i-EN-NL-FR-DE-ES-IT.pdf)) | — | — | Class I |
| Victron Blue Smart IP22 | 12/24 V only | NEMA 5-15 (3-prong) ([Victron](https://www.victronenergy.com/chargers/blue-smart-ip22-charger)) | — | — | No 48 V model |
| EG4 Chargeverter-GC | Up to 3,050 W at 120 V | Class I (grounded inverter-style) (**unverified**) | — | ~$530 ([Signature Solar](https://signaturesolar.com/eg4-chargeverter-gc-48v-100a-battery-charger-5120w-output/)) | Oversized, and is it isolated? (**unverified**) |
| LiTime 58.4 V 10 A | ~580 W | Not stated | FCC, CE, RoHS, UN38.3 only. **No UL** ([LiTime](https://www.litime.com/products/litime-58-4v-10a-lithium-battery-charger-for-48v-lifepo4-lithium-battery)) | $136 | Fails the certification requirement |
| Amazon golf-cart chargers (EPOWREY 18 A, etc.) | ~1,000 W | Not stated | Listing says "UL" without a file number ([Amazon](https://www.amazon.com/dp/B0F6MFZLD1)) | ~$130 | Can't verify the listing or the Class II claim. Don't rely on it |

**Paralleling several small Class II chargers** doesn't work out either. The listed 2-prong Class II chargers I found are e-bike units around 100–250 W, mostly with 54.6 V Li-ion (13S) profiles rather than 58.4 V LFP (e.g. [Hyleton 54.6 V 2 A](https://www.amazon.com/Certified-Overvoltage-Overcurrent-Overtemperature-Protection/dp/B0DYNP9X7Z)). Reaching 1.2 kW would take 5–10 of them, and finding a listed 58.4 V version is **unverified**. That isn't a sound design.

**An isolation transformer doesn't fix it either.** The [Tripp Lite IS1000HG](https://www.amazon.com/Tripp-Lite-IS1000HG-Isolation-Transformer/dp/B00008YMZO) (1,000 W, UL 60601-1, ~$500–600) is itself a Class I device with a 3-prong plug. It also gives you nothing that the NPB-1200's own 3 kVAC input-output isolation doesn't already provide.

### Best compliant alternative

1. **First choice: a licensed electrician adds an EGC (NEC 250.130(C)) or runs a new grounded circuit.** The ungrounded problem then goes away, and you can use build 1 or 2. Do this if you can.
2. **If that isn't possible:** NEC 406.4(D)(2) lets you replace a 2-wire receptacle with a GFCI receptacle marked **"No Equipment Ground"** ([ECM](https://www.ecmweb.com/national-electrical-code/qa/article/20900802/stumped-by-the-code-nec-requirements-when-replacing-nongrounding-type-receptacles), [NC OSFM](https://www.ncosfm.gov/electrical/4064d2-gfci-stickers-0/open)). The GFCI gives shock protection without an EGC. Plug the Class I, galvanically isolated **Mean Well NPB-1200-48** into it. The charger's earth pin then connects to nothing on the mains side. Its chassis (FG) is bonded only to the local DC/inverter bonding point (below). Have the electrician install the GFCI receptacle and sign off on this arrangement. The Y-capacitor leakage from the NPB's input to FG flows through the GFCI's sensing path; whether it stays under the ~5 mA trip threshold is **unverified** and has to be tested.

This is a code-permitted receptacle with a certified isolated charger. It isn't the true Class II charger the README asks for, because that product doesn't appear to exist at this power level.

Charger notes: the NPB-1200-48 has a 57.6 V maximum. Set CV to 56.8–57.6 V per the battery maker; a little under 58.4 V is gentler on LFP. At full output it draws about 1,210 / 0.94 / 120 ≈ **10.7 A** from the wall, which fits the README's ≤11 A target.

### Parts list (build 2 parts reused)

| Component | Model | Approx. price | Notes |
|---|---|---:|---|
| Battery | WattCycle 48 V 100 Ah | $899.99 | As in build 2 |
| Inverter + MPPT | EG4 3000EHV-48, **AC input left unconnected** | $699.99 | Runs as a pure off-grid inverter. Mains never reaches it |
| Mains charger | Mean Well NPB-1200-48 | $277 ([DigiKey](https://www.digikey.com/en/products/detail/mean-well-usa-inc/NPB-1200-48/22119131)) | Isolated, UL 62368-1, Class I |
| Charger DC protection | 30–40 A DC-rated fuse or breaker on the charger output, plus 10 AWG cable | ~$30 (**unverified**) | Protects the charger lead against back-feed from the battery |
| Supply GFCI receptacle | Any listed 15/20 A GFCI with a "No Equipment Ground" label | ~$25 (**unverified**) | Installed by an electrician per 406.4(D)(2) |
| Output GFCI | Listed 20 A GFCI receptacle in a metal box on the inverter output | ~$25 (**unverified**) | See grounding section |
| Bonding bar + 6 AWG bonding wire | — | ~$30 (**unverified**) | Local equipment bonding |
| DC protection, cabling, PV disconnect | Same as build 2 (Class T 125 A, MNEDC125, 2 AWG, PV disconnect, busbar) | ~$400 | Busbar needed here, since the charger and inverter both land on the battery |
| Solar | 5 × SolarSpace 405 W series | $729 | As in build 2 |
| **Total** | | **~$3,100** (~$2,370 without panels) | |

Lower-cost option, per the README: a 2.56 kWh 48 V 50 Ah battery instead of 5.12 kWh. The charger alone can carry the load. Model and price **unverified**.

### Topology

```text
2-wire mains ──► GFCI receptacle "No Equipment Ground" (electrician-installed)
                    │ (charger earth pin connects to nothing upstream)
                    ▼
            Mean Well NPB-1200-48 (isolated, 3 kVAC I/P-O/P)
                    │ 57.x V, 18 A max
                    ▼ fuse 30–40 A
             ┌──── + DC busbar ◄── Class T 125 A ◄── MNEDC125 ◄── battery +
             │      − DC busbar ◄──────────────────────────────── battery −
             │
PV 5×405 W ──► PV disconnect ──► EG4 3000EHV-48 PV IN
             └────────────────────► EG4 BAT+/BAT−      (AC IN unused)
                                         │
                                    EG4 AC OUT  (N-G bond screw KEPT: one bond, here)
                                         │
                                    GFCI receptacle (metal box)
                                         │
                                    workstation PSU + monitors

Local bonding bar ◄── EG4 chassis lug
                  ◄── battery case / rack
                  ◄── NPB-1200 FG (chassis)
                  ◄── EG4 output ground (receptacle ground pin → PC chassis via its cord)
(no connection to the building circuit, no ground rod used as an EGC)
```

### AC-output grounding for the Class I PC

How the pieces fit:

- **The inverter output is a separately derived source.** Nothing in it connects to mains. Keep the EG4's **internal N-G bond screw in place**, so its output neutral and ground are bonded at one point, inside the inverter. This works like a portable generator with a bonded neutral. A neutral-ground bond is **not** an earth connection, and it doesn't need to be one. Its job is to give a hot-to-chassis fault a low-impedance path back to the source, so overcurrent protection or a GFCI can clear it.
- **Local equipment bonding.** Run 6 AWG copper from the inverter chassis lug, the battery case or rack, and the NPB charger's FG to one bonding bar. The PC's chassis bonds to it through its 3-prong cord and the output receptacle's ground pin. All exposed metal then sits at the same potential.
- **GFCI on the inverter output.** Use a listed GFCI receptacle fed by the inverter's output. Because neutral and ground are bonded at the inverter, the GFCI sees any current leaking to the chassis, or through a person, as an imbalance, and trips at ~5 mA. Some inverters cause nuisance trips on GFCIs. Whether the EG4 works with an output GFCI is **unverified**, so test it with the push-button and a plug-in GFCI tester.

Fault cases:

| Fault | Result |
|---|---|
| PC PSU hot-to-chassis | Current flows chassis → ground pin → inverter N-G bond → neutral. The output GFCI trips, and the inverter's overcurrent limit or shutdown backs it up |
| Inverter hot-to-chassis | Same path through the bond. Inverter protection trips |
| DC-to-chassis (battery + to the rack) | The DC side floats, so one fault draws no current. A second fault is a dead short, and the Class T fuse clears it. Check with a meter periodically, or add a DC ground-fault monitor (**unverified** product) |
| Mains hot-to-chassis inside the charger | The charger chassis has no return path to the mains neutral, so no fault current flows and nothing trips. The chassis (and the whole bonded system) can sit at line voltage relative to earth. If a person or a grounded object completes the path, the supply-side GFCI sees the imbalance and trips at ~5 mA. This is the main residual risk, so test the GFCI monthly |
| Touch voltage | With everything bonded locally there's no voltage difference between exposed parts of this system |
| PSU leakage current | A switching PSU leaks well under 1 mA to its chassis, which returns through the bond and doesn't trip the GFCI. Several PSUs plus monitors add together, so test |

**Ethernet, USB, and display cables to other equipment:**

- Any device grounded to the **building** circuit (a router or printer on a different, grounded circuit) connects to the inverter system through cable shields. That cable shield becomes an unintended bond between two ground systems. Fault current or ground-loop current can then flow on a thin shield.
- Ethernet is transformer-isolated at each port, but shielded cable and the port housing are often tied to the chassis. **Use unshielded (UTP) Cat6**, or a fiber or media-converter link to the network.
- **Plug monitors, speakers, USB hubs, and docks into the inverter output too,** so they share the same bonded ground. Avoid USB or DisplayPort connections to anything powered from building mains. If you can't avoid it, use a USB isolator (in USB 2.0 speed classes) or a fiber HDMI/DP cable.
- Since the building circuit has no EGC, there's nothing grounded there to loop with today. Recheck this whenever new equipment is added.

**Never do these:** a bootleg ground (neutral jumpered to ground at a receptacle), cheater plugs, or a ground rod in place of the equipment-grounding conductor.

### Pros / cons

| Pros | Cons |
|---|---|
| Mains is galvanically isolated from the PC. No transfer event, ever | Most expensive build. Over budget with full solar |
| Uses certified parts and a code-permitted GFCI receptacle | No true Class II ~1.2 kW charger exists, so you rely on the 406.4(D)(2) GFCI with a Class I charger |
| The charger can carry the full load, so the battery only buffers | More DIY wiring: three sources on one DC bus, plus a bonding scheme |
| Still works fully if the circuit is later grounded | GFCI behavior on the inverter output and the charger's leakage both need testing |

**Bottom line:** if an electrician can add an EGC or a new circuit for a few hundred dollars (**unverified** local cost), do that and buy build 1 (cheapest) or build 2 (most capable). Build 3 works, but it costs more and is more complex than fixing the wiring.
