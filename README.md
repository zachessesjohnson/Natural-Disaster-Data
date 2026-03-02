# Natural Disaster Data

Data relevant to assessing natural disaster impact on real estate, including flood insurance claims and home insurance premiums at the county and state level.

---

## Table of Contents

1. [Overview](#overview)
2. [Data Inventory](#data-inventory)
3. [Data Sources](#data-sources)
4. [Repository Structure](#repository-structure)
5. [Requirements](#requirements)
6. [Setup](#setup)
7. [Usage](#usage)
8. [Reproducibility](#reproducibility)
9. [Citation](#citation)
10. [License](#license)
11. [Contributing](#contributing)
12. [Contact](#contact)

---

## Overview

This repository provides curated datasets intended to support research and analysis on how natural disasters—particularly flooding—affect real estate values, insurance costs, and property risk. The data combine federal flood insurance program claim records with state-level home insurance premium statistics, enabling cross-sectional and longitudinal studies of disaster exposure and insurance markets.

Potential use cases include:

- Modeling the relationship between flood frequency and property insurance premiums
- Estimating the financial exposure of real estate markets to natural disaster risk
- Supporting academic research in disaster economics, climate risk, and housing policy
- Building dashboards or visualizations of flood loss and insurance cost trends

---

## Data Inventory

| File | Description | Rows (excl. header) | Grain |
|---|---|---|---|
| `FimaNfipClaims_CountyYear.csv` | FEMA NFIP flood insurance claims aggregated by county and year | ~57,434 | County × Year |
| `state_home_insurance_combined.csv` | Average homeowners and renters insurance premiums by state and year | ~338 | State × Year |

### `FimaNfipClaims_CountyYear.csv` — Column Reference

| Column | Description |
|---|---|
| `County_Code` | FIPS county code |
| `State` | Two-letter state abbreviation |
| `Year` | Calendar year |
| `Total_Claims` | Total number of flood insurance claims filed |
| `Building_Payment_Total` | Gross building damage payments (USD) |
| `Contents_Payment_Total` | Gross contents damage payments (USD) |
| `ICC_Payment_Total` | Increased Cost of Compliance (ICC) payments (USD) |
| `Building_Payment_Net` | Net building payments after recoveries (USD) |
| `Contents_Payment_Net` | Net contents payments after recoveries (USD) |
| `ICC_Payment_Net` | Net ICC payments after recoveries (USD) |
| `Building_Damage_Amount` | Total reported building damage (USD) |
| `Contents_Damage_Amount` | Total reported contents damage (USD) |
| `Building_Insurance_Coverage` | Total building insurance coverage in force (USD) |
| `Contents_Insurance_Coverage` | Total contents insurance coverage in force (USD) |
| `Building_Property_Value` | Aggregate building property value (USD) |
| `Contents_Property_Value` | Aggregate contents property value (USD) |
| `Avg_Water_Depth_Inches` | Average reported flood water depth (inches) |
| `Policy_Count` | Number of active NFIP policies |
| `Community_Name` | NFIP community name (may be blank) |
| `City` | City name (may be "Currently Unavailable") |

### `state_home_insurance_combined.csv` — Column Reference

| Column | Description |
|---|---|
| `State` | Full state name |
| `Year` | Calendar year |
| `Homeowners_Avg_Premium` | Average annual homeowners insurance premium (USD) |
| `Homeowners_Rank` | State rank for homeowners premium (1 = highest) |
| `Renters_Avg_Premium` | Average annual renters insurance premium (USD) |
| `Renters_Rank` | State rank for renters premium (1 = highest) |

---

## Data Sources

- **FEMA NFIP Claims (`FimaNfipClaims_CountyYear.csv`):** Derived from the [FEMA National Flood Insurance Program (NFIP) claims dataset](https://www.fema.gov/flood-insurance/work-with-nfip/data-visualization). The raw claims data are publicly available through FEMA's OpenFEMA portal and have been aggregated here to the county-year level.

- **State Home Insurance Premiums (`state_home_insurance_combined.csv`):** Compiled from publicly available insurance industry reports and/or state insurance commissioner publications. <!-- TODO: Add the specific source URL(s) or publication names for this dataset if known. -->

---

## Repository Structure

```
Natural-Disaster-Data/
├── FimaNfipClaims_CountyYear.csv   # NFIP flood claims by county and year
├── state_home_insurance_combined.csv  # Home insurance premiums by state and year
├── LICENSE                         # Apache-2.0 license
└── README.md                       # This file
```

---

## Requirements

No specific software is required to read the data files—both are standard CSV format and can be opened with any spreadsheet application or data analysis tool. Common environments include:

- **Python:** `pandas`, `numpy`, `matplotlib` / `seaborn` (optional, for analysis and visualization)
- **R:** base R (`read.csv`) or `tidyverse` (`readr`, `dplyr`, `ggplot2`)
- **Excel / Google Sheets:** Direct import of `.csv` files

<!-- TODO: If this project adds scripts or notebooks in the future, list specific version requirements here (e.g., Python >= 3.9, R >= 4.2). -->

---

## Setup

1. **Clone the repository:**

   ```bash
   git clone https://github.com/zachessesjohnson/Natural-Disaster-Data.git
   cd Natural-Disaster-Data
   ```

2. **Install dependencies** *(only needed if running analysis scripts):*

   ```bash
   # Python
   pip install pandas numpy matplotlib seaborn

   # R (run inside an R session)
   install.packages(c("readr", "dplyr", "ggplot2"))
   ```

<!-- TODO: If analysis scripts or notebooks are added, update this section with any additional setup steps (e.g., virtual environment creation, conda environment file). -->

---

## Usage

### Python

```python
import pandas as pd

# Load NFIP flood claims data
nfip = pd.read_csv("FimaNfipClaims_CountyYear.csv")
print(nfip.head())
print(nfip.dtypes)

# Load state home insurance data
insurance = pd.read_csv("state_home_insurance_combined.csv")
print(insurance.head())

# Example: total building payments by state
state_totals = (
    nfip.groupby("State")["Building_Payment_Total"]
    .sum()
    .sort_values(ascending=False)
)
print(state_totals.head(10))
```

### R

```r
library(readr)
library(dplyr)

# Load NFIP flood claims data
nfip <- read_csv("FimaNfipClaims_CountyYear.csv")
glimpse(nfip)

# Load state home insurance data
insurance <- read_csv("state_home_insurance_combined.csv")
glimpse(insurance)

# Example: average homeowners premium trend for a state
insurance |>
  filter(State == "Florida") |>
  arrange(Year) |>
  select(Year, Homeowners_Avg_Premium)
```

### Excel / Google Sheets

1. Open the application and choose **File → Import** (or **Open**).
2. Select `FimaNfipClaims_CountyYear.csv` or `state_home_insurance_combined.csv`.
3. Confirm comma as the delimiter and import.

<!-- TODO: Add any additional example queries, notebooks, or scripts once they are available in the repository. -->

---

## Reproducibility

The datasets in this repository are static snapshots. To reproduce or refresh the underlying data:

1. **NFIP Claims:** Download the full NFIP claims dataset from [OpenFEMA](https://www.fema.gov/openfema-data-page/fima-nfip-redacted-claims-v2) and aggregate to county-year level using the grouping keys `reportedCity`, `countyCode`, and `yearOfLoss` (or equivalent).

2. **State Insurance Premiums:** Re-collect average premium data from the relevant state insurance commissioner reports or industry sources. <!-- TODO: Document the exact source and methodology used to compile `state_home_insurance_combined.csv`. -->

<!-- TODO: If data processing scripts are added, reference them here and describe how to run them end-to-end. -->

---

## Citation

If you use this data in academic work, please cite both the repository and the underlying data sources:

```
Johnson, Z. (2024). Natural Disaster Data [Data repository].
GitHub. https://github.com/zachessesjohnson/Natural-Disaster-Data
```

For the NFIP claims data, also cite FEMA:

```
Federal Emergency Management Agency (FEMA). FIMA NFIP Redacted Claims Dataset.
OpenFEMA. https://www.fema.gov/openfema-data-page/fima-nfip-redacted-claims-v2
```

<!-- TODO: Update the year and add any additional citations for the state insurance premium source. -->

---

## License

This repository is licensed under the [Apache License 2.0](LICENSE).

The underlying data sourced from FEMA are in the public domain as works of the U.S. federal government. State insurance premium data may be subject to the terms of the original publishers; please verify before redistribution.

---

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to your fork: `git push origin feature/your-feature-name`
5. Open a pull request describing your changes.

Please ensure any new data files include documentation of their source, grain, and column definitions in this README.

---

## Contact

For questions, suggestions, or data issues, please open a [GitHub Issue](https://github.com/zachessesjohnson/Natural-Disaster-Data/issues) or contact the repository owner directly via GitHub: [@zachessesjohnson](https://github.com/zachessesjohnson).
