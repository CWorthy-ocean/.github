# C-Star 0.17: runs that describe themselves, and CDR-LiTE end to end

*C-Star 0.16.1 and 0.17.0 · roms-tools 5.2.0 and 5.2.1 · ucla-roms 0.10.0, 0.10.1 and 0.11.0*

This release is about knowing what a run was. Every run now writes a manifest that says what was scheduled, when, where, with which package versions and from which inputs. A run directory can be copied and still read, and the workplan name is the run-id, so the common case needs no `--run-id` at all. It also closes the CDR-LiTE loop that 0.16.1 opened (a MARBL run's carbonate sensitivities feed a CDR-LiTE run without MARBL, and the wizard follows the BGC mode), carries the ucla-roms 0.10 and 0.11 lines with their science fixes, constant iron and faster tracer kernels, and takes the roms-tools 5.2 update, whose ESPER and boundary changes alter generated forcing. 0.16.1 was never announced on its own, so this note covers everything since 0.16.0: what most users will notice and what to do to move over. The full changelogs are linked at the end.

| package | before | now | notes |
|---|---|---|---|
| C-Star (`cstar-ocean`) | 0.16.0 | **0.17.0** | conda-forge / PyPI |
| roms-tools | 5.1.1 | **5.2.1** | required by the conda package (see §7); ESPER sources additionally need the CWorthy PyESPER fork at tag `v1.1.1+cworthy.1` |
| ucla-roms | 0.9.1 | **0.11.0** | pinned by your blueprint's ModelSpec; `roms-marbl-0.11-default` is bundled and is now the wizard's default model, `roms-marbl-0.10-default` (0.10.1) is bundled too |

---

## What changed

### 1. Every run records itself
*C-Star 0.17.0*

A run directory now holds `cstar-run.yaml`, the **run manifest**: an id minted once for the directory, the workplan's name and content hash, one attempt per time the run was scheduled (start time, system, `cstar-ocean` and `roms-tools` versions, which steps were clobbered or resumed, when it finished), one entry per step (application, working directory, the blueprint it ran, its status), and what the run depends on outside itself: referenced runs, source-data locations, codebases with their commits, and the catalog entries and forge blueprints named in the blueprints' provenance. Each step writes its own `cstar-step.yaml` when it finishes, with its outcome, times, versions and errors.

```bash
cstar run show my-run                 # attempts, steps and dependencies as tables
cstar run show my-run --format yaml   # the manifest itself (json works too)
cstar run show /path/to/a/copied/run  # reads the directory as it is; no tracking needed
```

`cstar run status` and `cstar run show` refresh the manifest, and close the latest attempt once every step is terminal, so a run that finishes while nobody is watching is closed the next time someone looks. A standalone `cstar blueprint run` writes the same two files in its working directory. Runs started with an earlier release get a manifest the first time they are re-entered.

### 2. `cstar run`: one place for existing runs, and run directories you can copy
*C-Star 0.16.1 and 0.17.0*

The commands that inspect a run live under a new `cstar run` group: `status`, `log`, `gather`, `ls`, `show`, `check`, and `path` and `cd` from 0.16.1. The `cstar workplan ...` spellings keep working. `cstar run path my-run [step]` prints a directory and nothing else, so it composes in scripts; `cstar run cd my-run [step]` moves your shell there once the small shell function is installed:

```bash
cstar env shell-init --install        # detects zsh or bash; re-run after every upgrade, then open a new shell
```

Everything C-Star writes inside a run is now relative: the ROMS namelist names its inputs relative to `work/`, forge's emitted blueprint names `../input_data/...`, and every symlink points inside the run. A run directory can therefore be copied or moved as a unit; `cstar run check my-run` reports anything that still points outside it (absolute paths, dangling links, missing step directories) and `--repair` rewrites in-run symlinks as relative ones.


### 3. The workplan name is the run-id
*C-Star 0.17.0*

`cstar workplan run my_workplan.yaml` runs under the id derived from the workplan `name`; `--run-id` is only for running the same workplan under a second id. Running the file again re-enters the run: steps that are done are reused, failed steps run again (`--resume` continues them in place instead), `--clobber <step|all>` clears steps deliberately, and every re-entry adds an attempt to the manifest and rotates the previous attempt's saved workplan and variables to `*.1`, `*.2` and so on. A *different* workplan with the same name is refused, so two experiments can never share a run directory by accident; editing the recorded file in place as a run grows is fine. Passing the transformed workplan saved in the run directory to `cstar workplan run` re-enters its run too.

**Breaking:** a workplan `name` must be a slug (`north-atlantic-2012`, not `North Atlantic 2012`); a non-slug name is refused with the slug to use, which is the id that name already produced, so existing runs stay reachable.

### 4. CDR-LiTE end to end
*C-Star 0.16.1 and 0.17.0 · ucla-roms 0.10.0*

`bgc_mode: cdr_lite` builds ucla-roms without MARBL and with the `CDR_LITE` key, from a ModelSpec, a forge blueprint or the wizard's BGC dropdown. It needs ucla-roms 0.10.0 or later and a CDR forcing whose releases target the CDR-lite tracers (simple-mode releases are tagged automatically); the tracer counts and the `_cdrtrc` output stream are derived from the generated forcing. The carbonate sensitivities such a run needs come from an earlier ROMS-MARBL run through a new workplan directive, so a MARBL spin-up and a cheap CDR-LiTE experiment chain without hand-copying files:

```yaml
directives:
  carbonate-sensitivity-from:
    step: marbl_run        # a MARBL step with cdr_gas_exch_output on; or path: <dir or file>
```

The producing run must be on ucla-roms 0.10.0 or later, whose `_cdrgas` files carry the forcing time variables and bracket the run window with a start and an end record. Forge blueprints can also list carbonate sensitivity files up front (`carbonate_sensitivity: files: [...]`).

In 0.17.0 the wizard follows the BGC mode: without MARBL it swaps the default ForcingSpec for the new physics-only `glorys-era5-physics` (GLORYS, ERA5, WOA restoring, TPXO tides, DAI rivers) and hides every setting that needs MARBL, keeping your edits for when you switch back. A new Output card between Run and Advanced gathers the OutputSpec picker and every output stream, the CDR streams included, and the CDR plots work for cdr_lite and passive-tracer forcing files.

### 5. ucla-roms 0.11: constant iron, and upscaling into CDR-LiTE parents
*ucla-roms 0.11.0 · C-Star 0.17.0*

MARBL's dissolved iron can be held constant at every wet point, which removes iron limitation as a variable in sensitivity experiments. In a forge blueprint or ModelSpec it is `marbl_bgc.use_constant_fe: true` with `constant_fe_value` in mmol/m³ (default 3.0e-3); in a ROMS-MARBL blueprint it is `use_constant_fe` under `namelist_overrides.marbl_biogeochemistry_settings`; in the wizard it is "Hold iron constant" under Advanced. Forge rejects the switch on an older ucla-roms or in a build without MARBL before generating anything.

Upscaled CDR now works into a parent without MARBL: Forge accepts `upscaled` CDR with `bgc_mode: cdr_lite` on 0.11.0 (state the parent's CDR-lite tracer counts in its `param` settings), the `upscaler` application writes the child's CDR-lite tracer fluxes after ALK and DIC with their names, and Forge writes `cdr_nz_chd = 0` so ROMS reads the level count from the file instead of aborting unless the child has exactly 50 levels. **Results change for depth-profile CDR runs:** 0.11.0 fixes the injected flux, which was scaled by roughly the child-to-parent column-depth ratio (about 4% low overall on a California Current nest, far more at the coast). Averaged `ddic_dco2`/`ddic_dalk` are now window means of the flux coefficients, so `_cdrgas` files and the CDR-LiTE runs they force do not reproduce earlier output bit for bit either.

**Breaking if you don't pin ROMS release tags:** a ucla-roms pin that is not a release tag (`main`, any branch) now gets the 0.11.0 namelist, which always writes the two iron keys; a build without them stops. Pin a release tag to keep the older namelist. Commit-hash pins are safe: Forge and the ROMS-MARBL step now agree on the namelist version for the release the commit descends from, so forge-then-run workplans pinned by hash no longer fail when the namelist is read.

### 6. ucla-roms 0.10: results change, and faster tracers
*ucla-roms 0.10.0 and 0.10.1*

**Results change in 0.10.0.** The second boundary update of each step read its data half a step too late, the Orlanski radiation condition used different time levels on opposite boundaries, the KPP 1-2-1 vertical smoothing ran in place and counted the background diffusivity about 1.33 times, rivers discharging across an MPI tile edge lost their flux, and several averaged fields (`Akv`, `Akt`, `hbls`, `hbbl`, surface fluxes) could hold NaNs. On MiniPac after five hours the RMS difference relative to each field's spread is 3.6% for sea level, 1.3% for velocity and about 3e-4 for temperature and salinity. Do not expect bit-for-bit reproduction of 0.9 output, and treat averaged CDR output as slightly different (the averaging weight was single precision). Every run's log now ends with a region-timer table, and the tracer kernels are reorganised for cache reuse, which cuts `step3d_t` by roughly a quarter with MARBL on.

0.10.1 restores the per-variable PIO sync whose removal made multi-node restart writes stall until the job hit its time limit; `roms-marbl-0.10-default` builds 0.10.1.

### 7. roms-tools 5.2: ESPER salinity conditioning, boundaries that follow the physics
*roms-tools 5.2.0 and 5.2.1 · C-Star 0.17.0*

**Results change for ESPER and density-interpolated BGC forcing.** An ESPER source now takes `esper_salinity_conditioning: true` (on in the bundled `glorys-era5-esper` spec): below 31 to 34 PSU the nets are evaluated at a salinity blended toward the WOA23 climatology and alkalinity and DIC are diluted back, which removes ESPER's river-plume artefacts (silicate blow-ups, negative nutrients, DIC above alkalinity). Forge stages the one WOA23 file this needs. It requires the CWorthy PyESPER fork at tag `v1.1.1+cworthy.1`, installed from a clone; on roms-tools 5.1 the key was accepted and silently ignored, which is why C-Star now requires 5.2.1. ESPER-derived oxygen is floored at 2.0 mmol/m³ instead of 0, with or without conditioning.

Boundary BGC with `density` or `density_mld` interpolation (the bundled `glorys-era5-glodap-woa` spec) is now written on the physics time axis in monthly files that follow each year's density structure, instead of one 12-record `_clim` file placed on a single sampled day. Expect about 36 BGC boundary files per simulated year and a much larger footprint (order 30 times the old file). The ESPER and density paths also no longer grow memory without bound while saving, and GLORYS subchunk references read only the slabs a crop needs.

### 8. Quality of life
*C-Star 0.16.1 and 0.17.0*

- Attaching large user-provided netCDF files in the wizard (tens of `_cdrgas` files) no longer reads them whole; the content hash samples each file.
- A relative `blueprint:` path in a workplan resolves against the workplan file, and a `{{ }}` placeholder in a blueprint path is filled when the workplan is prepared, as `check` already allowed.
- Forge rejects a river file whose tracer count ucla-roms 0.9.0 or later would abort on, and a roms-tools river with passive tracers or without `include_bgc` for a MARBL run, before the build.
- Blueprint paths and step names with spaces no longer break the launched step command, and a missing local blueprint is refused when the workplan is prepared rather than when the step launches.
- `cstar workplan run --resume <id>` explains that `--resume` takes no value; `cstar run status <unknown-id>` exits non-zero; shell completion after a mistyped run-id no longer prints a traceback.
- A transient HTTP error while inspecting a remote input is reported as such instead of rejecting the file as "a text file".
- The wizard's CDR settings gain a "Passive tracers" field, readable labels for the CDR-lite settings, tooltips on every Advanced field, and the "Compute dt" button works while other settings are still incomplete.

---

## Updating to C-Star 0.17.0

### 1. Update the environment

```bash
# laptop or workstation (bundles the compiler/MPI/netCDF toolchain)
conda create -n cstar-env -c conda-forge cstar-ocean-standalone=0.17.0

# HPC system (the site's modules supply the toolchain)
conda create -n cstar-env -c conda-forge cstar-ocean=0.17.0
```

Updating in place also works (`conda install -n cstar-env -c conda-forge cstar-ocean=0.17.0`) and pulls roms-tools 5.2.1 along. If you created a new environment and use Jupyter, re-register the kernel with `cstar env register-kernel`. If you generate ESPER forcing, install the CWorthy PyESPER fork into the environment; the bundled ESPER spec now needs it:

```bash
git clone --branch v1.1.1+cworthy.1 https://github.com/CWorthy-ocean/PyESPER
pip install -e PyESPER
```

### 2. Refresh the shell function

```bash
cstar env shell-init --install
```

Then open a new shell. The function from 0.16.1 does not know `cstar run cd`; `cstar workplan cd` keeps working either way.

### 3. Check names and relative paths

- Workplan `name` values must be slugs; a refused name is reported with the slug to use.
- If you previously used relative `location:` paths in a hand-written blueprint, note that they now resolve against the blueprint file, not the directory where you run the command. 

### 4. Move to ucla-roms 0.11 deliberately

Pick `roms-marbl-0.11-default` in the wizard (now the default) or pin ucla-roms 0.11.0 in your ModelSpec. Blueprints pinned to 0.9 or 0.10 release tags keep building as before; branch pins get the 0.11 namelist (§5) and must move to a release tag or to a branch that has the iron keys. Any comparison against earlier output should account for the science changes in §6, and CDR-LiTE runs need `_cdrgas` files from a 0.10.0-or-later producer.

### 5. Expect different BGC forcing from roms-tools 5.2

ESPER and density-interpolated boundaries regenerate differently (§7). A `_clim` boundary file left in a forge run from an earlier version still counts as present on a rerun without `--clobber`; remove it or rerun the forge step with clobber to get the new files.

---

Full release notes: [C-Star 0.16.1](https://github.com/CWorthy-ocean/C-Star/releases/tag/0.16.1) and [0.17.0](https://github.com/CWorthy-ocean/C-Star/releases/tag/0.17.0) · [roms-tools 5.2.0](https://github.com/CWorthy-ocean/roms-tools/releases/tag/5.2.0) and [5.2.1](https://github.com/CWorthy-ocean/roms-tools/releases/tag/5.2.1) · [ucla-roms 0.10.0](https://github.com/CWorthy-ocean/ucla-roms/releases/tag/0.10.0), [0.10.1](https://github.com/CWorthy-ocean/ucla-roms/releases/tag/0.10.1) and [0.11.0](https://github.com/CWorthy-ocean/ucla-roms/releases/tag/0.11.0)
