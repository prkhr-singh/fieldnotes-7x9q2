# Home Purchase — Property Criteria

**Last updated:** 5 May 2026

This is the central document for what makes a good property. It has two layers:
1. **Hard criteria** — non-negotiables and quick filters
2. **Scoring rubric** — the dimensions used for AI assessment and how they're weighted
3. **Personal learnings** — what in-person visits have revealed that wasn't in the original rubric

The third section is the feedback loop: every inspection adds to it, which then improves future AI assessments.

---

## 1. Hard Criteria (Non-Negotiables)

| Criterion | Rule |
|---|---|
| Property type | Freestanding house ONLY. "X/Y address" = unit/townhouse → reject immediately |
| Budget ceiling | $950,000 (FHBG cap — the only hard cliff) |
| Bedrooms | 3 minimum |
| Bathrooms | 2 minimum |
| Build year | 2000s or newer strongly preferred. 1970s–1990s: routinely small floor plates, poor layouts, asbestos risk → avoid unless exceptional |
| Land | Freestanding, own title, own street frontage — no shared driveways, no body corporate |

---

## 2. Scoring Rubric (AI Assessment Dimensions)

These are the dimensions used in `property-comparisons/comparison-matrix.csv` and individual `summary.json` files.

### Dimensions and Weights

| Dimension | Column in CSV | Weight | What it measures |
|---|---|---|---|
| Study / work area | `study_score` | **High** | Dedicated room or large retreat usable as home office. A 5.1×4.8m retreat scores 10. A bedroom-sized study scores 7. No study = 3–5. |
| Room spaciousness | `room_spaciousness_score` | **High** | Master bedroom area and feel. Also considers secondary bedrooms. Benchmark: master >15m² = good, >18m² = excellent. |
| Ensuite | `ensuite_score` | **High** | Size and quality of master ensuite. A large renovated ensuite = 9–10. Clean but compact = 6. Small/dated = 3–5. |
| Kitchen & storage | `kitchen_storage_score` | **High** | Kitchen storage depth: tall pantry, bench runs, overhead cabinetry, drawer count. Large functional kitchen = 9–10. |
| Shared bathroom | `shared_bathroom_score` | **Medium** | Whether a good bathroom is shared between non-master bedrooms. Important for guests/children. |
| Low maintenance outdoor | `low_maintenance_score` | **Medium** | Covered alfresco > open deck > large garden. Small paved courtyard with no lawn = high score. Large grass lawn = low score. |
| Light & orientation | `light_orientation_score` | **Medium** | North-facing main living. East light in kitchen/dining. |
| **Overall fit** | `overall_fit_score` | — | Weighted synthesis of all dimensions, 0–100. |

### Assessment Layer Notes

The comparison matrix now separates **pre-visit AI assessment**, **contract/legal watchpoints**, and **in-person qualitative assessment**.

| Layer | Columns / files | How to interpret it |
|---|---|---|
| AI pre-visit | `overall_fit_score`, dimension scores, `report.md`, `summary.json` | Useful for ranking before inspection, but should be treated as provisional. |
| Contract/legal | `legal_risk_score`, `contract_watchpoints`, Section 32/contract notes in `report.md` | Not a lifestyle score. Low score means "do not proceed without conveyancer clarity", even if the home looks good. |
| In-person | `in_person_visited`, `in_person_overall_score`, `in-person.md` | Highest-trust layer for room feel, light, storage, noise, build feel, and lifestyle fit. |

When both AI and in-person scores exist, the dashboard should prefer the in-person score for ranking while still showing the AI score for calibration.

### Scoring Notes

- Scores are set during AI assessment from floor plans, photos, and listing text.
- They should be **re-evaluated after in-person visits** — see Section 3 for deltas found so far.
- When a new criterion is added (Section 3), retrospectively score prior properties and add new columns to `comparison-matrix.csv`.

---

## 3. Personal Learnings from In-Person Visits

This section accumulates what in-person inspections reveal that listing data and photos cannot. It is the living feedback loop. Every entry here should eventually be reflected in the scoring rubric above.

### 3.1 Walk-in Robe Quality (discovered: 1 Buick Crescent, 2 May 2026)

**What we learned:** Walk-in robe quality matters significantly more than a built-in robe, and more than general room spaciousness suggests. At 1 Buick Crescent, the WIR was described as "the standout feature" — "itna space to chahiye aur kya chahiye." A large WIR eliminates the need for additional furniture storage, which changes how a room feels functionally.

**Rubric implication:** The current `room_spaciousness_score` partially captures this, but doesn't distinguish BIR from WIR. Future assessments should note robe type explicitly. If master has a WIR > ~4m², add +1 to room_spaciousness_score.

**Status:** Reflected informally in future assessments. Consider adding `storage_type` field to summary.json (WIR / BIR / walk-through).

---

### 3.2 Laundry Room Quality (discovered: 1 Buick Crescent, 2 May 2026)

**What we learned:** Laundry room was specifically and positively called out — one of the strongest functional spaces in the house. This was not an expected criterion, and it's not in the current rubric at all.

**Rubric implication:** Laundry quality is now a soft positive. A dedicated, well-sized laundry room adds modest but real value. A laundry cupboard or combined with a bathroom is neutral. No laundry space is a mild negative. Add `laundry_quality` note to future `summary.json` assessments.

**Status:** Not yet a scored column. Add when comparing properties where laundry is meaningfully different.

---

### 3.3 Covered vs Open Outdoor Space (discovered: Heidelberg Heights auction, 2 May 2026)

**What we learned:** An open deck or separate/detached outdoor seating area does not match their lifestyle. They confirmed they would not "naturally sit in that kind of rear area." Melbourne's outdoor season is ~180 usable days — an uncovered open deck scores heavily for 6 months and is unused the other 6. A covered alfresco with a heater (like Buick's) is genuinely year-round functional.

**Rubric implication:** The `low_maintenance_score` currently captures outdoor size and maintenance burden. It needs to distinguish: covered attached alfresco (9–10) > open attached deck (6–7) > detached outdoor seating area (4–5) > large lawn (3–4). Update scoring guidance accordingly.

**Status:** Incorporated into scoring guidance above. Apply to future assessments.

---

### 3.4 Multiple Living Zones — "Too Much Space" Risk (discovered: 1 Buick Crescent, 2 May 2026)

**What we learned:** Having both a large retreat (24.5m²) AND a separate lounge created slight confusion — felt like "too much space" that they might not use. This is not a dealbreaker but it's a data point. They are a couple/small family, not a large household.

**Rubric implication:** The `study_score` scores the retreat/study as a work zone. The lounge is separate. In future assessments, note when a house has 3+ distinct living zones — flag it as "assess whether all zones will be used" rather than treating more zones as automatically better.

**Status:** Awareness note only. No rubric change needed — scoring already handles this by looking at specific purposes, not raw zone count.

---

### 3.7 Bedroom Windows and Natural Light (discovered: Geelong/Highton session, 3 May 2026)

**What we learned:** Window size in bedrooms was called out as important for openness and light — specifically, larger windows made rooms feel more alive even when the room itself wasn't oversized.

**Rubric implication:** The `light_orientation_score` currently captures main living area orientation. For bedrooms, note window quality and size as a soft factor. A master with good-sized windows facing a reasonable direction scores better than the same dimensions with a small or poorly-placed window.

**Status:** Add as a watchpoint in inspection checklist (already partly there). Note in assessments when bedroom windows are unusually good or unusually poor.

---

### 3.8 Linen Storage (discovered: Geelong/Highton session, 3 May 2026)

**What we learned:** Linen storage was explicitly called out as a priority — alongside pantry, laundry, and general storage. The ideal is a dedicated walk-in linen cupboard. The current rubric captures kitchen storage but not linen/general household storage.

**Rubric implication:** Add `linen_storage` as a soft note in future `summary.json` assessments. A walk-in linen = positive. A linen shelf in a bathroom = neutral. No linen storage = mild negative.

**Status:** Add to inspection checklist as a specific question.

---

### 3.9 Suburb Character — "Enjoyable Walkability" vs Infrastructure Walkability (discovered: lifestyle audit, 3 May 2026)

**What we learned:** Walkability was defined more richly than proximity to a train station. Enjoyable walkability means: variation in streetscape, mature trees, people on the street, visual stimulation, dogs, trams, some retail — a place that you want to walk through, not just walk from. Car-dependent suburbs can have good infrastructure but still not feel pleasant to walk in.

**Rubric implication:** Suburb notes should distinguish between *functional connectivity* (train access, commute times) and *walkable character* (street-level enjoyability). These are independent. Mill Park scores well on connectivity and poorly on character. Heidelberg Heights scores well on both.

**Status:** Awareness criterion — not a scored dimension, but note in suburb sections and in-person assessments.

---

### 3.10 Mid-Century Style Preference / Modern Estate Aversion (discovered: lifestyle audit, 3 May 2026)

**What we learned:** One participant explicitly said "mujhe mid century chahiye" — a preference for mid-century or character homes over modern cookie-cutter builds. Both agreed that walking through identical homes in new estates felt emotionally unappealing. Character features — brick, distinct facades, fireplaces, visual variation — consistently drew more interest.

**Rubric implication:** This is a *suburb character* criterion more than a *property* criterion. New estate suburbs (Mill Park, Mernda, H&L packages generally) score lower on this dimension. Established suburbs (Heidelberg Heights, Bundoora, Highton) score higher. For the property itself, note exterior distinctiveness.

**Status:** Flag estate suburb character as a soft negative in suburb notes. Note facade/exterior distinctiveness when assessing properties.

---

### 3.11 Homebody Identity — House Quality Over Suburb Premium (discovered: lifestyle audit, 3 May 2026)

**What we learned:** Both described themselves as homebodies — they spend significant time at home relative to being out in the suburb. Consequence: house quality matters more than suburb connectivity premium. Paying extra for suburb attributes you won't use daily (restaurant density, café culture, frequent city access) is less rational for this household than paying for in-house quality (WIR, laundry, kitchen, study, light).

**Rubric implication:** This reweights the assessment framework. Features that affect daily lived experience at home (WIR, laundry, study, covered outdoor, natural light) should be weighted more heavily than suburb premium. A great house in a decent suburb beats a decent house in a great suburb.

**Most-used spaces ranked by frequency:**
1. Living room (TV, conversation, relaxing — daily)
2. Study (WFH, desk use — 3+ days/week)
3. Bathroom (daily)
4. Laundry (3×/week)
5. Kitchen (daily but threshold criterion, not differentiator)
6. Bedroom (sleeping/storage — adequate size + good windows is enough)
7. Outdoor (covered alfresco — weekend use)

**Status:** Apply this weighting when comparing properties. A property that scores high on the top-4 spaces is better than one that scores high on suburb amenity.

---

### 3.6 Walk-in Pantry (discovered: Carlisle Homes floor plan review, 3 May 2026)

**What we learned:** A walk-in pantry was called a "major positive" when reviewing the Carlisle Inspire Series floor plan. The pantry, combined with fridge space built into the layout and a generous bench run, made the kitchen feel genuinely functional rather than just passable. This is distinct from tall overhead cabinetry — it's dedicated, organised, walk-in dry-goods storage.

**Rubric implication:** The `kitchen_storage_score` currently captures overall kitchen storage. For future assessments, specifically note whether a walk-in pantry exists vs. just overhead/under-bench cabinetry. Add +1 to `kitchen_storage_score` when a walk-in pantry is present. Absence is neutral; presence is a positive.

**Status:** Add `walk_in_pantry: yes/no` field to future `summary.json` assessments.

---

### 3.5 Under-Stair Storage (discovered: 1 Buick Crescent, 2 May 2026)

**What we learned:** Under-stair storage exists at 1 Buick Crescent but was closed on the day — not inspected. This is a soft positive that doesn't appear in photos or listings.

**Rubric implication:** Flag "under-stair storage" in `summary.json` watchpoints when a two-storey property has stairs in the main living area. Should be physically checked at inspection.

---

### 3.12 Tile Quality as Immediate Photo-Based Rejection Trigger (discovered: 5 May 2026 browsing session)

**What we learned:** Tile quality became a repeated rejection trigger across multiple listings in one session. Homes with dated, dirty-looking, or mismatched tiles were dismissed before any other feature was considered. Poor tiles create an instant "cheap" impression even if the exterior looks acceptable.

**Rubric implication:** When assessing from photos, treat tile quality as an early-pass filter: good tiles (neutral, clean, modern) = proceed; dated or visually dirty tiles = flag as significant negative. A home requiring full re-tiling of kitchen, bathroom, or laundry should have ~$15–25k added to the renovation burden estimate.

**Status:** Add as an explicit photo-screening note. Poor tile quality downgrades `kitchen_storage_score` and `ensuite_score` by 1–2 points.

---

### 3.13 Black / Dark Bathroom Tiles — Explicit Aversion (discovered: 5 May 2026)

**What we learned:** One participant raised a strong objection to black or very dark bathroom tiles based on past experience in Chennai — water splash marks and soap residue are permanently visible on dark surfaces. Dark bathrooms also feel smaller and less pleasant. This is not a mild preference — it's a firm negative.

**Rubric implication:** Any bathroom with predominantly black or dark-coloured tiles scores -2 on `ensuite_score` / `shared_bathroom_score`. Flag explicitly in photo assessment notes. Check in person if photos are ambiguous.

**Status:** Add to inspection checklist: "Tile colour in all bathrooms — note if black/very dark."

---

### 3.14 Renovation Burden as Cost Burden (discovered: 5 May 2026 browsing session)

**What we learned:** Multiple cheaper homes were rejected because they required "quite a lot of work." A lower asking price does not offset renovation costs — it just shifts where the money goes. Homes needing new kitchen, new flooring, and new bathrooms add $40–80k to the effective cost while also adding months of disruption.

**Rubric implication:** When a listing is priced $50–80k below comparable homes, this should trigger a renovation cost estimate, not excitement. If the renovation to bring it to an acceptable standard costs more than the price gap, it is not a bargain. Note estimated renovation burden in `summary.json` as `reno_burden_estimate_aud`.

**Status:** Apply when assessing "affordable" listings. If reno burden closes the price gap, score accordingly.

---

### 3.15 Built-Up Floor Area — 315m² or Less Likely Too Cramped (discovered: 5 May 2026)

**What we learned:** During the browsing session, homes around 315m² total floor area were repeatedly described as feeling "too cramped." This is not a hard filter but a useful screening signal, especially for double-storey homes where the floor plate is divided across two levels.

**Rubric implication:** For double-storey homes, flag total floor area ≤315m² as a soft negative. Below 280m² is a strong negative unless the layout is exceptionally efficient. Target 320m²+ for comfortable feel across both floors.

**Status:** Add floor area check to photo-screening step. Note in `summary.json` as `total_floor_area_sqm`.

---

### 3.16 Outdoor Deck → Sunroom Conversion Preference (reinforced: 5 May 2026)

**What we learned:** One participant explicitly said they would prefer to convert an outdoor deck into a sunroom. This reinforces the prior finding (3.3) about covered vs open outdoor space — but goes further. An open deck is not just suboptimal, it's something they'd want to change. The conversion cost (~$15–25k for a basic enclosure) should be factored in when a property has only an open deck as its outdoor offering.

**Rubric implication:** When `low_maintenance_score` is being set: open deck → score 5–6, AND note "sunroom conversion likely desired, add ~$15k to reno budget." Covered alfresco remains the ideal (score 8–9). Already-enclosed sunroom = 9.

**Status:** Apply to future assessments. Reinforce in inspection checklist.

---

| Date | Inspection | New criterion / change | Action taken |
|---|---|---|---|
| 2 May 2026 | 1 Buick Crescent | Walk-in robe quality matters more than expected | Note in rubric guidance; add to summary.json storage_type |
| 2 May 2026 | 1 Buick Crescent | Laundry room quality is a real criterion | Add laundry_quality note to future assessments |
| 2 May 2026 | Heidelberg Heights auction | Covered alfresco >> open deck (~180 day Melbourne outdoor season) | Updated low_maintenance scoring guidance |
| 2 May 2026 | 1 Buick Crescent | Multiple living zones can feel excessive for small household | Awareness note — not a rubric change |
| 2 May 2026 | 1 Buick Crescent | Under-stair storage: soft positive, check in person | Add to watchpoints for two-storey properties |
| 3 May 2026 | Carlisle Homes floor plan review | Walk-in pantry is a major positive — distinct from overhead cabinetry | Add walk_in_pantry field to summary.json; +1 to kitchen_storage_score |
| 3 May 2026 | Geelong/Highton browsing session | Bedroom windows for light and openness matter — not just room size | Note window quality in future assessments |
| 3 May 2026 | Geelong/Highton browsing session | Linen storage is an explicit priority — specifically called out as important | Flag linen storage presence in assessments |
| 3 May 2026 | Geelong/Highton browsing session | "Enjoyable walkability" ≠ infrastructure walkability. Means trees, variation, people, visual stimulation | Suburb assessment should note walkable character, not just train distance |
| 3 May 2026 | Geelong/Highton browsing session | Wife prefers mid-century character ("mujhe mid century chahiye"). Cookie-cutter modern estates = emotional negative | Flag estate suburb uniformity as a mild negative in suburb notes |
| 3 May 2026 | Geelong/Highton browsing session | Homebody identity confirmed — house quality > suburb premium for their lifestyle | Paying for in-house quality (WIR, laundry, study, light) is more justified than paying for suburb connective premium they won't use daily |
| 3 May 2026 | Lifestyle audit | Bedrooms primarily for sleeping + storage, not activity zones — don't need to be oversized | Master needs adequate size + good windows + storage. Spaciousness threshold only, not premium. |
| 5 May 2026 | Multi-suburb browsing session | Tile quality = immediate photo rejection trigger. Dated/dirty tiles create "cheap" impression instantly | Downgrade kitchen + bathroom scores 1–2 pts for poor tiles |
| 5 May 2026 | Multi-suburb browsing session | Black/dark bathroom tiles = explicit aversion. Water splash marks visible permanently (Chennai experience) | -2 on ensuite/bathroom scores. Flag in every photo assessment |
| 5 May 2026 | Multi-suburb browsing session | Renovation burden = cost burden. Low price + high reno need is not a bargain — it just shifts where the money goes | Add `reno_burden_estimate_aud` to summary.json |
| 5 May 2026 | Multi-suburb browsing session | 315m² total floor area = likely too cramped for double-storey. Target 320m²+ | Add `total_floor_area_sqm` to summary.json; flag ≤315m² |
| 5 May 2026 | Multi-suburb browsing session | Open deck → one participant explicitly wants to convert to sunroom. Add ~$15k conversion to reno budget | Score open deck 5–6 AND add reno note. Covered alfresco still ideal (8–9). |

---

## 4. Property Preferences Summary

Condensed from sessions and in-person visits. Use to quickly filter new listings.

### Actively Look For

- Double storey with large upstairs retreat/study — this is the preferred floor plan type
- Bigger built-up area (total floor plate) over big land — large home on compact block beats small home on big block
- Walk-in robe in master bedroom
- Covered alfresco or courtyard (not an open exposed deck)
- Good laundry room (separate, well-sized)
- FTTP NBN — strongly preferred for WFH reliability
- Walking distance to train station (Mernda or Hurstbridge line preferred)
- Compact, manageable land (~300–550m²)

### Actively Filters Out

- "X/Y address" — unit/townhouse. Always reject.
- 1970s–1980s single-storey homes — small floor plates (120–145m²), single garage, cramped layouts, asbestos risk
- Very large land (660m²+) with a modest house — maintenance burden without proportional benefit
- Suburbs where wife's commute to Fitzroy + Bundoora is 85+ min each way
- Separate, detached outdoor seating areas (personal lifestyle mismatch confirmed in person)
- Open decks without cover — limited to ~180 usable days

### Things That Sound Good But Are Lower Priority

- Huge backyards / large lawns — maintenance burden, not valued
- 4-bedroom label on *established homes* — pays a $120–180k premium; 3-bed + large study/rumpus gives the same functional rooms. **Exception:** for H&L new builds, 4 bedrooms is the explicit target — study nooks in spec builds are small (2×2m), not a genuine home office replacement.
- Development potential / large land ratio — they're buying a home to live in, not develop

---

## 5. Inspection Checklist (Use At Every Property Visit)

### Structural

- [ ] Cracks in walls/ceilings — diagonal = structural movement
- [ ] Water stains on ceilings and near ground (leaks, damp)
- [ ] Under sinks — staining or soft cabinetry
- [ ] Every window and door — sticking indicates movement
- [ ] Every tap and toilet — water pressure, leaks
- [ ] Roof visible from outside — sagging, damaged tiles, gutters

### Fittings and Systems

- [ ] Hot water system age (>10 years = replacement risk, ~$2–3k)
- [ ] Electrical switchboard — old ceramic fuse boards need replacing
- [ ] Heating/cooling: ducted gas vs split systems vs evaporative
- [ ] NBN connection type (FTTP preferred) — check nbnco.com.au

### Property-Specific

- [ ] Walk-in robe — how big? Could you fit a chest of drawers inside?
- [ ] Laundry room — separate room or cupboard? Size and fit-out?
- [ ] Under-stair storage — open it and look inside
- [ ] Covered alfresco — what direction does it face? Is there a heater?
- [ ] Which direction does main living face? (North = better light, lower heating costs)
- [ ] Total built-up area (both floors) relative to price — target >200m² for double-storey

### Questions to Ask Agent

- Why are vendors selling? How long have they owned it?
- Has this property been on market before?
- What settlement period does the vendor prefer?
- Are there any known defects?
- What's included? (appliances, blinds, light fittings)
- **"Can I get a copy of the Section 32 and contract?"** — signals serious buyer intent
- For target properties: **"Is the vendor open to pre-auction offers? What price would make that conversation worth having?"**

### For 1970s–1980s Homes Only

- [ ] Asbestos risk areas: eaves, laundry, internal walls, window reveals
- [ ] Foundation type — slab vs stumps; check for subsidence
