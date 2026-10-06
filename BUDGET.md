# Budget builds

Cost-optimized versions of the three builds in [BUILDS.md](BUILDS.md). Prices were taken from retailer listings and search results on 2026-10-06 and change often (sale prices especially). **Unverified** means the figure or spec could not be confirmed from a manufacturer or retailer page.

Requirements kept: 1.2 kW sustained AC with headroom for GPU spikes (≥2.4 kW continuous), pure sine, LiFePO4, solar input, and the PC stays up if mains drops. Build 3 also keeps galvanic isolation plus the NEC 406.4(D)(2) GFCI approach. Safety items kept: Class T fuse, correctly sized cable, listed GFCIs, certified charger.

## Summary

| | Build 1: all-in-one | Build 2: component, grounded | Build 3: ungrounded |
|---|---:|---:|---:|
| BUILDS.md total | ~$1,840 | ~$2,650–2,800 | ~$3,100 |
| **Budget total** | **~$1,515** | **~$2,010** (~$1,450 without panels) | **~$2,430** (~$1,870 without panels), plus ~$90–200 electrician for the GFCI |
| Savings | ~$325 | ~$640–790 | ~$670 |
| Main cuts | Certified-refurb Apex 300, cheaper panels | 2.56 kWh battery, 100 A fuse, no external breaker or busbars, 4 panels | Same as build 2 |

**Cheapest viable overall: have an electrician add an equipment-grounding conductor, then buy budget build 1.** That's about $100–300 for the grounding work ([Angi](https://www.angi.com/articles/how-much-would-it-cost-ground-outlet.htm), [HomeGuide](https://homeguide.com/costs/cost-to-ground-an-outlet-or-a-house); your actual quote is **unverified** and depends on the wiring run) plus ~$1,515, so **~$1,615–1,815 in total**. That's still cheaper than budget build 3 and much simpler. If you want more solar and repairable parts, budget build 2 without panels (~$1,450) plus the electrician is a close second.

---

## Levers evaluated

### Battery: 2.56 kWh vs 5.12 kWh

| Battery | kWh | Price | $/kWh | Notes |
|---|---:|---:|---:|---|
| ECO-WORTHY 48 V 50 Ah server rack | 2.56 | $469.99 new ([eBay listing](https://www.ebay.com/itm/127963500931)); $448.49 refurb ([eBay](https://www.ebay.com/itm/396907845115)); $534.99 ([MyGeneratorPlus](https://www.mygeneratorplus.com/products/eco-worthy-48v51-2v-50ah-2-56kwh-3u-server-rack-lithium-battery)) | ~$184 | **50 A max continuous discharge**, 100 A BMS, built-in 63 A breaker, CAN/RS485 ([eBay listing specs](https://www.ebay.com/itm/127963500931); [Off Grid Stores](https://offgridstores.com/products/eco-worthy-48v51-2v-50ah-server-rack-battery)). UL listing **unverified** |
| ECO-WORTHY Cubix100 48 V 100 Ah | 5.12 | $799.99 ([Walmart](https://www.walmart.com/ip/ECO-Worthy-Cubix-100-48V-100Ah-LiFePO4-Lithium-Battery-w-Bluetooth-UL1973-UL9540A-CEC-Listed-5120Wh-Server-Rack-Battery-Solar-Energy-Storage-Home-Bac/19590615294)); $829.99 ([Urban Supply](https://www.urbansupplyinc.com/products/eco-worthy-48v-100ah-server-rack-ul9540a)) | ~$156 | UL 1973, UL 9540A, CEC listed (same listings) |
| WattCycle 48 V 100 Ah (BUILDS.md pick) | 5.12 | $899.99 ([WattCycle](https://www.wattcycle.com/products/48v-100ah-server-racks-bluetooth-lithium-battery)) | ~$176 | Beaten by the Cubix100 on price and listing |
| DIY: 16 × EVE 100 Ah cells + 16S BMS | 5.12 | Cells $38.50 each = $616 ([EV Components](https://www.evcomponents.com/eve-100ah-prismatic-lifepo4-cell-free-ship.html)); BMS, busbars, case, compression ~$150 (**unverified**) | ~$150 | Cheapest only on paper. Saves ~$30 vs Cubix100, but has no system listing and needs compression, balancing and an enclosure. **Not recommended** |

**Pick: ECO-WORTHY 48 V 50 Ah (2.56 kWh) for builds 2 and 3.** It's $330 cheaper than the 5.12 kWh Cubix100, and since mains carries the load the battery is only a buffer.

The catch is the **50 A continuous discharge limit**. 50 A × ~51 V ≈ 2.5 kW from the battery alone, which is over the 1.2 kW load (~28 A DC) and leaves about 2× headroom. Millisecond GPU spikes sit well inside that. Whether the BMS overcurrent delay rides through spikes above 50 A is **unverified**, so test under GPU load. If you want more runtime or margin, the Cubix100 at $799.99 is the best $/kWh with a confirmed UL 1973 listing. Adding a second 50 Ah pack later works too, but costs more (~$940).

Runtime on battery alone at 1.2 kW (≈1.3 kW DC with inverter losses, 90 % usable): 2.56 kWh → **~1.8 h**, 5.12 kWh → **~3.5 h**.

### Inverter / all-in-one

| Option | Price | Verdict |
|---|---:|---|
| **EG4 3000EHV-48** | $699.99 ([Current Connected](https://www.currentconnected.com/product/eg4-3000-ehv-48v-inverter-charger)); all-time low $674.10 (same page, per search) | **Keep.** Known specs: 3 kW, 120–450 V MPPT, 10 ms UPS transfer ([spec sheet](https://eg4electronics.com/wp-content/uploads/2024/05/EG4-3000EHV-48-Spec-Sheet.pdf)). No cheaper unit had confirmed specs and certification |
| ECO-WORTHY 3500 W 48 V AIO | $499.99 sale ([ECO-WORTHY](https://www.eco-worthy.com/products/3500w-solar-off-grid-inverter-charger-48v-dc-to-120v-ac-split-phase-power-inverter)) | Would save $200, but the listing's "4400W@150V PV input" suggests a low-voltage MPPT, and its transfer time and UL/ETL listing are **unverified**. Use it only after checking the datasheet |
| PowMr 3 kW 48 V | Listings found ([Amazon](https://www.amazon.com/PowMr-Solar-Inverter-3000W-48V/dp/B08BNBF8YB)); price and certification **unverified** | Not recommended without a confirmed listing |
| SRNE, Growatt, Sungold, MPP Solar, used Victron | No confirmed late-2026 US prices found | Not evaluated (**unverified**) |

### Build 1 power station

| Option | Price | Verdict |
|---|---:|---|
| BLUETTI Apex 300 new | $1,499 ([Outbound Power](https://outboundpower.com/products/bluetti-apex-300-portable-power-station-2764-8wh-3840w-2400w-solar-input)) | BUILDS.md pick |
| **BLUETTI Apex 300 restored/refurbished** | **$1,193.99** ([Walmart](https://www.walmart.com/ip/Restored-Bluetti-Apex-300-Expandable-Power-Station-2764-8Wh-Capacity-NEMA-TT-30-L14-50-AC-Car-Charge-for-Outdoor-Adventures-Refurbished/17739918627?conditionGroupCode=2)); $1,279–1,299 on eBay ([eBay](https://www.ebay.com/itm/376511915423), [Slickdeals](https://slickdeals.net/f/18702634-bluetti-apex-300-2764wh-portable-power-station-expandable-generator-certified-refurbished-free-200w-solar-panel-1279)); official refurb store ([BLUETTI](https://www.bluettipower.com/collections/refurbished-1)) | **Pick.** Same unit, $305 less. Warranty is shorter and seller-dependent (Walmart "Restored" terms **unverified**; eBay refurbs list 2 years via Allstate per search). BLUETTI's own refurb store gives better warranty if it has stock |

### Solar

| Panel | Price | $/W | Key specs | Use |
|---|---:|---:|---|---|
| **QCells 400 W 132 half-cell, new** | $140 ([SanTan Solar](https://www.santansolar.com/product/new-qcells-400w-black-mono-solar-panel/)) | $0.35 | Voc 45.3 V, Isc 11.14 A (same page) | **Pick for both builds** |
| BYD 400 W mono, used | $100 ([SanTan used panels](https://www.santansolar.com/product-category/used-solar-panels/), per search) | $0.25 | Guaranteed ≥80 % of rated output; Voc/Isc **unverified** | Cheapest option; check the label specs before buying |
| Lumina SolarSpace 405 W (BUILDS.md) | $145.80 ([survey](https://ecobusinesslinks.com/solar-panels-price-survey/)) | $0.36 | Voc ~37 V (**unverified**) | Replaced |

String math:

- **Build 1 (Apex 300, 12–60 V, 20 A per port):** one QCells per port. Cold Voc ≈ 45.3 × 1.1 = 50 V < 60 V ✅. Isc 11.14 A < 20 A ✅. Two panels in series (100 V cold) would exceed the limit, as in BUILDS.md.
- **Builds 2 and 3 (EG4, 120–450 V operating, 500 V max):** **4 QCells in series = 1,600 W.** Cold Voc ≈ 4 × 45.3 × 1.1 = 199 V < 500 V ✅. Vmp per panel ~37.7 V (**unverified**; typical for this Voc) → ~151 V at STC, and ~130 V at 65 °C cell temperature (−0.35 %/°C, **unverified** coefficient), which is still above 120 V. This is tighter than BUILDS.md's 5-panel string. If the datasheet Vmp comes in lower, add a fifth panel ($140).
- 1,600 W is already above the ~800–1,200 W target. Three panels in series (~113 V Vmp) would fall below the MPPT minimum, so four is the floor.

### DC protection and cable

The fuse and cable are sized to what the **inverter can draw**, not to the 1.2 kW load:

- EG4 max continuous DC: 3,000 W ÷ (44 V low battery × 0.92) ≈ **74 A** (as in BUILDS.md). At 1.2 kW it's ~30 A.
- NEC continuous rule: 1.25 × 74 A = **93 A**, so a **100 A** Class T fuse works (≥93 A).
- Cable: **2 AWG copper** (115 A at 75 °C, NEC 310.16) ≥ 100 A fuse ✅. 4 AWG (85 A) is *below* 93 A, so it doesn't pass at the inverter's rating. Keep 2 AWG.
- Surge: 2× for 5 s ≈ 150 A. A 100 A fast-acting Class T fuse may open on a full 5 s 150 A surge (check the Mersen time-current curve, **unverified**). In practice the battery's 50 A rating and 63 A breaker cap the current long before that, so the fuse is there for short circuits and its interrupt rating is what matters.
- Fuse: Mersen A6T100, 300 VDC, **100 kA DC interrupt** ($48.59, [AllFuses](https://www.allfuses.com/a6t100)). Holder: Blue Sea 5007100 (110–200 A rated; confirm it accepts a 100 A fuse, **unverified**) at ~$68–87 ([Off Grid Stores](https://offgridstores.com/products/blue-sea-5007100-class-t-fuse-block-w-insulating-cover-110-to-200a)). The generic Amazon Class T holders (~$20) carry no confirmed listing, so skip them.
- **External DC breaker dropped.** The ECO-WORTHY 50 Ah has a built-in 63 A breaker and power switch ([listing](https://www.ebay.com/itm/127963500931)), which serves as the battery disconnect. Keep the Class T fuse; the battery breaker's DC interrupt rating is **unverified**, so it doesn't replace the fuse.

### Build 3 charger

| Option | Price | Energy balance at 1.2 kW load (~1.3 kW DC) |
|---|---:|---|
| **Mean Well NPB-1200-48** (1,210 W, isolated, UL 62368-1) | $277 ([DigiKey](https://www.digikey.com/en/products/detail/mean-well-usa-inc/NPB-1200-48/22119131)) | Covers ~93 % of the load. Battery drains ~0.1 kW → 2.56 kWh lasts ~23 h with no sun. **Keep** |
| Mean Well NPB-750-48 (750 W) | $188.20 ([DigiKey](https://www.digikey.com/en/products/detail/mean-well-usa-inc/NPB-750-48/22119113), per search; stock **unverified**) | Deficit ~0.55 kW. 2.56 kWh lasts ~4.2 h, 5.12 kWh ~8.4 h. Saves $89 but needs the $330 bigger battery for overnight runs. **Not worth it** |

The NPB-1200's 18 A output plus the battery's 50 A limit also fit together well: the inverter pulls ~28 A, the charger supplies ~18–22 A of it, and the battery covers the rest plus spikes. No cheaper certified isolated 1.2 kW 48 V charger was found.

---

## Budget build 1: refurbished all-in-one, grounded site

| Component | Model | Key specs | Price | Cut vs BUILDS.md, and why it's OK |
|---|---|---|---:|---|
| Power station | BLUETTI Apex 300, restored | 2,764 Wh LFP, 3,840 W, 0 ms UPS (vendor claim) | $1,193.99 ([Walmart](https://www.walmart.com/ip/Restored-Bluetti-Apex-300-Expandable-Power-Station-2764-8Wh-Capacity-NEMA-TT-30-L14-50-AC-Car-Charge-for-Outdoor-Adventures-Refurbished/17739918627?conditionGroupCode=2)) | Refurbished instead of new (−$305). Same hardware and specs |
| Panels (×2) | QCells 400 W, new | Voc 45.3 V, Isc 11.14 A | $280 ([SanTan](https://www.santansolar.com/product/new-qcells-400w-black-mono-solar-panel/)) | Cheaper per watt (−$12). One per port, within the 60 V / 20 A limits |
| PV extension cables | 10 AWG MC4 | — | ~$40 (**unverified**) | Shorter run |
| **Total** | | | **~$1,515** | **Saves ~$325** |

Tradeoffs: shorter warranty than new (5 years), possible cosmetic wear, and refurb stock comes and goes. Test the 0 ms switchover under full GPU load before you trust it, same as BUILDS.md. Still needs a grounded receptacle.

## Budget build 2: component system, grounded site

| Component | Model | Key specs | Price | Cut vs BUILDS.md, and why it's OK |
|---|---|---|---:|---|
| Battery | ECO-WORTHY 48 V 50 Ah | 2.56 kWh, 50 A continuous, 63 A breaker | $469.99 ([eBay](https://www.ebay.com/itm/127963500931)) | Half the capacity (−$430). Load is 28 A DC, under the 50 A limit; ~1.8 h runtime, then the EG4 goes to grid bypass (10 ms) |
| Inverter/charger + MPPT | EG4 3000EHV-48 | 3 kW, 120–450 V MPPT, 10 ms transfer | $699.99 ([Current Connected](https://www.currentconnected.com/product/eg4-3000-ehv-48v-inverter-charger)) | Unchanged |
| Panels (×4, series) | QCells 400 W | 1,600 W, ~199 V cold Voc | $560 ([SanTan](https://www.santansolar.com/product/new-qcells-400w-black-mono-solar-panel/)) | 4 instead of 5 (−$169). Still above the 120 V MPPT floor (see string math) |
| Class T fuse | Mersen A6T100 | 100 A, 300 VDC, 100 kA | $48.59 ([AllFuses](https://www.allfuses.com/a6t100)) | 100 A instead of 125 A (−$21). Still ≥93 A (1.25 × 74 A) |
| Class T fuse block | Blue Sea 5007100 | 110–200 A block, cover | ~$68 ([Off Grid Stores](https://offgridstores.com/products/blue-sea-5007100-class-t-fuse-block-w-insulating-cover-110-to-200a)) | Unchanged |
| DC breaker | — | — | $0 | Dropped (−$50 plus enclosure). The battery's built-in breaker/switch is the disconnect |
| Busbars | — | — | $0 | Dropped (−$60–120). Only the inverter lands on the battery |
| Battery cables | 2 AWG, ~2 × 1.5 m, crimped lugs | 115 A at 75 °C | ~$60 (**unverified**) | Unchanged size |
| PV disconnect | 2-pole DC PV disconnect ≥250 VDC | — | ~$60 (**unverified**) | Kept (required) |
| PV wire / MC4 | 10 AWG | — | ~$40 (**unverified**) | Shorter run |
| **Total** | | | **~$2,010** (~$1,450 without panels) | **Saves ~$640–790** |

Tradeoffs: 1.8 h instead of 3.5 h of battery ride-through, and battery-only power is capped at ~2.5 kW by the 50 A BMS (not the inverter's 3 kW). The solar string has less voltage margin on hot days. For a confirmed UL 1973 battery with double the runtime, swap in the Cubix100 for +$330 (still $100 below BUILDS.md's WattCycle).

## Budget build 3: ungrounded site

Same parts as budget build 2, with the EG4's AC input left unconnected, plus:

| Component | Model | Price | Cut vs BUILDS.md, and why it's OK |
|---|---|---:|---|
| Build 2 parts (battery, EG4, 4 panels, fuse, block, cable, PV disconnect, PV wire) | As above | ~$2,005 | See build 2 |
| Mains charger | Mean Well NPB-1200-48 (isolated, UL 62368-1, Class I) | $277 ([DigiKey](https://www.digikey.com/en/products/detail/mean-well-usa-inc/NPB-1200-48/22119131)) | Unchanged. The 750 W model saves $89 but needs a bigger battery (see above) |
| Charger output fuse | 30–40 A DC-rated fuse/breaker, 10 AWG | ~$30 (**unverified**) | Unchanged |
| Busbars | Small pair, ≥100 A | ~$40 (**unverified**) | Smaller than BUILDS.md (−$20–80). Needed because the charger and inverter both land on the battery |
| Supply GFCI receptacle | Listed 15/20 A GFCI, "No Equipment Ground" label | ~$25 (**unverified**) | Unchanged |
| Output GFCI | Listed 20 A GFCI in a metal box on the inverter output | ~$25 (**unverified**) | Unchanged |
| Bonding bar + 6 AWG wire | — | ~$30 (**unverified**) | Unchanged |
| **Hardware total** | | **~$2,430** (~$1,870 without panels) | **Saves ~$670** |
| Electrician to install the supply GFCI | — | ~$90–200 ([HomeGuide](https://homeguide.com/costs/cost-to-ground-an-outlet-or-a-house), **unverified** locally) | Not counted in BUILDS.md either |

Tradeoffs: as in build 2, plus everything in BUILDS.md's build 3 still needs testing (output GFCI behavior with the EG4, NPB leakage vs the 5 mA supply GFCI). With the charger covering ~93 % of the load, the 2.56 kWh battery mainly handles spikes and charger dropouts, which suits this build well.

---

## Caveats

- **The 50 A BMS limit is the main new risk.** It's fine for 1.2 kW, but don't add load beyond ~2 kW without moving to the Cubix100 or a second pack.
- Sale and refurb prices (Walmart restored Apex 300, $469.99 battery, $699.99 EG4) may not last. Re-check before buying.
- Items marked **unverified** (cables, PV disconnect, GFCIs, bonding, electrician cost) are estimates; budget ~$100 of slack.
- Cheaper AIOs (ECO-WORTHY 3500 W, PowMr) were not adopted because their listings and transfer times couldn't be confirmed. Don't buy uncertified mains-connected gear to save $200.
