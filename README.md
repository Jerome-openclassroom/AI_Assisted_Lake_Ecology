# 🌿 Lake NPP Modeling – Empirical Bioenergetics with Field & AI Support

This repository presents a practical and biologically realistic estimation of **Net Primary Production (NPP)** in a natural lake ecosystem using:

- Real-world field measurements (light, turbidity, depth, temperature, pH, KH),
- A physically-based irradiance integration model (Python),
- An empirical ecosystem correction factor,
- And interpretative support by **GPT-4o** (OpenAI), enhancing ecological reasoning and dynamic parameter tuning.

---

## 🧪 Methodology Overview

### 1. **Field Protocol & Equipment**

Measurements were taken from two sites: **Lake du Paty** (altitude ~400 m, surface 3.5 ha, max depth ~20 m) and **the Ouvèze River**.

Parameters measured **on-site**:
- pH, KH, NO₃⁻/NO₂⁻, GH via **JBL 7-in-1 test strips**
- Water temperature (digital thermometer)
- Visual turbidity (sunlight and suspended particles)
- Sampling was done using **a custom-built pole**, **boots**, and **a cold-preserved thermos** for turbidimetry.

Turbidity was then analyzed **at home** using a custom low-cost photometric system (see companion repository [📎 TurbiditySensor_OpenScience](https://github.com/Jerome-openclassroom/TurbiditySensor_OpenScience)).

1.1 Ecological Field Data (Lake du Paty – June 2025)
The following parameters were measured directly on-site under clear sky conditions at midday:

- pH: 7.5–8.0

- KH (carbonate hardness): 6 °dKH

- GH (general hardness): 14–21 °dGH

- Nitrate/Nitrite: 0 mg/L (undetectable)

- Water temperature: 22°C

- Air temperature: min 16°C / max 26°C

- Estimated CO₂ concentration (from pH and KH chart): 2–5 mg/L

- Turbidity (JTU): < 5 (confirmed post-transport via calibrated optical sensor)

- Observed biodiversity: numerous alevins, two species of odonates (anisoptera), several lepidoptera

These values confirm a clear, oxygenated, weakly alkaline freshwater body, with low nutrient content and high photic zone depth — matching mesotrophic to oligotrophic profiles.


1.2 Ecological Field Data (Ouvèze River – June 2025, Bédarrides site)
Measurements were taken a few meters downstream of a small cascade, under optimal solar exposure:

- pH: 7.5–8.0

- KH (carbonate hardness): 15 °dKH

- GH (general hardness): estimated < 21 °dGH

- Nitrate/Nitrite: 0 mg/L (undetectable)

- Water temperature: 20°C

- Estimated current speed: 0.5–1.0 m/s (visual estimation)

- Estimated CO₂ concentration (from pH and KH): 5–11 mg/L

- Turbidity (JTU): < 5 (confirmed via calibrated turbidimeter after gentle remixing)

- Biodiversity indicators: clear water, presence of fish, high oxygenation due to upstream cascade

These data indicate a carbonate-buffered, oxygen-rich stream with extremely low nutrient levels and high water clarity, ideal for light penetration and consistent with low turbidity high-flow environments.

---
### 📊 Additional water chemistry (Ouvèze River – June 21, 2025)

- **Sample:** Downstream of the cascade, 15:30 local time
- **Air temperature:** 35 °C
- **Water temperature:** 20.8 °C
- **Visual turbidity:** < 5 JTU (clear)
- **pH:** 8.1 (carbonate-buffered)
- **Total hardness (GH):** 13 °dGH (23 °fH) ≈ 283 mg/L HCO₃⁻
- **Alkalinity (KH, assumed):** ~13 °dKH
- **Nitrates/Nitrites:** 0 mg/L
- **Dissolved oxygen (O₂):** ~8 mg/L (±1)

**🧮 CO₂ calculation:**

\[
CO₂ (mg/L) = 0.44 × KH × 10^{(7.7 − pH)}
\]

\[
= 0.44 × 13 × 10^{(7.7 − 8.1)}
= 0.44 × 13 × 10^{-0.4}
≈ 0.44 × 13 × 0.398 ≈ 2.3 mg/L
\]

**✅ Interpretation:**
- The calculated free CO₂ is about **2.3 mg/L**, which is low compared to the dissolved O₂ (~8 mg/L).
- This reflects excellent aeration due to the cascade and turbulent flow: CO₂ degasses easily while O₂ stays close to saturation.
- Combined with very low nitrates and clear water (<5 JTU), this confirms the Ouvèze is **oligotrophic to mesotrophic**, with minimal organic pollution.
- Numerous anisopteran and zygopteran dragonflies observed along the banks further support high water quality and strong dissolved oxygen levels.

**➡️ Implication for NPP modeling:**  
High oxygen, low CO₂, and clear water mean deep light penetration and low shading from suspended solids, favouring primary productivity but within the constraints of low nutrient levels.

---

### 2. **Python Model Description**

The NPP model integrates:

- **Hourly irradiance** over 24h (bell-shaped solar curve)
- **Light attenuation** using measured turbidity-derived `k` values
- **Photosynthetic efficiency** and energy-to-mass conversion via enthalpy of cellulose
- **Autotrophic respiration losses**

A **correction factor `f = 1/300`** was introduced to translate ideal daily production into a realistic annual average, accounting for:

- Mortality and senescence
- Zooplankton grazing
- Respiration and trophic recycling
- Seasonal variation and nutrient constraints

---

## 🔬 Outputs (Lac du Paty)

- **Daily NPP (ideal theoretical)**: ~93.05 g/m²/day
- **Corrected annual NPP** (f = 1/300): ~113.2 g/m²/year
- Consistent with mesotrophic lake ecosystems

---

### 📊 Additional water chemistry (Lac du Paty – June 24, 2025)

- **Sample:** North shore, midday, clear sky, light breeze, 17:00 local time
- **Air temperature (50 cm above water):** 32.6 °C
- **Reference air temperature (Carpentras):** 35.9 °C (source: [Infoclimat](https://www.infoclimat.fr/))
- **Water temperature (20–30 cm depth):** 28.1 °C
- **Turbidity:** < 5 JTU (very clear)
- **pH:** 8.0 (moderately alkaline)
- **Alkalinity (KH):** 10 °dKH (17.8 °fH) ≈ 218 mg/L HCO₃⁻ ≈ 3.6 mmol/L
- **Nitrates/Nitrites:** 0 mg/L (undetectable)
- **Dissolved oxygen (O₂):** 6 mg/L
- **Estimated free CO₂:** ~2.2 mg/L (calculated: 0.44 × KH × 10^(7.7 – pH))
- **O₂/CO₂ ratio (mass basis):** ~2.7
- **Observed biodiversity:** abundant alevins, large anisopteran dragonflies (*likely Anax imperator*), freshwater jellyfish (*Craspedacusta sowerbii*)

**✅ Interpretation:**  
- Very low turbidity confirms excellent light penetration and low suspended solids.  
- pH and KH indicate stable carbonate buffering with weakly basic conditions.  
- Low CO₂ and moderate O₂ reflect good photosynthetic activity despite warm water temperature.  
- Biological observations (odonates, alevins, jellyfish) reinforce the diagnosis of a well-balanced mesotrophic lake with stable summer stratification.

---

### 🌿 Ecological Comparison – Lac du Paty vs Ouvèze (June 2025)

| Parameter | **🌊 Lac du Paty** | **🌊 Ouvèze River** |
|-----------|---------------------|---------------------|
| **Water Temp.** | ~28 °C (surface) | ~20–21 °C |
| **Dissolved O₂** | ~6 mg/L | ~8 mg/L |
| **pH** | ~8.0 | ~8.0 |
| **KH** | ~10 °dKH | ~13–15 °dKH |
| **CO₂** | ~2.2 mg/L | ~2.3 mg/L |
| **Turbidity** | < 5 JTU | < 5 JTU |
| **Biodiversity** | 🐉 Dominance of *Anax imperator* (top predator) reduces other odonates | 🪰 Higher odonate diversity due to flow and turbulence |

**✅ Key points:**  
- The river stays cooler and more oxygenated thanks to upstream cascades and steady current.  
- Both sites have stable carbonate buffering, very low CO₂ and excellent clarity.  
- In the lake, the large dragonfly (*Anax imperator*) predates on other odonates, lowering species diversity compared to the river’s richer community.


---

## 📈 Diagram – Model Flow

![Conceptual Flow](pictures/diagram.png)

---

## 🖼️ Field Reference – Ecological Clarity

The lake's turquoise to green coloration suggests high clarity and photic penetration, consistent with the very low turbidity measurements (4–5 JTU).

![Lake View](pictures/Lake_2.JPG)

---

## 🤖 Role of AI (GPT-4o)

The **model construction, parameter tuning**, and **ecological interpretation** were all co-developed in real time using **GPT-4o**.

This includes:
- Light integration modeling,
- Calculation of euphotic depth (`Zeu`) from turbidity,
- Bioenergetic correction and annual scaling,
- Ecological reasoning and trophic interpretation (predation, respiration),
- Visualization and repository documentation.


## 🧬 Accidental Proto-IBGN – June 26, 2025 (Ouvèze, upstream oxbow, i.e., stagnant side-arm before cascade oxygenation)

On **June 26, 2025**, around **17:00**, a **non-intentional biological sampling** occurred during sediment collection in a **shallow, stagnant side-arm (10 cm depth)** of the Ouvèze River. Air temperature was high (~35 °C), water was still, exposed to full sun, and likely already **severely oxygen-depleted**.

🫴 The larva was **collected manually (no net)** by scraping under submerged rounded stones.  
🫙 It was placed in a **glass container**, cooled during transport, and remained **alive upon arrival** at home.

---

### 🧪 Water parameters (after 24h in pot, June 27):

- 🌡️ **Temperature:** 27.8 °C  
- 🫧 **Dissolved oxygen:** 2–3 mg/L (JBL test)  
- 🔄 **Oxygenation attempt:** Manual stirring at time of capture – unsuccessful  
- 💀 **Status after 24h:** Dead

---

### 🔬 Morphological diagnosis (loupe ×5 & microscope ×40)

- Body length ~10 mm
- 🦠 **3 long caudal filaments** (cerci)
- 🫁 **Abdominal lamellar gills**, visibly palmate
- 🔬 Lateral compound eyes, fusiform body
- No case (excludes Trichoptera), no claws/hooks (excludes Odonata)

🧭 **Conclusion:**  
Probable **_Ephemeroptera_**, family **Baetidae**, genus **_Cloeon_**  
↳ Tolerant species, still **moderately rheophilic and steno-thermal**  
↳ Likely mortality due to **thermal + oxygen stress**


---

### 🌿 IBGN-like interpretation

| Variable | Value |
|----------|-------|
| Depth | ~10 cm |
| Flow | None (oxbow) |
| Exposure | Full sun |
| Temp (after 24h) | 27.8 °C |
| Dissolved O₂ | 2–3 mg/L |
| Diversity | 1 sensitive macroinvertebrate |
| IBGN estimate | Class 4–5 / 10 (low/moderate quality) |

🧪 This **accidental yet complete observation** demonstrates that **citizen science + minimal tools + LLM assistance** can yield **valid ecological insights** comparable to formal protocols.

---


---

## 📂 Repository Structure

```
📁 /
├── README.md              ← This file 
├── README_fr.md           ← French version

📁 pictures/
├── diagram.jpg            ← NPP conceptual diagram
├── Lake_1.jpg
├── Lake_2.jpg             ← illustrative coloration of the lake
├── River_1.jpg
├── River_2.jpg
├── Sampling.jpg           ← sampling on site
├── Tools.jpg              ← field material used
│
├── ephemeroptera_larvae/  ← accidental IBGN observation (June 26, 2025)
│   ├── ephemeroptera_ouvèze_squares_5mm.jpg    ← overview of larva on 5 mm grid (loupe ×5)
│   ├── double_magnifier_5X.jpg                 ← educational magnifier used for capture & observation
│   ├── branchies_X40.jpg                       ← abdominal lamellar gills (×40)
│   ├── cerques_caudales_X40.jpg                ← caudal filaments (cerci) detail (×40)
│   ├── pattes_X40_1.jpg                        ← anterior leg detail (×40)
│   ├── tête_thorax_X40_1.jpg                   ← head + thorax morphology (×40)
│   ├── thorax_abdomen_X40.jpg                  ← thorax–abdomen transition (×40)
│   ├── X40_optimal_4.jpg                       ← scale reference: 1 mm grid under ×40 microscope (for ImageJ free software https://imagej.net/)
│   └── README_fragment.md                      ← metadata and cognitive interpretation

```


🔗 **See also my related work on GitHub**:
## 🔗 Part of the Lyra Ecosystem

- [Lyra_Leaf_SPAD_Calibration](https://github.com/Jerome-openclassroom/Lyra_Leaf_SPAD_Calibration) – SPAD sensor calibration for estimating chlorophyll density in leaves.
- [Lyra_LowCost_Soil_Leaf](https://github.com/Jerome-openclassroom/Lyra_LowCost_Soil_Leaf) – Integrated low-cost soil and leaf model for terrestrial primary productivity.
- [Leaf_Chlorose_CNN_Training](https://github.com/Jerome-openclassroom/Leaf_Chlorose_CNN_Training) – CNN-based classification of chlorotic vs. healthy leaves from scanned images.
- [Lyra_DO_Green_Mesurim](https://github.com/Jerome-openclassroom/Lyra_DO_Green_Mesurim) - A low-tech protocol combining MesurimPro and ImageJ to estimate chlorophyll levels from scanned leaves, with validation against SPAD readings and AI-assisted correlation analysis.
- [TurbiditySensor_OpenScience](https://github.com/Jerome-openclassroom/TurbiditySensor_OpenScience) - A low-cost optical turbidity sensor calibrated in JTU, illustrating an open science approach for accessible, field-based water quality monitoring.
- [LimonTree_NPP_Model](https://github.com/Jerome-openclassroom/LimonTree_NPP_Model) — Low-cost water and NPP model for a potted lemon tree.
- [Mountain_Bocage_Soil_Analysis](https://github.com/Jerome-openclassroom/Mountain_Bocage_Soil_Analysis) — Complete soil dataset for a mid-mountain bocage site (Haute-Loire, France): texture, CEC, pH, nitrates, structural stability, and Berlèse funnel microfauna extraction. Ready for AI-assisted ecological modeling.
- [Lyra_Sentinel_MODIS_Site_HauteLoire](https://github.com/Jerome-openclassroom/Lyra_Sentinel_MODIS_Site_HauteLoire) — Combined Sentinel-2 NDVI and MODIS LST for a semi-natural site in Haute-Loire (France): ROI clipping, clean map, and reproducible notebook. Ready for AI-assisted ecological modeling.
- [Eco_Profile_Saint_Julien_1060](https://github.com/Jerome-openclassroom/eco-profile-saint-julien-1060) — 1-ha eco-climatological site (1060 m, Massif Central) with 2017–2018 records and multi-disciplinary field data.
- For field-based plant identification and LLM-assisted inference of ecological co-occurrence:  
[**Lyra_Botanical_Protocol**](https://github.com/Jerome-openclassroom/lyra-botanical-protocol) —  
*Protocol for probabilistic GPT-based species identification from photos and context (southern France, spring flora).*




## 📜 License

This project is open science. You are free to reuse, adapt, and expand it under the terms of the MIT License.
