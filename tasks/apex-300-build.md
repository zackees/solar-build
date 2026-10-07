# Task: Apex 300 workstation build

Chosen setup: a **BLUETTI Apex 300** power station running the workstation in 0 ms mode, charged from the ungrounded 120 V wall and up to four 400 W rigid panels. This replaces the custom builds in [BUILDS.md](../BUILDS.md) and [BUDGET.md](../BUDGET.md) as the plan.

Location is the Mission District, San Francisco: partly cloudy, little tree shade.

## Why this unit

All specs below are from the Apex 300 US user manual V1.0 ([PDF](https://images.thdstatic.com/catalog/pdfImages/bc/bc015b2f-eb9c-47e6-ab3c-92378e02f58b.pdf)).

| Need | Apex 300 |
|---|---|
| No interruption to the PC | **0 ms** on the two left NEMA 5-20R outlets when the selector is set to **240 V** and the unit is fed from the 120 V wall. Those outlets always run from the inverter. The right outlets switch in 20 ms |
| 1.2 kW design max | Left two outlets: 16 A each, **1,920 W combined** |
| Wall charging | 120 V / 15 A input, **1,800 W max** (charging + pass-through), about 11 A at 1.2 kW |
| Gradual solar → wall handoff | **Backup** UPS mode: "charges using available solar and grid power, with a priority on solar" |
| Solar | 2 DC inputs, each **12–60 V Voc, 20 A, 1,200 W** |
| Battery | 2,764.8 Wh LiFePO₄ (about 2 h at 1.2 kW on battery alone) |
| Idle draw | ~20 W |

## Grounding

- **It works without a ground wire.** The manual has no ground-detection error codes. Forums and BLUETTI's own docs say the output has a floating ground and the unit charges fine from a 2-wire outlet.
- **BLUETTI still says to ground it for safety.** The manual says "must be grounded when connected to the grid". It also covers the 2-pin / ineffective-ground case, telling you to run a wire from the unit's grounding terminal to the wall outlet or breaker panel ground.
- **Without a ground, the risk is the same as plugging the PC straight into that outlet today.** A hot-to-case fault inside the Apex or the PC won't trip anything.
- **Plan:** have the office outlet replaced with a **GFCI receptacle labeled "No Equipment Ground"** (NEC 406.4(D)(2)). It legally accepts the 3-prong plug, so no cheater adapter is needed. If the GFCI trips, the PC stays up on battery.
- **Optional upgrade:** have an electrician run a ground wire to the breaker panel (NEC 250.130(C)), about $100–300.
- **Never** use a cheater plug, a bootleg ground, or a ground rod in place of the equipment ground.

## Solar plan

- Panel type barely matters in overcast. Premium "low-light" panels gain only 2–5%. **Panel area is what counts.**
- **Over-panel each input:** 4 × 400 W, **two in parallel per input** = 1,600 W nameplate.
  - Roughly doubles cloudy-day output (~320 W vs ~160 W at 20% light).
  - Clips slightly at ~1,200 W on the brightest days, which costs little.
  - Rough yearly average: ~5–6 kWh/day vs ~2.5–3 kWh/day with two panels (**estimate, unverified**).
- **Never wire two of these panels in series.** 2 × ~37–45 V Voc exceeds the 60 V input limit.
- **Mounting:** face south (or SW for afternoons after fog clears), tilt flat-ish at **10–20°** to catch diffuse sky light, keep them clean, and leave an air gap behind them.

### Current-limit question

Two QCells in parallel can produce about **22 A** short-circuit current (Isc), against a "20 A max" input.

- Panels can't push current. Isc is a physical ceiling, reached only in full bright sun.
- The Apex controls how much it draws, and the manual says **DC input current can be adjusted in the app's advanced mode**.
- Most MPPTs tolerate over-paneling, but BLUETTI doesn't say either way for the Apex.
- **Fallback:** pick panels with Isc ≤ 10 A, so two in parallel stay ≤ 20 A even in a fault.

## Shopping list

Prices as of 2026-10-06. Walmart and eBay listings couldn't be machine-verified, so confirm price and stock.

| Item | Where | Price |
|---|---|---:|
| Apex 300, refurbished ("Restored: Like New") | [Walmart](https://www.walmart.com/ip/Restored-Bluetti-Apex-300-Expandable-Power-Station-2764-8Wh-Capacity-NEMA-TT-30-L14-50-AC-Car-Charge-for-Outdoor-Adventures-Refurbished/17739918627?conditionGroupCode=2) | $1,193.99 |
| *Alt:* Apex 300, certified refurbished (listing claims 5-yr warranty) | [eBay](https://www.ebay.com/itm/376511915423) | $1,199 |
| *Alt:* Apex 300 refurbished + free 200 W panel | [eBay via Slickdeals](https://slickdeals.net/f/18702634-bluetti-apex-300-2764wh-portable-power-station-expandable-generator-certified-refurbished-free-200w-solar-panel-1279) | $1,279 |
| *Alt:* BLUETTI official refurbished store | [bluettipower.com](https://www.bluettipower.com/collections/refurbished-1) | varies |
| 4 × QCells 400 W (Voc 45.3 V) | [SanTan Solar](https://www.santansolar.com/product/new-qcells-400w-black-mono-solar-panel/) | 4 × $140 = $560 |
| *Alt panels:* Alexus 400 W (Voc 37.2 V) | [SanTan Solar](https://www.santansolar.com/product/alexus-solar-400w-mono-solar-panel/) | $145 each |
| Panel freight shipping (SanTan is in Arizona) | — | ~$0–150 (**unverified**). Local pickup or Marketplace may be cheaper |
| 2 × MC4-to-XT60 solar cables, 12 AWG | search "MC4 to XT60 solar charging cable 12 AWG", or BLUETTI's official cable | ~$40 |
| 2 × MC4 Y-branch connectors (to parallel 2 panels per input) | any listed MC4 branch pair | ~$20 (**unverified**) |
| GFCI receptacle + "No Equipment Ground" label, electrician-installed | local | ~$90–200 (**unverified**) |
| **Total** | | **~$1,900–2,165** |

With two panels instead of four: ~$1,620–1,885.

## Settings

- [ ] Voltage selector: **240 V** (switching cuts output, so set it before connecting the PC)
- [ ] PC plugged into a **left** 5-20R outlet (0 ms). Monitors go on the other left outlet or on the right outlets (20 ms)
- [ ] App → UPS mode: **Backup** (solar priority)
- [ ] App → **Grid Self-Adaption mode: OFF** (manual: "Not for devices like data servers or workstations")
- [ ] App → AC-ECO: **OFF**
- [ ] App → advanced mode: set each DC input's current limit to ≤ 20 A

## To do

- [ ] Ask BLUETTI support: "Can two panels with combined Isc ~22 A be paralleled on one Apex 300 DC input if the input current is set to 20 A in the app?" If no, pick panels with Isc ≤ 10 A
- [ ] Check the office outlet with a receptacle tester (expect OPEN GROUND)
- [ ] Measure workstation power (idle, normal work, GPU max, CPU + GPU max, with monitors)
- [ ] Buy the Apex 300 (check warranty terms on the refurbished listing)
- [ ] Buy panels, cables, and Y-connectors
- [ ] Have an electrician install the GFCI "No Equipment Ground" receptacle (optional: add a ground wire to the panel)
- [ ] Apply the settings above
- [ ] **Load-pull test** during the return window: run CPU + GPU at full load, pull the wall plug (or press GFCI TEST), and repeat 5–10×. The PC must never reboot
- [ ] Watch app input/output around sunset to confirm the solar → wall handoff is gradual
- [ ] Test the GFCI monthly
