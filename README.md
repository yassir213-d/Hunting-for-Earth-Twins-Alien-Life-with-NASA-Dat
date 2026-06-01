# 🌌 Searching for Earth Twins & Intelligent Life Using NASA Data

## 🎯 Project Mission
Are we alone in the universe? This project answers this fundamental human question using data science. It establishes a direct pipeline to the **NASA Exoplanet Archive API** to scan the cosmos, ingest atmospheric data from confirmed exoplanets, and apply strict astrophysical criteria to detect rocky planets residing within the **Habitable "Goldilocks" Zone**.

The pipeline filters high-volume astronomical data and applies a simplified **Drake Equation** framework to evaluate the statistical probability of extraterrestrial intelligent civilizations.

---

## 🛠️ Astrophysical Criteria & Filtering Logic
The pipeline extracts real-time data from the `pscomppars` table and enforces strict scientific boundaries to isolate true Earth twins:

1. **Planetary Composition (`pl_rade`):** Restricts the planet's radius between **0.5 and 1.5 Earth Radii**. This ensures the planet is structurally rocky (like Earth or Mars) rather than a gas giant (like Jupiter).
2. **Thermal Equilibrium (`pl_eqt`):** Restricts the equilibrium temperature between **180K and 310K**. This crucial window allows for the potential existence of liquid water and a sustainable atmosphere.
3. **Data Integrity:** Automates the elimination of noisy observations or missing critical parameters (`dropna`) to guarantee high-fidelity statistical profiling.

---

## 💻 Tech Stack & Architecture
* **Data Source:** Live API query to NASA Exoplanet Archive (Caltech/IPAC).
* **Languages & Libraries:** Python, Pandas, NumPy.
* **Statistical Modeling:** Implemented a predictive heuristic based on the Drake Equation assumptions (assessing habitable zones, biological development probability, and technological evolution scales).

---

## 📊 Scientific Insights & Output Sample
The pipeline dynamically fetches and outputs the exact planet names alongside their physical dimensions. 

* **The Drake Engine:** By applying a conservative weight (10% chance of developing life, and 1% of those evolving technological capabilities), the script calculates the localized probability of detectable alien signals within the captured stellar sector.
* **Error Handling:** Features a robust network `try-except` block to gracefully manage NASA server throttling or heavy traffic timeouts.

---

## 🚀 The Core Python Script
```python
import pandas as pd
import numpy as np

# Direct URL to fetch confirmed exoplanets directly in CSV format from NASA's API
url = "[https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=select+pl_name,pl_rade,pl_eqt,pl_insol+from+pscomppars&format=csv](https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=select+pl_name,pl_rade,pl_eqt,pl_insol+from+pscomppars&format=csv)"

try:
    print("📡 Connecting to NASA Exoplanet Archive... Scanning the cosmos...")
    nasa_data = pd.read_csv(url)
    nasa_data = nasa_data.dropna(subset=['pl_name', 'pl_rade', 'pl_eqt'])
    print(f"✅ Connection Successful! Downloaded {len(nasa_data):,} planets from NASA.\n")
    
    # Filtering the Goldilocks Zone
    earth_like = nasa_data[
        (nasa_data['pl_rade'] >= 0.5) & (nasa_data['pl_rade'] <= 1.5) & 
        (nasa_data['pl_eqt'] >= 180) & (nasa_data['pl_eqt'] <= 310)
    ]
    
    print(f"🌍 Number of Earth-like planets found: {len(earth_like)}")
    
    if len(earth_like) > 0:
        print("\n📜 Top Candidate Planets for Potential Intelligent Life:")
        print("=" * 65)
        print(earth_like[['pl_name', 'pl_rade', 'pl_eqt']].to_string(index=False))
        print("=" * 65)
        
        # Drake Equation Probability Heuristic
        prob_life = len(earth_like) * 0.10 * 0.01
        print(f"\n🔮 Statistical Probability of Intelligent Civilizations f this sample: ~{prob_life:.4f}")
except Exception as e:
    print(f"❌ Error occurred: {e}")
