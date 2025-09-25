# Mumps- Washington Focused Build

## Build Overview
- **Build Name**: Mumps- Washington Focused Build
- **Pathogen/Strain**: Mumps/MuV
- **Scope**: Genotype G whole genomes (2006–present).
- **Purpose**: This repository contains the Nextstrain build for Washington State genomic surveillance of mumps. It is specifically designed to retain all available mumps sequences from Washington State, while also incorporating representative samples from the United States, North America, and a reduced set of global background sequences. The build focuses on whole-genome sequences of genotype G collected since 2006, which is the lineage responsible for nearly all recent mumps outbreaks in the United States.
- **Considerations**: The Washington-focused mumps build reuses much of the Nextstrain global mumps workflow
, with local modifications to the configuration and subsampling strategies. This README explains how the Washington-specific build relates to and depends on the global mumps build, and how it can be adapted.
- **Nextstrain Build/s Location/s**:
- https://github.com/NW-PaGe/mumps
- https://github.com/nextstrain/mumps

## Table of Contents
- [Pathogen Epidemiology](#pathogen-epidemiology)
- [Scientific Decisions](#scientific-decisions)
- [Getting Started](#getting-started)
  - [Data Sources & Inputs](#data-sources--inputs)
  - [Setup & Dependencies](#setup--dependencies)
    - [Installation](#installation)
    - [Clone the repository](#clone-the-repository)
- [Run the Build](#run-the-build-with-test-data)
  - [Expected Outputs](#expected-outputs)
  - [Visualizing Results](#visualize-results)
- [Customization for Local Adaptation](#customization-for-local-adaptation)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)


## Pathogen Epidemiology
- Overview:
  - Mumps virus (MuV), a negative-sense single-stranded RNA virus in the family Paramyxoviridae.
  - Twelve recognized genotypes (A–N, excluding E and M). In the U.S., genotype G has been dominant since ~2006.
  - Mode of transmision include respiratory droplets, saliva, or direct contact with fomites. Highly transmissible in close-contact environments (schools, dormitories).

- Geographic Distribution and Seasonality
  - Globally distributed, with recurrent outbreaks in Europe and North America despite high vaccination coverage.
  - Washington has experienced recurrent outbreaks, often linked to close-contact settings.
  - Transmission may occur year-round, but outbreaks often cluster in late winter and spring.

- Public Health Importance
  - Outbreaks occur in vaccinated populations due to waning immunity.
  - Important for outbreak response, vaccination strategies, and cluster detection.

- Genomic Relevance
  - Enables tracking of introductions vs. sustained transmission.
  - Identifies linkages between Washington and external cases.
  - Supports outbreak investigations, especially in congregate settings.

- Additional Resources
  - https://www.cdc.gov/mumps/about/index.html 
  - Link Pathogen Genomic Profile if we have one created

## Scientific Decisions
Nextstrain builds are designed for specific purposes and not all types of builds for a particular pathogen will answer the same questions. The following are critical decisions that were made during the development of this build that should be kept in mind when analyzing the data and using this build. *Subsampling, root selection, and reference selection must be included at minimum.*

- **Nomenclature**: [CDC genotype nomenclature; this build only includes genotype G.]
- **Subsampling**: [For this build, subsampling was designed to prioritize Washington State sequences while maintaining appropriate contextual diversity. All available Washington genotype G sequences are retained without limitation to ensure complete coverage of local transmission dynamics. To provide regional perspective, U.S. sequences outside of Washington are subsampled evenly by division, while additional contextual sequences are drawn from Canada and Mexico. A reduced number of global sequences are included to preserve phylogenetic background without overwhelming the Washington-specific signals.]
- **Root selection**: [The root sequence is not specified, but inferred by `augur ancestral`]
- **Reference selection**: [The alignment and mutation calling are performed against a MuV-G reference genome (such as NC_002200), which provides a consistent baseline for variant calling and comparative analysis.]
- **Inclusion/Exclusion**: [In terms of inclusion and exclusion, the build accepts only sequences from 2006 onward to reflect the era of genotype G predominance. Sequences from other genotypes, those with incomplete genomes, and those with insufficient or poor-quality metadata are excluded to ensure reliability.]

- **Other adjustments**: [Additional adjustments include applying maximum thresholds on the number of contextual sequences retained from outside Washington while leaving Washington itself unrestricted, guaranteeing that no local data are lost. Low-quality or hypervariable regions of the genome are masked during analysis to reduce noise and improve phylogenetic accuracy.] 

## Getting Started
This project is set up so that a new user can clone the repository, place (or link) curated mumps sequence and metadata files into the expected data/ directory, and run an end-to-end Washington-focused build with a single command. The workflow is orchestrated by Snakemake via the Nextstrain CLI and uses Augur for phylogenetics and Auspice for visualization. The build is opinionated toward complete retention of Washington sequences and carefully limited contextual sampling from the U.S., North America, and a small global background, so that state-level questions (introductions vs. sustained transmission; linkages to other jurisdictions) can be answered quickly and reproducibly.
- [Washington Prioritization: The build is opinionated toward complete retention of Washington sequences and carefully limited contextual sampling from the U.S., North America, and a small global background, so that state-level questions (introductions vs. sustained transmission; linkages to other jurisdictions) can be answered quickly and reproducibly.]

### Data Sources & Inputs
The build expects a FASTA of MuV sequences and a TSV of metadata that conform to the column conventions used by the Nextstrain mumps workflow. In practice, the simplest path is to reuse the curated inputs from the Nextstrain mumps repo: either copy those files into this repository’s data/ directory or create symlinks so they always track upstream updates. The filenames the workflow looks for are data/sequences.fasta and data/metadata.tsv. You do not need to pre-filter these inputs to Washington; the Washington-focused selection and contextual subsampling are handled by the build’s configuration. If you maintain a local include/exclude list (for example, to force-include specific Washington accessions or exclude problematic records), place those files under config/washington/ and reference them in the Washington config so they are applied consistently each run. This build focuses on genotype G whole genomes collected from 2006 onward. Inputs that are non-G, incomplete, or missing critical metadata will be filtered out during the Augur steps to preserve analysis quality. Leaving the raw inputs broad (rather than pre-trimmed to Washington only) ensures the pipeline can apply its WA-first retention and context-aware sampling logic exactly as designed.

- **Sequence Data**: [Data sources]
- **Metadata**: [Metadata sources]
- **Expected Inputs**:
    - `[data_location]/sequences.fasta` (containing viral genome sequences)
    - `[data_location]/metadata.xls` (with relevant sample information)
- **Private data, if applicable**:

### Setup & Dependencies
#### Installation
Ensure that you have [Nextstrain](https://docs.nextstrain.org/en/latest/install.html) installed.

To check that Nextstrain is installed:
```
nextstrain check-setup
```

#### Clone the repository:

```
git clone https://github.com/[your-github-repo].git
cd [your-github-repo]
```

## Run the Build
*(Explain how to run the build with test data. Example text on how this might be explained is below)*

To test the pipeline with the provided example data located in `[data_location]/` make sure you are located in the build folder `[your-github-repo]` before running the build command:

```
nextstrain build .
```

When you run the build using `nextstrain build .`, Nextstrain uses Snakemake as the workflow manager to automate genomic analyses. The Snakefile in a Nextstrain build defines how raw input data (sequences and metadata) are processed step-by-step in an automated way. Nextstrain builds are powered by Augur (for phylogenetics) and Auspice (for visualization) and Snakemake is used to automate the execution of these steps using Augur and Auspice based on file dependencies.

### Run the Build with Test Data (Optional)
For builds that do not programmatically pull data from NCBI or another source, include a `test_data/` folder containing a minimal working example of test data that can be successfully executed by the build.

### Expected Outputs
*(Outline the expected outputs and in which folders to locate them)*
The file structure of the repository is as follows with `*`" folders denoting folders that are the build's expected outputs.

```
.
├── README.md
├── Snakefile
├── auspice*
├── clade-labeling
├── config
├── new_data
├── results*
└── scripts
```
More details on the file structure of this build can be found here (link to Wiki page that contains contents of  Repository File Structure Overview section).

After successfully running the build there will be two output folders containing the build results.

- `auspice/` folder contains: a .json file
- `results/` folder contains:

### Visualize Results
- Dropping .json into auspice.us
- `nextstrain view auspice/*.json`

- Link folks to tree interpretation resources that people can use to make their inferences.


## Customization for Local Adaptation
 *[Brief overview on how to adapt this build for another jurisdiction, such as a state, city, county, or country. Including links to Readmes in other sections that contain detailed instructions on what and how to modify the files]*

This build can be customized for use by other demes, including as states, cities, counties, or countries.
This build can be adapted for another jurisdiction (e.g., Oregon, California) by:

Updating config/[jurisdiction].yaml with appropriate filters.

Editing subsampling/[jurisdiction].yaml to retain all local sequences.

Adjusting builds.yaml to define the new build target.

- What files or folders need to be modified in order to adapt for other jurisdictions? If this is lengthy then you can link to a wiki page tab that goes into detail on how someone might adapt this build for their jurisdiction.

## Contributing
For any questions please submit them to our [Discussions](insert link here) page otherwise software issues and requests can be logged as a Git [Issue](insert link here).

## License
This project is licensed under a modified GPL-3.0 License.
You may use, modify, and distribute this work, but commercial use is strictly prohibited without prior written permission.

## Acknowledgements

Workflow based on Nextstrain mumps
Adapted structure from NW-PaGe mpox
Data contributions from GISAID, GenBank, CDC, and WA DOH laboratories.

<!-- Repository File Structure Overview [**Move contents of this section to Wiki**]
(This section outlines the high-level file structure of the repo to help folks navigate the repo. If the build follows the pathogen template repo feel free to make this section brief and link to the pathogen template repo resource)*

Example text below:

This Nextstrain build follows the structure detailed in the [Pathogen Repo Guide](https://github.com/nextstrain/pathogen-repo-guide).
Mainly, this build contains [number] workflows for the analysis of [pathogen] virus data:
- ingest/ [link to ingest workflow] Download data from [source], clean, format, curate it, and assign clades.
- phylogenetic/ [link to phylogenetic workflow] Subsample data and make phylogenetic trees for use in nextstrain.

OR
The file structure of the repository is as follows with `*`" folders denoting folders that are the build's expected outputs.

```
.
├── README.md
├── Snakefile
├── auspice*
├── clade-labeling
├── config
├── new_data
├── results*
└── scripts
```

- `Snakefile`: Snakefile description
- `config/`: contains what
- `new_data/`: contains What
- `scripts/`: contains what
- `clade-labeling`: contains what
-->
