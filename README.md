# Interactive Simulator (Nutrition Planning)

**Author:** Gabriel Della Mattia
**Produced for:** AGMT2.TEAM

---

## What this is

A self‑contained web app to **plan fueling and glycogen management** for endurance cycling using **Critical Power (CP)** and **power‑duration (MMP)** data. It converts power and time into **mechanical → metabolic energy**, estimates **CHO needs**, proposes **dose schedules** within intestinal transport limits (SGLT1/GLUT5), and visualizes a **muscle glycogen curve** alongside the intake timeline.

> Decision rules and caps follow contemporary sports‑nutrition literature (ACSM/IOC/Jeukendrup et al.). See **References**.

---

## Quick start

1. Put this file structure anywhere (no server required):

   ```
   /project
     ├─ index.html        # the app (from canvas)
     └─ logo.png          # optional, shown as header icon + watermark
   ```
2. Open **`index.html`** in a browser.
3. (Optional) Click **Upload CSV** and load your power‑curve file (see formats below). The app estimates CP automatically (toggleable).

---

## Key features

* **Effort Projection**: duration (up to 6 h), target watts, and **gross efficiency** (18–25%) → kJ, kcal, kJ/kg.
* **Auto‑CP**: fits the 2‑parameter CP model *P = W'/t + CP* from your MMP (using 2–30 min windows) and classifies domain (**Endurance / Moderate / Severe**).
* **CHO Strategy**: select **G60 / GF90 / GF120** modes with caps at **≤60/≤90/≤120 g·h⁻¹**; choose cadence (10–20′) and start time (default **120′** reflecting “<2 h fueling generally optional”).
* **Timeline Chart**: intake bars + simulated **glycogen curve**; warnings when projected stores fall below reserve.
* **Handlebar Guide**: **Copy** and **Export TXT** of the minute‑stamped fueling schedule with G/F split.
* **Branding**: drop a **`logo.png`** next to the HTML to set header icon, favicon, watermark, and button badges.

---

## CSV formats

Two formats are accepted; durations < **60 s** are filtered out for clearer MMP:

**Wide (preferred)**

```
Date, m60, m120, m300, ... , m18000
2024-03-01, 330, 315, 290, ... , 220
```

* Columns named `m<seconds>`
* Any date string is accepted; used only for coverage info

**Long**

```
Date, Duration, Power
01/03/2024, m1800, 250
01/03/2024, 00:20:00, 280
```

* `Duration` accepts `m1800`, `s300`, `mm:ss`, `hh:mm:ss`, or tokens with `h/m/s` (e.g., `1h30m`)

---

## Modeling details

### Energy math

* **Mechanical energy**: $E_{mech} = (W \cdot t)/1000$ kJ.
* **Metabolic energy**: $E_{met} = E_{mech} / GE$ (GE = gross efficiency, slider 0.18–0.25). 1 kcal = 4.184 kJ.
  GE references and typical ranges reported in cyclists are summarized in Coyle (1992) and reviews (Jobson et al., 2012).

### CP estimation and domains

* Linear fit of $P = W'/t + CP$ using MMP points from **2–30 min**.
* Domains mapped from %CP: **Endurance <75%**, **Moderate 75–100%**, **Severe >100%** consistent with CP as the heavy/severe boundary (Poole & Jones reviews).

### CHO requirement

* Total **CHO need** estimates combine metabolic cost with an intensity‑dependent **CHO fraction** (heuristic informed by the **crossover concept**: CHO contribution rises as intensity approaches/exceeds LT/CP).
* **Intestinal caps** by mode: **G60 ≤60 g·h⁻¹** (glucose/SGLT1), **GF90 ≤90 g·h⁻¹** (add fructose/GLUT5, 2:1), **GF120 ≤120 g·h⁻¹** (1:1 in gut‑trained athletes).
* **Start rule**: for efforts **<2 h** fueling is generally **optional** unless intensity is high or glycogen is low; for longer efforts the schedule is shown and can be tuned.

### Glycogen curve

* Simulates minute‑by‑minute **endogenous CHO draw** = (CHO need − exogenous oxidation capped by mode).
* **Bolus absorption** modeled with an exponential time constant (τ ≈ 20 min) applied to each dose; display shows initial stores, reserve floor, and projected end.

> **Disclaimer**: outputs are educational planning aids; individual tolerance and oxidation rates vary. For elite protocols (e.g., >90–120 g·h⁻¹), supervised gut training and monitoring are advised.

---

## UI tips

* The overlay message hides automatically when duration ≥ 2 h.
* **Carbohydrate strategy** sits under the timeline to reduce scrolling; the chart height is compact (≈320 px).
* The **Copy** button falls back to TXT download if clipboard is unavailable.

---

## Roadmap

* Optional **FPCA of MMP** (PC1–PC3) as shape descriptors.
* Personal presets (weight, CP, stores).
* Print‑friendly **A6 PDF** export of the handlebar guide.
* Light/dark auto theme + accessibility passes.

---

## References

**Fueling amounts, transporters, and performance**

* Jeukendrup, A. (2014). *Carbohydrate Intake During Exercise*. **Sports Medicine**. Open‑access review summarizing 60 g·h⁻¹ glucose ceiling and >1 g·min⁻¹ with glucose+fructose (2:1). [https://pmc.ncbi.nlm.nih.gov/articles/PMC4008807/](https://pmc.ncbi.nlm.nih.gov/articles/PMC4008807/)
* Fuchs, C. J. et al. (2019). *Fructose co‑ingestion to increase carbohydrate availability*. **J Physiol** (review). Mechanisms for SGLT1/GLUT5 and performance benefits of mixed sugars. [https://pmc.ncbi.nlm.nih.gov/articles/PMC6852172/](https://pmc.ncbi.nlm.nih.gov/articles/PMC6852172/)
* Podlogar, T. et al. (2022). *Increased exogenous but unaltered endogenous CHO oxidation with 120 vs 90 g·h⁻¹*. **Med Sci Sports Exerc**. [https://pubmed.ncbi.nlm.nih.gov/35951130/](https://pubmed.ncbi.nlm.nih.gov/35951130/)  (OA summary: [https://pmc.ncbi.nlm.nih.gov/articles/PMC9560939/](https://pmc.ncbi.nlm.nih.gov/articles/PMC9560939/))
* Hearris, M. A. et al. (2022). *120 g·h⁻¹ across fluids/gels/chews shows comparable oxidation*. **J Appl Physiol** (13C). [https://journals.physiology.org/doi/abs/10.1152/japplphysiol.00091.2022](https://journals.physiology.org/doi/abs/10.1152/japplphysiol.00091.2022)

**When to start fueling**

* ACSM Position Stand / Joint statement (2016): **30–60 g·h⁻¹ for 1–2.5 h**, up to **90 g·h⁻¹** for longer events; <60 min usually not required. PDF: [https://drugfreesport.org.za/wp-content/uploads/2018/04/Position-stand-on-Nutrition-Athletic-Performance-ACSM-2016-1.pdf](https://drugfreesport.org.za/wp-content/uploads/2018/04/Position-stand-on-Nutrition-Athletic-Performance-ACSM-2016-1.pdf)
* IOC Consensus (2010/updates): higher intakes for very long events; practice “gut training.” [https://stillmed.olympic.org/media/Document%20Library/OlympicOrg/IOC/Who-We-Are/Commissions/Medical-and-Scientific-Commission/EN-IOC-Consensus-Statement-on-Sports-Nutrition-2010.pdf](https://stillmed.olympic.org/media/Document%20Library/OlympicOrg/IOC/Who-We-Are/Commissions/Medical-and-Scientific-Commission/EN-IOC-Consensus-Statement-on-Sports-Nutrition-2010.pdf)
* Vitale & Getzin (2019). *Nutrition and Supplement Update for the Endurance Athlete*. **Nutrients** (open access). Practical bands by duration. [https://www.mdpi.com/2072-6643/11/6/1289](https://www.mdpi.com/2072-6643/11/6/1289)

**Substrate use vs intensity (CHO fraction heuristic)**

* Brooks & Mercier (1994). *The crossover concept*. **J Appl Physiol**. [https://pubmed.ncbi.nlm.nih.gov/7928844/](https://pubmed.ncbi.nlm.nih.gov/7928844/)
* van Loon et al. (2001). *Effect of increasing intensity on muscle fuel selection*. **PNAS/Am J Physiol** (open access in PMC). [https://pmc.ncbi.nlm.nih.gov/articles/PMC2278845/](https://pmc.ncbi.nlm.nih.gov/articles/PMC2278845/)

**Critical Power & domains**

* Poole, D. C. (2016). *Critical Power: an important fatigue threshold*. **Exerc Sport Sci Rev**. [https://pmc.ncbi.nlm.nih.gov/articles/PMC5070974/](https://pmc.ncbi.nlm.nih.gov/articles/PMC5070974/)
* Jones, A. M. (2017). *The ‘Critical Power’ concept: applications to sport*. **J Physiol**. [https://pmc.ncbi.nlm.nih.gov/articles/PMC5371646/](https://pmc.ncbi.nlm.nih.gov/articles/PMC5371646/)

**Gross efficiency**

* Coyle, E. F. (1992). *Cycling efficiency and fiber type*. **J Appl Physiol**. [https://pubmed.ncbi.nlm.nih.gov/1501563/](https://pubmed.ncbi.nlm.nih.gov/1501563/)
* Jobson, S. A. et al. (2012). *Gross efficiency and cycling performance: a brief review*. **J Sci Cycling** (OA). [https://www.researchgate.net/publication/258122117](https://www.researchgate.net/publication/258122117)

**Background reading**

* Jeukendrup, A. (2004). *Carbohydrate intake during exercise and performance*. **Nutrition**. [https://www.sciencedirect.com/science/article/abs/pii/S0899900704001169](https://www.sciencedirect.com/science/article/abs/pii/S0899900704001169)

---

## Contributing

* Keep UI changes **responsive and compact**.
* Preserve element IDs used by JS (see inline tests at bottom of the HTML).
* Add unit tests for new calculations (simple asserts are fine in a `<script>` dev block).

## License

Choose what fits your repo (MIT/Apache‑2.0). If unsure, MIT is common for client‑side demos.

---

**Contact:** @gdellamattia — suggestions welcome (e.g., FPCA of MMP, athlete presets, PDF export).
