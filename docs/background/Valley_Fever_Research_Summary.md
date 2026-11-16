# Valley Fever — Research Summary (Q1–Q3)
*Prepared for the FSE 570 capstone team*

---

## 1. What is Valley Fever & how does it spread?

**Valley Fever** (medical name: *coccidioidomycosis*, or "cocci") is a lung infection caused by breathing in spores of the fungus *Coccidioides*, which lives in soil in parts of the southwestern U.S.

- **Two species** cause it: *C. immitis* (mainly California) and *C. posadasii* (Arizona and elsewhere) — both cause identical illness.
- **How you get it**: soil/dust containing spores gets disturbed (wind, digging, construction) and the spores are inhaled. It is **not contagious** person-to-person.
- **Incubation**: symptoms appear 1–3 weeks after exposure.
- **Severity spectrum**:
  - ~50–65% of infected people have no symptoms or mild symptoms and never seek care.
  - Symptomatic cases look flu-like: cough, fever, shortness of breath, headache, fatigue, body aches.
  - ~1% of diagnosed cases progress to *disseminated disease* (spreads beyond the lungs) — about a third of those are fatal.
  - Average case duration: ~120 days; average cost per case: ~$93,700 (direct + indirect).
- **Higher-risk groups**: older adults, pregnant individuals, people with weakened immune systems (HIV/AIDS, immunosuppressive drugs), and people with diabetes.
- **Higher-exposure occupations**: construction, agriculture, excavation, archaeology/geology, wildland firefighting.

---

## 2. How does it affect Arizona?

- The U.S. had roughly **17,500 reported cases in 2022**, concentrated overwhelmingly in **California and Arizona**.
- Arizona alone reported about **14,787 cases in 2024**.
- Geographic concentration: **Maricopa, Pima, and Pinal counties** account for the large majority of Arizona's cases.
- Health-system impact: in Phoenix and Tucson, Valley Fever is estimated to cause **up to 30% of community-acquired pneumonia cases** — a significant public health burden, not a rare disease.
- Trend: incidence has been **rising over the past two decades**, roughly doubling between 2005 and 2022.

---

## 3. What causes outbreaks? (The "wet-dry cycle")

*Coccidioides* is a **dimorphic fungus** — it changes form depending on soil moisture:

1. **Rainy period** → fungus grows in soil as *mycelia* (mold), least infectious form.
2. **Dry period follows** (typically late summer/early fall) → mycelia convert into tough, dormant *arthrospores* that can survive in soil for months to years.
3. **Soil disturbance** (wind, dust storms, construction, excavation) → arthrospores break apart into airborne *arthroconidia* — the actual infectious particles.
4. **Inhalation** → infection.

**Outbreak trigger chain:**
`Rain → fungal growth → drying → wind/soil disturbance → airborne spores → inhalation → cases`

**Why this matters for our project**: this is a well-documented biological mechanism, not a guess — it directly justifies using **rainfall (lagged), soil temperature/moisture, wind, and dust/PM10 levels** as predictive features. The lag between rainfall and a resulting case spike is a key modeling decision — published estimates vary, so we should confirm the specific lag length once the "prior research" group reports back.

---

### Sources referenced
CDC, Mayo Clinic, Cleveland Clinic, OSHA, Medscape, and peer-reviewed research on *Coccidioides* biology and Arizona surveillance trends.
