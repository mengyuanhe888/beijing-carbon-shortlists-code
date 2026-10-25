# Beijing subdistrict carbon-emission shortlist analysis

Analysis code accompanying *Sensitivity and stability of subdistrict carbon-emission shortlists under district-level statistical constraints: Evidence from Beijing*.

Repository: [https://github.com/mengyuanhe888/beijing-carbon-shortlists-code](https://github.com/mengyuanhe888/beijing-carbon-shortlists-code).

We compare subdistrict allocations, shortlist membership, and electricity-and-heat reallocation under citywide and district-level constraints. The implementation uses the study's 343 subdistricts and seven annual observations per subdistrict (2016–2022).

## Contents

`Code.zip` contains the analysis entry point (`复现.py`), analysis modules (`分析代码/`), selected figure scripts (`绘图代码/`), dependency files, and an input dictionary (`INPUTS.md`). Extract the archive before running the commands below.

Reported_results.zip provides CSV versions of Tables 3–5 and Supplementary Tables S1 and S3, together with definitions and units. These tables support comparison with the findings reported in the manuscript.

The processed panel, comparison inputs, fitted parameters, recorded bootstrap samples, and complete numerical outputs are supplied separately. Model estimation and recalculation require those inputs; the reported-result tables do not replace them. Requests for study inputs should be directed to the authors.

## Installation and checks

The analysis dependencies are specified in `requirements.txt`; the recorded analysis environment uses Python 3.13.11.

```bash
python -m pip install -r requirements.txt
python check_code.py
python 复现.py --help
```

`check_code.py` checks the distributed files and Python syntax. It does not read study data, fit models, or validate numerical results.

## Running the analysis

Keep the separately supplied study inputs outside the code directory. Replace `/path/to/study_inputs` below with their location.

```bash
python 复现.py predict \
  --panel /path/to/study_inputs/模型数据/panel.csv \
  --config /path/to/study_inputs/参数配置/method.json \
  --out ../results_prediction
```

The output directory must be new and outside the extracted code directory. `predict` writes the calculated panel, the parameters used, and input/code checksums.

| Command | Purpose | Additional input |
|---|---|---|
| `predict` | Calculate allocations using saved coefficients and standardization parameters | None beyond panel and configuration |
| `fit` | Estimate the full-sample model using the configured starts and optimizers | None beyond panel and configuration |
| `validate` | Run the selected holdout or variable-configuration analysis | `--part holdout`, `joint`, `blocks`, or `all` |
| `bootstrap` | Refit the recorded resamples and calculate selection frequencies | `--draws` with the supplied `bootstrap_draws.json` |
| `compare` | Compare shortlists and calculate external-product scenarios and reallocation bounds | `--external` with the comparison-input directory |
| `shortlist` | Combine supplied selection frequencies with scenario ranks | `--frequencies` and `--scenarios` |

Each command accepts `--panel`, `--config`, and `--out`. Use `python 复现.py COMMAND --help` for its arguments. A supplied `parameters.json` can be selected with `--parameters` where supported; the study configuration is still required. The legacy `check` command expects the complete study inputs in their original package locations; use `check_code.py` for this code distribution.

Saved-coefficient calculations, model refitting, and bootstrap refitting are separate operations. Analysis commands do not update manuscript tables, figures, or LaTeX files. We use all 1,000 `continued_all` draws as the selection-frequency denominator; 998 fits in the saved study run reported convergence. That convergence count is a saved-run result, not a result of checking this code distribution.

## Figure scripts

The included entry points redraw Figure 1 and Supplementary Figures S1, S2, and S4 from separately supplied map or plotting inputs. Their `00_运行说明.txt` files list the required files, dependencies, and output names. Figure 2 was prepared in PowerPoint. The repository does not contain standalone entry points for every manuscript figure.

Install `requirements-figures.txt` before redrawing figures and the additional requirements listed for Figure 1. Arial must be available locally. Map files and plotting inputs are not included. The original third-party colour-map license notices are retained with their files.

## Inputs and sources

`INPUTS.md` defines fields, units, configuration files, and optional inputs. The source datasets include CEADs inventories, Beijing Regional Statistical Yearbooks, EOG monthly VIIRS composites, OSM/Geofabrik, EDGAR, ODIAC, and POI86. Their links and the processing conventions are listed in the input dictionary. Downloading these source products alone does not reconstruct the processed panel; the study's GIS preprocessing inputs are also required.
