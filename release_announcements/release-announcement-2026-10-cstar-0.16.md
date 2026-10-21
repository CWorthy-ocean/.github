# C-Star 0.16: chain runs, prepare on the login node, build workplans in the wizard

*C-Star 0.16.0 · roms-tools 5.1.1 · ucla-roms 0.9.0 and 0.9.1*

This release is about running real campaigns with less hand-wiring: a workplan can now reference steps of an earlier run, a run can be fully prepared on a login node before it enters the queue, and the wizard gains a Workplan page that composes all of this for you. It also carries the ucla-roms 0.9 line, with CDR-lite tracers that no longer need MARBL, and the roms-tools/xarray update that stops Forge from hanging mid-generation. This note covers what most users will notice and what to do to move over; the full changelogs are linked at the end.

| package | before | now | notes |
|---|---|---|---|
| C-Star (`cstar-ocean`) | 0.15.1 | **0.16.0** | conda-forge / PyPI |
| roms-tools | 5.1.0 | **5.1.1** | required by the conda package, together with xarray 2026.9 (see §7) |
| ucla-roms | 0.8.0 | **0.9.1** | pinned by your blueprint's ModelSpec; `roms-marbl-0.9-default` is bundled and is now the wizard's default model |

---

## What changed

### 1. Workplans can reference other runs
*C-Star 0.16.0*

A workplan no longer has to spell out paths into an earlier run's directory. Declare the other run under `runs:` with an alias (its run-id, as shown by `cstar workplan ls`; a `{{var}}` runtime variable works too), then write `<step>@<alias>` wherever a step name is accepted: `depends_on`, a deferred `blueprint: {from_step: ...}`, a directive's `step:` key and `{{output_dir: ...}}`-style placeholders. Every reference is checked when the workplan is scheduled, and the exact run record used is pinned into the transformed workplan. On SLURM a workplan may even depend on a step that is still running; the new step is submitted with a SLURM dependency on that job.

```yaml
runs:
  spinup: my-spinup-run-id
steps:
  - name: production
    blueprint: production.yaml
    depends_on: [outer@spinup]
    directives:
      continue-from:
        step: outer@spinup
```

`continue-from` also accepts a `timestamp:` (an ISO date-time, a date, or the 14-digit stamp from a restart file name) to continue from an exact restart rather than the latest one.

### 2. Prepare a run on the login node, launch it later
*C-Star 0.16.0*

`cstar workplan run my_workplan.yaml --pre-run` stages inputs, clones and compiles the codebases, and generates and validates the namelist for every step, without launching ROMS or submitting anything. Start-up problems surface immediately instead of after a wait in the queue, and compilation moves out of the allocation. Running the same workplan again without the flag attaches to the prepared directories and launches. The same works for a single blueprint with `cstar blueprint run --pre-run` followed by `--resume`. Steps that cannot be prepared (Forge steps, deferred blueprints, anything consuming another step's output) are skipped with the reason and run from scratch on the second command. After a failed scheduled run, `--pre-run` again clears and re-prepares the failed steps.

### 3. A Workplan page in the wizard
*C-Star 0.16.0*

`cstar wizard` now opens a two-page app: Blueprint and Workplan (`cstar forge wizard` launches the same app). The Workplan page composes a workplan from catalog, path, uploaded, deferred or inline blueprints, with per-step overrides, `continue-from` and `nest-from` directives, compute overrides and `step@alias` references. Recipes generate time-chunked runs, spin-up dt ramps, forge-then-run pairs and upscaling chains. A live preview shows the YAML (editable) and the dependency graph, and Check, Pre-run and Run stream their output into the page. Loading an older workplan rewrites deprecated forms (`rst_path`, `bry_path`, `joined_output`) and lists every change. The old "Workplan (experimental)" export on the Blueprint page is gone.

A workplan's `compute_environment` can now choose the launcher (`launcher: local | slurm`), name the system it was written for, and set workplan-wide SLURM defaults (account, queue, walltime) that every step inherits. Account and queue values given there satisfy the launcher's environment check, so `CSTAR_SLURM_*` variables are only needed for what the workplan leaves out.

### 4. Less boilerplate in blueprints and workplans
*C-Star 0.16.0*

- `working_dir` is optional. A blueprint that omits it runs under `CSTAR_DATA_HOME/blueprint_runs/<application>/<name>`, so blueprints are portable between machines. The ROMS-MARBL blueprint Forge emits no longer carries one.
- A step may say `blueprint: inline` and supply everything through `blueprint_overrides`, so `nest_ic` and `upscaler` steps need no dummy blueprint file. The merged blueprint is written into the step's work directory when the workplan is scheduled.
- Workplan runs now live at `CSTAR_DATA_HOME/workplan_runs/<run-id>` (runs started earlier keep their directory).
- The catalog keeps one file per blueprint at `blueprints/<application>/<name>.yaml`; a forge blueprint and the ROMS-MARBL blueprint it emits may share a name. `wales-toy` ships in the bundled catalog, so a workplan can start from a real catalog blueprint out of the box.
- A blueprint with the same major schema version as the installed C-Star is used as-is: no migration copy, no workplan rewrite. `cstar blueprint check` tells you when an older file needs `cstar blueprint migrate` instead of listing validation errors.

### 5. Blueprints record where they came from
*C-Star 0.16.0*

Every blueprint may carry a `provenance` block: when and by what it was generated (tool, C-Star system, package versions, and for a workplan step the run id and working directory) and what it was derived from. ROMS-MARBL blueprints emitted by Forge name their forge blueprint (with a content hash) and each catalog spec they were composed from; blueprints saved from the wizard are stamped too, and re-stamped only when their content changes. `cstar forge run` now publishes the emitted blueprint to the run's `output/` directory and prints that copy as the one to run. This is a minor schema bump (roms_marbl 3.1.0 and the like): existing files load unchanged, but files written by 0.16 are refused by older installs with an "upgrade cstar-ocean" message.

### 6. ucla-roms 0.9: CDR-lite tracers, with or without MARBL
*ucla-roms 0.9.0 and 0.9.1 · C-Star 0.16.0*

The passive CDR tracer mode is now called **CDR-lite**: the CPP key is `CDR_LITE`, the namelist groups are `&CDR_LITE_SETTINGS` and `&CDR_LITE_OUTPUT_SETTINGS`, and Forge's settings section is `cdr_lite_output` with `do_cdr_lite_output`. Stored forge blueprints migrate automatically; hand-written names in ModelSpecs, OutputSpecs and overrides are renamed with a warning. Builds pinned to ucla-roms 0.7 or 0.8 still get the old group names, byte for byte.

With ucla-roms 0.9.1 (the pin in `roms-marbl-0.9-default`), parameterized CDR releases work without MARBL and can target CDR-lite, passive, or temperature and salinity tracers. In Forge, CDR forcing in `simple`, `yaml` and `netcdf` modes can be combined with `bgc_mode: none`; `upscaled` mode still needs MARBL and says so. A `bgc_mode: none` run also no longer requests 32 BGC tracers in its namelist, which used to crash ROMS with garbage tracer names.

**Results change in 0.9.0.** The carbonate-chemistry constants moved to the total pH scale with a bracketed pH solver (the old one silently converged to pH ~13.7 in high-alkalinity water, which is exactly the OAE regime), so `ddic_dco2` and `ddic_dalk` shift by roughly 0.5% and 0.03%. KPP and solar heating now see the free surface, which matters in shallow tidal regions. The CDR DIC gas-exchange flux no longer carries a spurious layer-thickness factor, spread releases deliver exactly the requested amount of every tracer, volume releases add water at the release depth, and the `TRACER_DIFF2` group is finally read (so `tnu2` takes effect). Expect runs not to reproduce 0.8 output bit for bit, and treat existing CDR-lite output as not comparable.

### 7. Forge no longer hangs mid-generation
*roms-tools 5.1.1 · xarray 2026.9*

The intermittent stall during Forge input generation, where the dask progress bar froze partway through writing initial or boundary conditions, was a lock-ordering deadlock in xarray's netCDF backend. It is fixed in xarray 2026.9, which roms-tools could not use until 5.1.1 removed its xarray ceiling. The `cstar-ocean` 0.16.0 conda package requires both, so updating the environment picks up the fix. roms-tools 5.1.1 also moves its compiled numba kernels into the user cache directory, which fixes the `cannot import name 'Grid'` error after switching to an editable install.

### 8. Quality of life
*C-Star 0.16.0*

- Under `use_pio`, Forge converts user-provided netCDF-4 river and CDR files to CDF-5 for you (original kept) and warns up front about which files it will convert.
- `max_walltime` accepts SLURM's day form (`2-03:00:00`), which used to crash job creation.
- Local-launcher walltimes work again (the `timeout` wrapper was malformed), and on macOS `cstar workplan run` tells you to install coreutils if GNU `timeout` is missing rather than failing with exit code 127.
- `cstar admin clean` no longer deletes your catalog at `~/cstar/catalog` or the source-data cache.
- Forge resolved `ntimes` to 0 for any run window shorter than a day; a short test run now runs.
- `cstar workplan run --dry-run` is gone; `cstar workplan check` (deep by default) is the way to validate without scheduling.

---

## Updating to C-Star 0.16.0

### 1. Update the environment

A fresh environment is the cleanest way to take the xarray and roms-tools updates:

```bash
# laptop or workstation (bundles the compiler/MPI/netCDF toolchain, now with GNU coreutils)
conda create -n cstar-env -c conda-forge cstar-ocean-standalone=0.16.0

# HPC system (the site's modules supply the toolchain)
conda create -n cstar-env -c conda-forge cstar-ocean=0.16.0
```

Updating in place also works (`conda install -n cstar-env -c conda-forge cstar-ocean=0.16.0`) and will pull roms-tools 5.1.1 and xarray 2026.9 along. If you created a new environment and use Jupyter, re-register the kernel with `cstar env register-kernel`.

### 2. Migrate your catalog layout

Blueprints saved by earlier versions (`<name>.forge_blueprint.yaml`, `B_<name>.yaml`, per-machine `blueprints/<machine>/<name>/`) are still read for one more release, with a warning listing them. Move them into place once:

```bash
cstar admin migrate-catalog ~/cstar/catalog --dry-run
cstar admin migrate-catalog ~/cstar/catalog
```

Nothing is overwritten; anything that could not be moved is reported.

### 3. Know where new runs land

- New workplan runs go to `CSTAR_DATA_HOME/workplan_runs/<run-id>` instead of `CSTAR_DATA_HOME/<run-id>`. Scripts or notebooks that build the old path need updating; runs started before 0.16 keep their directory and are still found when you re-run, resume, gather or clean them.
- `cstar blueprint run` (and `cstar forge run`) writes under `CSTAR_DATA_HOME/blueprint_runs/<application>/<name>` for a blueprint without `working_dir`, so Forge output is at `blueprint_runs/forge/<name>` rather than the older `~/cstar/_forge_bp_runs/<name>` layout; forge blueprints that still carry that old default lose it on load. Older runs can stay where they are; nothing needs moving. A blueprint that relied on running in the current directory needs `working_dir: .`.
- On Bouchet, `CSTAR_DATA_HOME` now defaults to your `scratch_pi_*` directory, so new runs land on scratch without setting `SCRATCH` yourself.
- The shared source-data cache follows one rule everywhere: `cstar/source-data` under `CSTAR_PROJECT_HOME` (defaults to `$PROJECT`), else under scratch, else under your home directory. An existing `cstar-forge-data/source-data` cache in the same place is linked in the first time Forge runs, so nothing is downloaded again. On a laptop with `PROJECT` set, or a machine that exports only `SCRATCH_DIR`/`LOCAL_SCRATCH`, the cache therefore moves out of your home directory; point `CSTAR_PROJECT_HOME` at a group directory to share one cache with collaborators. `cstar forge show-paths` prints the resolved locations.

### 4. Move to ucla-roms 0.9.1 deliberately

Pick `roms-marbl-0.9-default` in the wizard (now the default) or pin ucla-roms 0.9.1 in your ModelSpec to get CDR forcing without MARBL. Blueprints pinned to 0.7 or 0.8 keep building as before. Any comparison against earlier output should account for the science changes in §6.

---

Full release notes: [C-Star 0.16.0](https://github.com/CWorthy-ocean/C-Star/releases/tag/0.16.0) · [roms-tools 5.1.1](https://github.com/CWorthy-ocean/roms-tools/releases/tag/5.1.1) · [ucla-roms 0.9.0](https://github.com/CWorthy-ocean/ucla-roms/releases/tag/0.9.0) and [0.9.1](https://github.com/CWorthy-ocean/ucla-roms/releases/tag/0.9.1)
