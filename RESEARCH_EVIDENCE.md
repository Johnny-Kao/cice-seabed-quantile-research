# CICE probabilistic seabed-stress quantile — full-model Research

Status: **PASS for reachable numerical difference; not yet cleared for Staging/upstream**.
Date: 2026-11-14 JST.

## Question

`cicecore/cicedyn/dynamics/ice_dyn_shared.F90` -> `seabed_stress_factor_prob()` uses:

```fortran
x_kmax = exp(mu_i + sqrt(c2*sigma_i)*1.9430d0)
```

But `sigma_i` is already the lognormal standard deviation defined by `sqrt(log(1 + v_i/m_i**2))`. The corresponding distribution-percentile expression is `exp(mu_i + sqrt(2)*sigma_i*1.9430d0)`.

The original 2021 code introduction is CICE [PR #565](https://github.com/CICE-Consortium/CICE/pull/565). This is a **model-contract question**, not automatically permission to alter physics calibration.

## Official whole-model test

[Github Actions run 43502353568](https://github.com/Johnny-Kao/cice-seabed-quantile-research/actions/runs/43502353568).

- Official CICE cloned with submodules and built with `./cice.setup`, `./cice.build`, `./cice.submit`.
- GNU compiler, `gbox80`, `boxnodyn`, 1 MPI rank, 72 time steps, `ice_ic='internal'`, `kdyn=1`.
- Generated case namelist verified `seabed_stress=.true.`, `seabed_stress_method='probabilistic'`.
- **Synthetic research fixture:** `get_bathymetry` outputs a uniform water depth of 10 m, identical in both variants. This is **not** a natural bathymetry dataset or a production forecast.
- Source diagnostics record the computed quantile cutoff and `sum(Tbt)`. Both variants include identical diagnostics and depth fixture.
- Variant A retains baseline quantile code; variant B changes that **one formula line** to `sqrt(c2)*sigma_i`.

### Results

Both variants: **official compile / run / smoke test PASS**.

The same first printed `SEABED_TBT_SUM` instance in the respective simulation runs:

| Measurement | Baseline | 1-line corrected |
|---|---:|---:|
| Sum of sea-bed T-point stress factor `Tbt` | 863.7757314760 | 353.28239 (approx) |
| Relative ratio | 2.445x | 1x |

The final printed sample (of the last five per-job log excerpts) is approximately 857.85459 baseline vs 348.19410 corrected; these are different model trajectories after timestepping and must not be interpreted as matched single-cell local errors.

The full model actually **executes the intended production-path routine** with nonzero stress and changing outcomes. The smoke tests pass because they only verify model completion, not scientific equivalence.

**Important limitations:**
- The 10 m bathymetry and box forcing are intentionally synthetic; this does not quantify an error in actual Arctic landfast-ice simulations.
- The **paper/calibration intent** must be verified with original authors/maintainers or reference data before labeling this as an accepted bug and before proposing behavioral changes.
- No full grid/reference climatology or physical validation has yet been performed.
- No evidence has yet established that historical empirical calibration used the equation from the publication instead of the implementation.
- No Toolkit, Staging, or upstream CICE repo has been modified.

## Next validation gate

Compare reference/author implementation or published single-condition benchmark to both formulas; verify conservation, grid behavior, and old test baselines. If confirmed, prepare a **1-line production patch**, plus targeted regression test and a short discussion of changed calibration semantics, **only after user authorization**.
