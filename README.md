# Network Medicine–Based Drug Repurposing for Canine Neuralgia

Code, inputs, and archived results used for canine neuralgia network analysis. The folders are numbered by workflow stage. Each stage retains its own dependency specification because the archived environments differ.

The publication dataset contains 1,546 drugs. The primary ranking uses STRING score >= 700; the sensitivity ranking uses >= 900. The archived Z-scores, p-values, and ranks are retained.

## Workflow

| Folder | Purpose | Main entry point |
| --- | --- | --- |
| [01_human_to_canine_orthologs](01_human_to_canine_orthologs/README.md) | Human drug-target symbols to canine one-to-one orthologs | `map_drug_targets_to_dog_biomart.py` |
| [02_canine_string_mapping](02_canine_string_mapping/README.md) | Canine gene symbols to STRING protein IDs | `map_dog_targets_to_string.py` |
| [03_disease_module_lcc](03_disease_module_lcc/README.md) | Disease-module connectivity and LCC significance | `canine_neuralgia_LCC_netmedpy.py` |
| [04_network_proximity](04_network_proximity/README.md) | Drug-target proximity to the disease set | `run_proximity.py` |

The ortholog output matches the STRING-mapping input exactly. The final STRING-mapping workbook is used by the proximity analysis. It contains 4,919 target rows for 1,546 drugs.

The LCC analysis and proximity analysis answer different questions. LCC evaluates connectivity among disease proteins. Proximity uses the complete supplied disease union, subject to background-network coverage exclusions; it does not substitute the disease-induced LCC.

## Setup and execution

Enter the relevant numbered directory before following its README. Use a separate virtual environment for each stage; do not combine the different requirements into one installation. The root directory has no global requirements file for this reason.

The original human input to the ortholog stage was intentionally excluded from the supplied folder. A matching private copy is required to rerun that first stage; its checksum is recorded in the archived metadata. All supplied archived data are retained.

Use fresh output directories when rerunning analyses. Live BioMart and STRING queries can change mappings, and the historical LCC hash seed is unknown. The stage-specific documentation explains these limits and identifies missing historical scripts.

## Verification

From the repository root, check all recorded file hashes with the standard-library verifier:

```bash
python3 tools/verify_repository.py
```

Run the existing regression tests in the appropriate scientific environments:

```bash
cd 03_disease_module_lcc
python -m unittest discover -s tests -v
```

```bash
cd 04_network_proximity
python -m unittest discover -s tests -v
```

These two examples start from the repository root. File-integrity verification confirms the files match the recorded snapshots. [DATA_SHA256SUMS](DATA_SHA256SUMS) covers the supplied scientific data. [CODE_SHA256SUMS](CODE_SHA256SUMS) identifies the Python and shell files. See [REPRODUCIBILITY.md](REPRODUCIBILITY.md) for the scope of reproduction supported by the supplied files.

## Naming, citation, and release

[RENAMING.md](RENAMING.md) maps the original folder and LCC archive names to their new locations. Main scientific script filenames were retained so their imports, recorded commands, and output conventions remain recognizable.

Citation metadata for the collection are provided in [CITATION.cff](CITATION.cff); individual stages retain their own metadata. The public repository is [tnigen/network-medicine-drug-repurposing-canine-neuralgia](https://github.com/tnigen/network-medicine-drug-repurposing-canine-neuralgia). Version `1.0.0` is the configured publication-package version; a release tag has not yet been created. Cite an immutable release or commit in the thesis.

The original software is covered by the [MIT License](LICENSE). Third-party software and database content retain their own terms and attribution requirements. The upstream disease-set curation is not supplied in this collection.
