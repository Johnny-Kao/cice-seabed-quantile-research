# Independent-observation validation feasibility — 2026-11-14

## Status: observation archive verified; observational Red/Green NOT completed

### Reproducible evidence

Official [CI run 43508658132](https://github.com/Johnny-Kao/cice-seabed-quantile-research/actions/runs/43508658132) retrieved the independent Zenodo dataset https://doi.org/10.5281/zenodo.19546997.

Verified:
- Archive: `Arctic_landfastice_dataset_1976_2026_OUC.zip`.
- Bytes: 9,913,777.
- Verified MD5: `53bdc88530d1e11495251ae82f73298e` (matches deposited record).
- 2,269 ZIP entries: 2,265 weekly landfast ice `.bin` raster files (approximately 1,042,568 bytes per grid) plus documentation/scripts.
- 25-km Arctic gridded weekly landfast extent across 1976–2026 according to deposit metadata.
- Record also provides `north_x_y_lat_lon.txt` with grid coordinates and `contour_weekly_extent_code.m`.

This constitutes an actual independent observational data availability/integrity test, **not yet a test of predictive model skill**.

### Candidate independent work

Augustin Lambotte et al. (2026), https://doi.org/10.5194/egusphere-2026-3861 (published as preprint 2026-09-30), compare ground/no-ground NEMO-SI3 across two rheologies. Public abstract verifies relevance, but no directly matched baseline/corrected CICE-NEMO forecast grids or complete 2026 source/configuration were verified in this investigation. Related older SI3 model outputs, e.g. https://doi.org/10.5281/zenodo.11582103, concern **different rheology experiments** and do **not** constitute a controlled one-LOC quantile A/B experiment.

### Essential next gate: scientifically meaningful paired simulation

To measure observational effect of the one-LOC correction, run a **matched** actual-landfast-ice simulation with (1) original `x997` expression and (2) mathematically corrected `x997`; keep model SHA, bathymetry, forcing, boundaries, resolution, timestep, initial state, and `alphab` fixed. Analyze aligned landfast masks over the same dates in the independent observed dataset, with IoU, false fast-ice fraction, missed coverage, and optionally stability in ice velocity and thickness biases. Avoid calibrating on the same held-out verification dates. Evaluate separately no-ice, strong coastal grounding, and near-cutoff cells and by region/month. The archived 2022 source https://doi.org/10.5281/zenodo.6534162 has the **original formula**, but was not verified to include all meteorological forcing, restart, and diagnostic model output needed to replay a historical Arctic simulation.

**Do not** compare the uniform 10 m shallow-bathymetry synthetic whole-CICE run against real observations: geography/timing/physics mismatch invalidates any skill score. A passing smoke test or unchanged paper performance would likewise not prove better real-world predictive accuracy.

## Decision

- Mathematical mismatch with claimed 99.7th percentile: CONFIRMED from previous evidence.
- Whole CICE shallow-water synthetic sensitivity: CONFIRMED in [full-model research CI](https://github.com/Johnny-Kao/cice-seabed-quantile-research/actions/runs/43502353568).
- Historical original implementation: CONFIRMED in [Dupont 2022 archive](https://github.com/Johnny-Kao/cice-seabed-quantile-research/actions/runs/43504872301).
- Public independent fast-ice observations accessible and checksum-verified: CONFIRMED in [observation CI](https://github.com/Johnny-Kao/cice-seabed-quantile-research/actions/runs/43508658132).
- Higher predictive skill after 1LOC fix: **UNKNOWN / NOT TESTED WITH MATCHED REALISTIC MODEL FORCING**.

No Staging or upstream changes. Manual-only CI.
