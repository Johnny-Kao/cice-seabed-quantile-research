# Historical calibration-source verification — Dupont et al. (2022)

Verified 2026-11-14 JST. Source: https://doi.org/10.5281/zenodo.6534162
Zenodo record 6534162: `CODE_Dupont_et_al_2022.tgz`, 346,118,701 bytes, archived 2022-05-10.
The read-only source-inspection GitHub Actions run is:
https://github.com/Johnny-Kao/cice-seabed-quantile-research/actions/runs/43504872301

## Direct evidence from published archive

At least the following **non-DEBUG, non-TEMP** source locations contain the same formulation:

- `CODE_Dupont_et_al_2022/CICECMC_2.3.8/cice5.1.2_cmc/source/ice_dyn_shared.F90`, near lines 1077-1090.
- `CODE_Dupont_et_al_2022/CICECMC_2.3.8/cice5.1.2_cmc/work_import/ice_dyn_shared.F90`.
- `CODE_Dupont_et_al_2022/CONCEPTS_4.3.0/NEMO3.6/NEMOGCM_eccc-ppp2/CONFIG/CREG025_CMC_ts_C5/WORK/ice_dyn_shared.F90`.
- `CODE_Dupont_et_al_2022/CONCEPTS_4.3.0/NEMO3.6/NEMOGCM_eccc-ppp2/NEMO/CICE5_SRC_2.3.8/source/ice_dyn_shared.F90`.

The archived model computes:

```fortran
sigma_i = sqrt(log(c1 + v_i/m_i**2))
x997 = exp(mu_i + sqrt(c2*sigma_i)*1.9430d0)
x_kmax = x997
```

The declared `sigma_i` is the lognormal standard deviation. A literal 99.7th-percentile expression for this distribution would use `sqrt(c2)*sigma_i`, not `sqrt(c2*sigma_i)`. The archived production-source formulation is the one currently seen in CICE, supporting historical lineage rather than a later transcription bug.

## Interpretation

- CONFIRMED: Old formula exists in the published historical CICE/NEMO source and named CREG025 config WORK copy, not just temporary experimental files.
- STRONGLY SUPPORTED: Historical implementation for reported study was based on the old formula.
- NOT PROVEN: Which exact binary, configuration and parameter calibration run produced every published figure, and whether the incorrect percentile form was intentionally preferred as a calibrated approximation. An archive code copy alone cannot prove which binary ran.
- CONFIRMED from separate official CICE red/green: A single-line correction materially changes stress factor in synthetic shallow-water CICE run. https://github.com/Johnny-Kao/cice-seabed-quantile-research/actions/runs/43502353568

**Decision**: Treat as model/paper mathematical inconsistency with calibration compatibility implications. Do not submit a purportedly harmless one-line physics correction without clarifying desired semantic contract with maintainers. Research only, no Staging/upstream modification.
