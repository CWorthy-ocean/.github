# Welcome!
[[C]Worthy](https://www.cworthy.org) is a nonprofit organization dedicated to accelerating ocean-based climate solutions through innovative software and rigorous scientific research.
For more on [C]Worthy, see our [two minute introduction video](https://www.cworthy.org/#block-7dcd1d1ed572d734b0e9).

This page is where all of our development work comes together.

<p align="center">
  <img src="https://github.com/CWorthy-ocean/.github/raw/draft/ALK_FG_CO2uptake_highres.gif" alt="GitHub centered image">
  <br>
  <i>A demonstration of our tools in action: a simulation of an ocean alkalinity enhancement experiment in the East Pacific (left), and the resultant carbon dioxide uptake (right)</i>
</p>

# Navigating our GitHub
The below table describes our main projects and where to find them:

| Project         | What it does           | Who for?  |
| -------------   |-------------| -----|
| [ROMS-MARBL](https://github.com/CWorthy-ocean/ucla-roms.git) ([docs](https://cworthy-ucla-roms.readthedocs.io/))     | Physical-biogeochemical regional ocean simulations with modules for advanced mCDR research (Fortran)| Experienced ocean modeling scientists who want maximum control over their experiments |
| [C-Star*](https://github.com/CWorthy-ocean/C-Star.git) ([docs](https://c-star.readthedocs.io))         | Allows frictionless serialization and reproduction of complex ocean simulation workflows to ensure verifiable scientific integrity (Python) | Scientists who wish to share and reproducibly run models (from "blueprints") and multi-step model/analysis pipelines ("workplans") using simple CLI commands |
| [ROMS Tools](https://github.com/CWorthy-ocean/roms-tools.git) ([docs](https://roms-tools.readthedocs.io/))    | Generates (netCDF) input data required by ROMS for simulations; Produces high-quality figures and diagnostics of simulation results (Python) | ROMS and C-Star users who want a highly accessible toolkit to help manually prepare ocean simulations |
| [C-SON Forge*](https://github.com/CWorthy-ocean/cson-forge.git) ([docs](https://cworthy-ocean.github.io/cson-forge/))  | Wraps the three above packages, providing an accessible utility to automate (based on existing templates) the preparation of ROMS simulations and their C-Star blueprints. | New scientific users to the [C]Worthy ecosystem who seek an accessible toolkit for conducting reproducible mCDR research. |

\* C-Star and C-SON Forge are under active and rapid development. Some features are incomplete and user-facing APIs may change. However, we welcome early testers and feedback!

# Where to start

A user who wants to quickly go from building your own ocean domain to running it on a laptop (assuming it's small) or on a C-Star supported HPC could start from C-SON Forge's [installation instructions](https://cworthy-ocean.github.io/cson-forge/installation/) and [demo notebook](https://cworthy-ocean.github.io/cson-forge/installation/#try-running-the-cstarspecbuilder-demo). Please note that C-SON Forge is under active development!

For a more detailed and hands-on tour of all of the inputs that go into a ROMS-MARBL simulation, you can walk through the [ROMS-Tools end-to-end workflow](https://roms-tools.readthedocs.io/en/latest/end_to_end.html). Understanding these components is essential to building a robust and accurate model.

If you'd like to skip the details around setting up the model and would learn how to use pre-configured blueprints and workplans to reproducibly run an ocean model or orchestrate long-running and complex analyses, try out the standalone [C-Star blueprint tutorial](https://c-star.readthedocs.io/en/latest/tutorials/tutorial_bp.html) and workplan tutorial (coming soon).

# We want your feedback!

Anyone is welcome to submit Github issues or start discussions. Let us know what is and isn't working, where your pain points are for ocean modeling, and what you'd like to see next!
