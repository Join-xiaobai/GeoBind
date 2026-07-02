# Data directory

Place datasets here using the paths configured in `main.py`.

Expected layouts:

```text
data/dta/davis/
data/dta/kiba/
data/dti/hetionet/
data/dti/yamanishi_08/
data/moa/activation/
data/moa/inhibition/
```

Required columns:

- DTA: `drug_id`, `protein_id`, `affinity`
- DTI: `drug_id`, `protein_id`, `label`
- MoA: `DrugID`, `TargetID`, `label`

Large raw datasets are not included in this clean release.
