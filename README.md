# Palmer Penguins — Exploratory Data Analysis

> **AI Engineer Roadmap — Project 0.2**
> *Teaches: matplotlib/seaborn, statistical intuition, communicating with data.*
> *Done when: a non-technical person reads your notebook and understands the finding.*

A storytelling EDA of the [Palmer Penguins](https://allisonhorst.github.io/palmerpenguins/)
dataset (344 penguins, 3 species, Antarctica). The notebook walks from "what is
this data?" to **one genuinely surprising finding: Simpson's paradox** in the
bill measurements.

👉 **[Open the notebook with all charts rendered](eda_penguins.ipynb)** — it runs
top to bottom and needs no setup to *read*.

---

## The finding in one picture

![Simpson's paradox in penguin bills](images/05_simpsons_paradox.png)

Pooled across all penguins, **bill length and bill depth are negatively
correlated** (the dashed line slopes down, *r* = **−0.23**): longer bills look
*shallower*. But split by species, the relationship **flips to positive** in
every group:

| Group | Correlation (bill length vs. depth) |
| --- | ---: |
| All penguins (pooled) | **−0.23** |
| Adelie | +0.39 |
| Chinstrap | +0.65 |
| Gentoo | +0.65 |

The species sit in different regions of the plot (Gentoo: long + shallow bills;
Adelie: short + deep), so pooling them draws a misleading *between-group* line
that points the **opposite way** from the real *within-group* relationship.

**Why an AI engineer should care:** this is exactly how a metric, a feature
importance, or an A/B test can be confidently wrong. A relationship measured on
pooled data can reverse once you condition on a hidden variable (here species; in
production often segment, device, or time period). Always check that a
relationship survives *inside* the subgroups.

---

## What the notebook covers

1. **Setup & data loading** — from a vendored CSV (`data/penguins.csv`), so it's
   fully reproducible offline.
2. **Data quality check** — dtypes, missing values, and an honest decision to
   *drop* (not impute) the few incomplete measurement rows.
3. **Composition** — species counts and how species map onto islands.
4. **Distributions** — all four body measurements, split by species.
5. **Body mass by species & sex** — the consistent male/female gap.
6. **Correlation matrix** — which measurements move together.
7. **The surprise** — Simpson's paradox, shown visually and numerically.
8. **Summary table** — every finding in one place, plus a plain-language
   takeaway for a non-technical reader.

## Reproduce it yourself

```bash
python -m venv .venv
source .venv/bin/activate          # Windows: .\.venv\Scripts\activate
pip install -r requirements.txt

# Regenerate the notebook structure from build_notebook.py and execute it:
python build_notebook.py
jupyter nbconvert --to notebook --execute --inplace eda_penguins.ipynb

# ...or just open it interactively:
jupyter lab eda_penguins.ipynb
```

The notebook is **built from a script** (`build_notebook.py`) rather than
hand-edited, so it regenerates identically every time and stays easy to review in
a diff.

## Project layout

```
project-0.2-eda-penguins/
├── eda_penguins.ipynb     # the analysis, with outputs + charts committed
├── build_notebook.py      # regenerates the notebook reproducibly
├── data/penguins.csv      # vendored dataset (no network needed)
├── images/                # exported figures used in this README
├── requirements.txt
└── README.md
```

## Credits

Data: Dr. Kristen Gorman and the Palmer Station LTER, packaged as
`palmerpenguins` by Allison Horst. Distributed under CC0.

## License

MIT (code). Dataset is CC0.
