# solar-build

Design workspace for a small off-grid solar + battery system that runs one high-power AI workstation (Threadripper + one modded 96 GB GPU).

Solar comes first. The battery buffers it, and mains takes over automatically without interrupting the computer. This is **not** a whole-home backup system. It is sized for one workstation plus its monitors and network gear.

> **Status:** Concept / research. Nothing has been purchased. See [Open engineering questions](#open-engineering-questions).

---

## Target architecture

```text
two-wire mains
      │
      ▼
certified isolated AC/DC charger (Class II, ~1.0–1.2 kW)
      │
      ├───────────────┐
      ▼               │
  51.2 V BUS ◄──── MPPT ◄──── solar (~800–1,200 W)
      │
      ▼
 ~5 kWh LiFePO₄ (16S)
      │
      ▼
3 kW pure-sine inverter
      │
      ▼
workstation
```

The computer runs from the inverter **all the time**. Mains never bypasses the inverter. It only charges the DC bus. If mains drops out, the charger stops contributing and the battery keeps the inverter going, so no AC transfer event ever happens.

Design philosophy: **use the battery as an energy buffer, not as a way to replace the grid overnight.**

### Operating modes

| Condition | Power flow |
|---|---|
| Strong sun | solar → workstation + battery charging |
| Cloud | solar + battery → workstation |
| Weak sun | solar + battery + mains charger → workstation |
| Night | mains charger + battery buffer → workstation |

---

## Critical site constraint: no equipment ground

The office appears to have **120 V mains with no working equipment-ground conductor**. A receptacle tester will confirm this. If it reports **OPEN GROUND**, treat the circuit as genuinely ungrounded.

The design must **not** rely on any of these:

- neutral-to-ground "bootleg" bonding at the outlet
- cheater/fake grounding plugs
- a standalone ground rod used in place of the branch-circuit equipment grounding conductor
- any other kind of "ground spoofing"

The workaround is to keep the old two-wire mains **galvanically isolated** from the DC/battery/inverter system.

> ⚠️ An isolated Class-II charger only solves the *mains-charging* side. It does **not** give the computer chassis a protective-earth connection. The inverter-output grounding needs its own engineering (see below).

---

## Load assumptions

| Parameter | Value |
|---|---:|
| Design max (AC, sustained or burst) | **1.2 kW** |
| Expected average | 600–1,000 W (**needs measurement**) |

### Power budget at stock settings

| Component | DC power |
|---|---:|
| 1× modded 96 GB GPU (4090-based, 450 W; 5090-class, 575 W) | 450–575 W |
| Threadripper (7000/9000 series) | ~350 W |
| Motherboard, RAM, NVMe, fans, pump | ~80–120 W |
| **DC total** | **~880–1,045 W** |
| **AC at wall (~92% PSU)** | **~0.96–1.14 kW** ✅ |

This fits under 1.2 kW at stock settings, but with little room for monitors. If measurements show it going over, cap the GPU (`nvidia-smi -pl`) or the CPU (PPT). The GPU's millisecond spikes (up to about 2× its rating) are absorbed by the 3 kW inverter and the battery, so they don't affect the 1.2 kW sizing.

Confirm the card's limits with:

```
nvidia-smi --query-gpu=name,memory.total,power.default_limit,power.max_limit --format=csv
```

### Measurements needed before purchase

Measure with an AC power meter:

| Condition | Watts |
|---|---|
| System idle | |
| Normal coding/work | |
| GPU loaded | |
| CPU + GPU max | |
| PC + all monitors/peripherals | |
| Highest short-duration load | |

Also check the office receptacle with a receptacle tester.

---

## Why 48 V (51.2 V nominal)

| Bus | Battery current at ~1.2 kW AC |
|---|---:|
| 24 V | ~55 A |
| 51.2 V | ~26 A |

The 48 V bus cuts cable current, connector heating, voltage drop, BMS stress, and inverter losses.

---

## Component requirements

### Battery

| Requirement | Target |
|---|---:|
| Nominal voltage | 48 / 51.2 V (16S) |
| Preferred capacity | ~5.12 kWh (51.2 V × 100 Ah) |
| Minimum practical capacity | ~2.5 kWh |
| Continuous output | ≥3 kW preferred |
| BMS continuous current | ≥60 A, ~100 A preferred |
| Chemistry | LiFePO₄ preferred (sodium-ion if one becomes economical) |
| Cycle life | ≥4,000 cycles |
| Price target | ~$800–1,000 for 5.12 kWh |

Candidate to re-evaluate: WattCycle 48 V / 100 Ah.

### Inverter

| Requirement | Target |
|---|---:|
| Input | 48 / 51.2 V |
| Continuous output | ~3,000 W |
| AC output | 120 V / 60 Hz, pure sine |
| Surge | ≥5–6 kW transient preferred |
| Load at 1.2 kW | ~40% of rating |
| Efficiency | ≥90–93% in the relevant load range |

Document the following: floating vs. bonded neutral, any internal N-G bonding/transfer relay, GFCI compatibility, and the manufacturer's protective-earth requirements. A neutral-ground bond is **not** an earth-ground conductor.

### Mains charger (highest-priority research item)

| Requirement | Target |
|---|---:|
| AC input | 120 V / 60 Hz, 2-prong |
| Construction | Class II / double insulated |
| Isolation | Galvanic, input-to-output |
| Certification | UL / ETL or equivalent |
| Output | 16S LiFePO₄, ~58.4 V (per battery maker) |
| Power | ~1.0–1.2 kW |
| PFC | Preferred |
| Duty | Continuous |

It must be explicitly designed to run without protective earth. A two-prong plug alone doesn't prove isolation, so verify certification and topology.

Energy balance with a 1.2 kW charger:

| PC load | Battery behavior |
|---:|---|
| 800 W | charging |
| 1.0 kW | slow charge / near neutral |
| 1.2 kW | ~energy neutral |
| 1.2 kW (design max) | ~energy neutral; battery only covers spikes and charger shortfall |

Expect ~10–11 A draw on the 120 V circuit. Don't design for sustained 15+ A on an unverified older branch circuit.

### Solar

- Initial array: **~800–1,200 W**, rigid commodity panels (e.g. 2 × 400 W to start)
- MPPT for a 48 V LiFePO₄ battery, ≥1,200 W PV input, high efficiency
- **Pick the PV string voltage before choosing the MPPT**
- Leave room to add panels later without replacing core components

---

## Runtime estimates

5.12 kWh nominal, ~90% practical → **~4.6 kWh usable AC**.

| Load | Battery-only runtime |
|---:|---:|
| 600 W | ~7.5 h |
| 800 W | ~5.7 h |
| 1,000 W | ~4.6 h |
| 1,200 W | ~3.8 h |

With the 1.2 kW charger running, the charger alone can carry the full design load. The battery mostly absorbs GPU spikes, solar dips, and mains outages, so a smaller battery (~2.5 kWh) is a viable lower-cost option. Losses and charge/discharge limits still need to be modeled.

---

## Required protection hardware

Select and size:

- battery-side fuse (DC interrupt rating matters)
- battery disconnect / DC-rated breaker
- busbars as needed
- copper battery cables with properly crimped lugs
- PV disconnect and PV overcurrent protection (depends on array topology)
- charger-side DC protection
- inverter chassis bonding strategy
- battery enclosure / physical protection
- ventilation and clearances per manufacturer

Don't use ordinary AC breakers for high-current DC unless they're rated for that DC voltage and current.

---

## Benchmark: PECRON E2000LFP

This is a reference for its topology, not a current favorite. It has 1.92 kWh LFP, a 2 kW inverter, ~1.2–1.3 kW solar input, and an **external ~42 V DC AC charger**, so mains is turned into isolated low-voltage DC before it enters the unit. Downsides: no UPS support, only a ~600 W charger, and it looks to be legacy inventory. Worth it at ~$600, not at $1,000+.

---

## Budget

Target **$1,500–2,000** in hardware before installation. Lower is better.

Spend on: battery, a safe isolated charger, a quality inverter, the MPPT, DC protection, and properly sized cabling.

Avoid: 10+ kWh storage, 6 kW+ inverters, whole-home transfer equipment, premium portable panels, and a redundant online UPS.

Optimize for $/usable kWh, $/W of inverter, solar utilization, battery longevity, isolation from ungrounded mains, and continuous computer operation.

---

## Open engineering questions

1. **Isolated charger:** Find a certified Class-II, galvanically isolated, two-prong, ~1.0–1.2 kW charger for 16S 51.2 V LiFePO₄.
2. **Inverter:** Choose a quality ~3 kW 48 V pure-sine unit and fully document its neutral/ground/chassis topology.
3. **Protection and wiring:** Work out cable gauge, battery fuse, disconnects, busbars, connectors, and PV protection.
4. **AC-output grounding (deepest review needed):** Find the safest code-compliant way to power a Class-I desktop PSU from the isolated inverter when the building circuit has no equipment ground. Analyze:
   - PC PSU hot-to-chassis fault
   - inverter hot-to-chassis fault
   - DC-to-chassis fault
   - bonding of exposed inverter/battery metal
   - GFCI behavior and neutral-ground topology
   - touch voltage and switching-PSU leakage current
   - Ethernet/USB/display cables connecting the PC to externally grounded equipment

   Do **not** solve these with an improvised neutral-ground jumper.
