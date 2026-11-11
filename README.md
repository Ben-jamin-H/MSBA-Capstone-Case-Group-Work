# PBS Utah Major Donor Prediction: Group Work

MSBA Capstone (IS 6813), University of Utah. Team: Erin Bednarik, Benjamin Hogan, Samantha Huang, and Kate Klinger.

This repository holds the team's shared exploratory data analysis, the group modeling notebook, the code and data both of them run on, and the team documents behind them.

## Viewing the notebooks

GitHub shows `.html` files as code rather than as a web page. To read a rendered notebook, open one of these links, or download the `.html` file and open it in a browser:

- Group EDA: <https://raw.githack.com/Ben-jamin-H/MSBA-Capstone-Case-Group-Work/main/pbs_utah_group_eda.html>
- Group modeling: <https://raw.githack.com/Ben-jamin-H/MSBA-Capstone-Case-Group-Work/main/modeling/pbs_utah_group_modeling.html>
- Group modeling decisions (the 22 decisions the team accepted): <https://raw.githack.com/Ben-jamin-H/MSBA-Capstone-Case-Group-Work/main/team_decisions/group_modeling_decisions.html>
- Team modeling synthesis (how the four individual notebooks compare): <https://raw.githack.com/Ben-jamin-H/MSBA-Capstone-Case-Group-Work/main/team_decisions/team_modeling_synthesis.html>

The Word document in `business_problem/` keeps its tracked changes, so download it to see them.

## What the modeling notebook is for

The course data are synthetic, so no number in the notebook is a fact about PBS Utah's donors. The notebook hands PBS Utah a procedure to run on its own records: write down the best list an officer could build by hand and test whether a model beats it; rank by expected dollars and check whether that changes anything; read the list size off a curve of return against size; do the same for retention; and put both lists in dollars per name worked to decide which call comes next. The numbers in the notebook are one run of that procedure. On this release the rules did as well as the models at every job, so the recommended lists are rule-ordered and the models supply probabilities and expected amounts alongside them. The six steps added after the instructor's review are decisions D23 to D28, each dated and explained in the notebook's Introduction.

## What is here

| Path | What it is |
|---|---|
| `pbs_utah_group_eda.qmd` / `.html` | Group EDA notebook (source and rendered) |
| `build_panel.py` | Shared build script: loading, cleaning, the donor-year panel, cohorts, and giving features. Both notebooks use it |
| `data_cache/` | The nine course tables, typed and converted to Parquet. Both notebooks read the data from here |
| `data_dictionary_combined.csv` | Kate's master data dictionary, used in the EDA appendix |
| `modeling/pbs_utah_group_modeling.qmd` / `.html` | Group modeling notebook (source and rendered), including the six amendments (D23 to D28) made after the instructor's review on November 11 |
| `modeling/requirements.txt` | Python packages for both notebooks |
| `modeling/model_cache/` | Saved model fits, so a render does not have to refit everything |
| `modeling/outputs/` | FY2026 lists and the four saved models with their records. `working_queue_FY2026.csv` is ordered by the officer rule (former major donors first, then FY2026 giving, then years of giving) and carries the model's probability and rank, the expected giving if the donor converts, and the expected-increase rank; `working_queue_FY2026_expected_increase_additions.csv` holds the few donors the expected-increase ordering would add; `nurture_FY2026.csv` is the 3 to 5 year list; `major_donor_risk_FY2026.csv` carries the model's drop probability and four orderings (model probability, first-time rule, dollars at risk by the model and by the tenure table) |
| `team_decisions/` | The group modeling decisions page and the team modeling synthesis |
| `business_problem/` | The revised business problem statement draft, with tracked changes |

## Data

The course data is a synthetic release from the instructor's repository, <https://github.com/jefftwebb/donor_prediction_capstone_project>, which also holds the data dictionaries and release notes. Results from it validate methods, not findings about PBS Utah's donors.

The raw CSV files are not in this repository: together they are about 900 MB, and three are larger than GitHub's 100 MB file limit. They are not needed to run the notebooks, because `data_cache/` holds the same tables as Parquet. To rebuild the cache from the raw files, put the nine CSV files in a `tables/` folder next to this repository's folder (so the EDA finds them at `../tables`) and delete `data_cache/`.

## Running the notebooks

Both notebooks were rendered with Python 3.13 and Quarto 1.10.

```
python -m venv .venv
.venv\Scripts\activate            # on macOS or Linux: source .venv/bin/activate
pip install -r modeling/requirements.txt

quarto render pbs_utah_group_eda.qmd --no-execute-daemon
cd modeling
quarto render pbs_utah_group_modeling.qmd --no-execute-daemon
```

- The EDA takes about 3 to 4 minutes.
- The modeling notebook takes about 10 minutes using the saved fits in `model_cache/`, and about 55 minutes if it has to refit everything.
- Use `--no-execute-daemon`. A Python kernel left running by an earlier render can hold several gigabytes of memory, which makes the modeling notebook's longest fits fail on a 16 GB laptop.
- Both renders are reproducible: every bootstrap and every model uses a fixed seed.

## Individual work

Each member's individual EDA and modeling notebook stays in their own repository. The Purpose sections of both group notebooks list what each one contributed.
