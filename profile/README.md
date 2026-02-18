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
| [C-Star](https://github.com/CWorthy-ocean/C-Star.git) ([docs](https://c-star.readthedocs.io))         | Allows frictionless serialization and reproduction of complex ocean simulation workflows to ensure verifiable scientific integrity (Python) | Scientists who wish to share and reproducibly run models (from "blueprints") and multi-step model/analysis pipelines ("workplans") using simple CLI commands |
| [ROMS Tools](https://github.com/CWorthy-ocean/roms-tools.git) ([docs](https://roms-tools.readthedocs.io/))    | Generates (netCDF) input data required by ROMS for simulations; Produces high-quality figures and diagnostics of simulation results (Python) | ROMS and C-Star users who want a highly accessible toolkit to help manually prepare ocean simulations |
| [C-SON Forge](https://github.com/CWorthy-ocean/cson-forge.git) ([docs](https://cworthy-ocean.github.io/cson-forge/))  | Wraps the three above packages, providing an accessible utility to automate (based on existing templates) the preparation of ROMS simulations and their C-Star blueprints. | New scientific users to the [C]Worthy ecosystem who seek an accessible toolkit for conducting reproducible mCDR research. |

# Where to start

To begin developing your own ocean domain, you can start with the default configurations in CSON-Forge and simply change the grid, or explore ROMS-tools in more detail to better understand and configure each of your inputs.

For codifying and running your simulations, especially on an HPC with SLURM, explore the C-Star CLI tools, which will take care of building ROMS-MARBL, launching your simulations in the correct environment, and orchestrating multiple simulations. You can also run smaller example simulations from premade blueprints on your laptop to get a feel for the system.
