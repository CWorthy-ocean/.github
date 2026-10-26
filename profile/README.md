# Welcome!
[[C]Worthy](https://www.cworthy.org) is a nonprofit organization dedicated to accelerating ocean-based climate solutions through innovative software and rigorous scientific research.
We build open-source ocean models, datasets and workflows that help researchers, regulators, communities and project developers understand where ocean carbon dioxide removal (CDR) may work, what risks it carries, how much carbon it removes, and how those outcomes can be monitored and verified.
Alongside the modeling software on this page, we publish the [Ocean CDR Atlas](https://www.cworthy.org/ocean-cdr-atlas), open datasets and interactive maps of ocean CDR efficiency.

This page is a guide to our modeling projects on GitHub and where to get started with them.

<p align="center">
  <img src="https://github.com/CWorthy-ocean/.github/raw/main/ALK_FG_CO2uptake_highres.gif" alt="Animation of a simulated ocean alkalinity enhancement experiment in the East Pacific and the resulting carbon dioxide uptake">
  <br>
  <i>A demonstration of our tools in action: a simulation of an ocean alkalinity enhancement experiment in the East Pacific (left), and the resultant carbon dioxide uptake (right)</i>
</p>

# The modeling stack
For ocean CDR methods such as ocean alkalinity enhancement, the carbon removed cannot be measured directly at sea: quantifying it takes observations combined with ocean models.
Our stack combines a regional ocean model with the physics and biogeochemistry CDR research needs, tools that prepare the model's inputs and analyze its output, and a workflow system that sets up, runs and keeps a record of each simulation.

Because removal claims rest on these simulations, the stack is built to be trusted and checked.
It builds on community ocean models with decades of development behind them; its code is open, so academic and commercial users can inspect what it does; each simulation is a complete description that others can rerun and reproduce; and one consistent workflow holds every project to the same standard.

A typical project chooses a region, a grid and forcing datasets; generates the model's input files from global ocean and atmosphere datasets (GLORYS, ERA5, TPXO tides, river discharge, biogeochemical climatologies); compiles and runs the model on a laptop or an HPC cluster; and analyzes the output, for example computing the CO2 uptake and CDR efficiency of an alkalinity release.

## Our projects

### [UCLA-ROMS](https://github.com/CWorthy-ocean/ucla-roms) ([docs](https://cworthy-ucla-roms.readthedocs.io/))
The ocean model itself, written in Fortran: our fork of the UCLA Regional Ocean Modeling System, optionally coupled to the [MARBL](https://marbl-ecosys.github.io) biogeochemistry model, with additions for CDR research such as CDR release forcing and nested runs with upscaling.
It suits experienced ocean modelers who want full control over the model's compile-time and run-time settings.

### [ROMS-Tools](https://github.com/CWorthy-ocean/roms-tools) ([docs](https://roms-tools.readthedocs.io/))
A Python package that generates the netCDF input files UCLA-ROMS needs (grid, initial conditions, surface, boundary, tidal and river forcing, CDR forcing, nesting) from global source datasets, and that reads, plots and analyzes model output, including CDR metrics.
Use it directly when you want hands-on control of each input; C-Star also uses it under the hood.

### [C-Star](https://github.com/CWorthy-ocean/C-Star) ([docs](https://c-star.readthedocs.io/))
The Python workflow system that ties the stack together, handling setup, data management, provenance tracking and orchestration of UCLA-ROMS.
Its Forge application, with a point-and-click wizard, builds a new domain and its input files; a **blueprint** records everything one run needs; a **workplan** chains runs into a multi-step experiment.
The same files run on a laptop or on a [supported HPC system](https://c-star.readthedocs.io/en/latest/deployments/machines.html), from the `cstar` command line.
Use C-Star to get from a region to a running simulation quickly, or to share runs that others can reproduce.
C-Star is under active development, and we welcome early users and feedback!

# Where to start

**Run your first simulation.** [Install C-Star](https://c-star.readthedocs.io/en/latest/getting_started/installation.html) and work through the guided tutorials in order.
The [end-to-end tutorial](https://c-star.readthedocs.io/en/latest/tutorials/end_to_end.html) uses the wizard to build a tiny Western Indian Ocean domain, generates its inputs and runs it on a laptop in minutes.
The [workplan tutorial](https://c-star.readthedocs.io/en/latest/tutorials/workplan.html) runs the same steps from a single file, and the [CDR tutorial](https://c-star.readthedocs.io/en/latest/tutorials/cdr_workplan.html) adds a spin-up and an alkalinity release, then computes the release's CO2 uptake and CDR efficiency.
The [nesting and upscaling tutorial](https://c-star.readthedocs.io/en/latest/tutorials/nesting_upscaling.html) then runs a release in a fine grid nested inside a coarser one.

**Run a ready-made blueprint.** To see what a blueprint contains and run one without generating a domain first, try the [toy-domain example](https://c-star.readthedocs.io/en/latest/applications/ucla_roms/example.html).
Every input is downloaded for you, and it finishes in a few minutes on a laptop.

**Build the inputs yourself.** The [ROMS-Tools end-to-end workflow](https://roms-tools.readthedocs.io/en/latest/end_to_end.html) walks through every input a UCLA-ROMS + MARBL simulation needs, on a laptop.
Understanding these pieces is essential to building a robust and accurate model.
The [ROMS-Tools examples](https://roms-tools.readthedocs.io/en/latest/examples.html) cover each input in more depth (set up for the Perlmutter and Anvil clusters), including [CDR forcing](https://roms-tools.readthedocs.io/en/latest/cdr_forcing.html) and [CDR analysis](https://roms-tools.readthedocs.io/en/latest/cdr_analysis.html), and the [ROMS-Tools deep dive](https://www.cworthy.org/videos/v/a-roms-tools-deep-dive) video tours the package.

**Work with the model directly.** The UCLA-ROMS docs cover [installing and compiling the model](https://cworthy-ucla-roms.readthedocs.io/en/latest/installation/index.html) and two [tutorials](https://cworthy-ucla-roms.readthedocs.io/en/latest/tutorials/index.html): a nested CDR workflow explained step by step, and a from-scratch template for building your own simulation.

# We want your feedback!

Anyone is welcome to open an issue or start a discussion on any of these repositories ([UCLA-ROMS](https://github.com/CWorthy-ocean/ucla-roms/discussions), [ROMS-Tools](https://github.com/CWorthy-ocean/roms-tools/discussions), [C-Star](https://github.com/CWorthy-ocean/C-Star/discussions)).
Let us know what is and isn't working, where your pain points are for ocean modeling, and what you'd like to see next!
