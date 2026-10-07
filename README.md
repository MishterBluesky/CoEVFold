# CoEVFold suite <img width="45" height="43" alt="image" src="https://github.com/user-attachments/assets/fd3b848c-4565-4347-b881-813c0e27ae09" />


Browser-based  notebooks to calculate protein coevolution and see it on contact maps, 3D structures and protein networks. No installation or programming is needed: open a notebook, paste a sequence or upload a structure, and run the cells from top to bottom.

[![Open the CoEVFold suite in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/Co_evolution_suite.ipynb)

## Which notebook should I use?

| I want to… | Notebook | Open |
|---|---|---|
| See the coevolution (contact) map of a single protein | `Simple_GREMLIN.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/Simple_GREMLIN.ipynb) |
| Map coevolution between different proteins onto a complex structure (heteromers) | **CoEVFold** – `Co_EVFold_Heteromer.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/Co_EVFold_Heteromer.ipynb) |
|  CCMpred version | `Co_EVFold_Heteromer_CCMpred.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/Co_EVFold_Heteromer_CCMpred.ipynb) |
|  plmc version (the EVcouplings engine) | `Co_EVFold_Heteromer_plmc.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/Co_EVFold_Heteromer_plmc.ipynb) |
  | Heteromers using preexisting matrix (e.g. PyCoM) | `CUSTOM_HEATMAP_CoEVFold.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/CUSTOM_HEATMAP_CoEVFold.ipynb) |
| Find where copies of the same protein assemble (homo-oligomers) | **CoEVFold:4D** (CoEVRank) – `CoEVRank.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/CoEVRank.ipynb) |
| Heteromers using preexisting matrix | `CUSTOM_HEATMAP_CoEVFold_4D.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/CUSTOM_HEATMAP_CoEVFold_4D.ipynb) |
| Rank which proteins in a set are most likely to interact, as a network | **CoEVMapper** – `Bootstrapped_CoEV_Mapper.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/Bootstrapped_CoEV_Mapper.ipynb) |
| Benchmark with STRING, Chris's new default | `CoEV_Mapper_with_STRING.ipynb` | [Colab](https://colab.research.google.com/github/MishterBluesky/CoEVFold/blob/main/CoEV_Mapper_with_STRING.ipynb) |

## The tools

**CoEVFold (heteromers).** Upload a structure or model of a complex (PDB or mmCIF, e.g. from AlphaFold). The notebook reads the chain sequences, builds a paired MMseqs2 alignment, calculates coevolution and draws inter-protein coevolution onto the structure as a PyMOL session (`.pse`), together with a contact map and a table of the most strongly coevolving residue pairs. Identical chains are treated as copies of one protein, so complexes of up to two different proteins can have any number of chains. If the structure is experimental, the top-L/5 cells report how many of the top-ranked inter-protein pairs are real contacts.

**CoEVFold:4D (homo-oligomers, also called CoEVRank).** Coevolution between copies of the same protein cannot be separated from coevolution within one copy by sequence alone. CoEVFold:4D therefore compares the coevolution map with the contact map of a monomer (or lower-order oligomer) and finds coevolution the monomer does not explain. In the structure-guided mode, you also upload a model of the multimer and the notebook shows which of the unexplained pairs it explains; in the unguided mode, the unexplained pairs and their clusters point to where an interface may form. The top-L/5 cell reports, as a curve against the Z-score threshold, how many top-ranked pairs are monomer contacts and how many of the remaining pairs are contacts between protomers.

**CoEVMapper (networks).** Paste the sequences of a set of proteins separated by `:`. CoEVMapper reduces the joint alignment by bootstrapped column sampling so that large sets fit in memory, and ranks every protein pair by its coevolution. Outputs are a ranked table, heatmaps and an interaction network. The STRING version also retrieves STRING v12 scores for all pairs and compares the two by rank correlation and ROC analysis.

## Choosing the coupling method

The coupling step is interchangeable, and everything downstream (APC correction, Z-scoring, contact masking, 3D mapping and networks) is applied in the same way whichever you choose.

- **GREMLIN** (default): the TensorFlow implementation of GREMLIN, run on the GPU for a fixed 100 iterations. Fastest option in Colab.
- **CCMpred** and **plmc**: alternative pseudo-likelihood methods, in their own versions of the heteromer notebook. plmc is the coupling engine of EVcouplings.
- **Your own matrix** (`CUSTOM_HEATMAP_…` notebooks): skip the alignment and coupling steps and upload a pre-computed matrix, for example from [PyCoM](https://pycom.brunel.ac.uk/), CCMpred, plmc or a saved GREMLIN run. Accepted files:
  - an L × L matrix as `.npy`, `.npz`, `.csv`, `.tsv`, `.txt` or a CCMpred `.mat` file;
  - a pair list with three columns, `i j score`;
  - a plmc couplings file (`-c` output).

  A PyCoM matrix can be saved in Python with `numpy.save("matrix.npy", matrix)`. Tick or untick *Apply APC* depending on whether your matrix is already APC-corrected. For complexes, the matrix must cover the chains joined in the order printed by the notebook.

## Recommended settings

| Tool | Z-score cut-off | Distance shown in PyMOL | Notes |
|---|---|---|---|
| CoEVFold (heteromers) | 2.5–3 for inter-protein pairs | up to 20 Å (Cα–Cα) | Up to ~1,000 residues in a standard Colab session, ~1,500 on an A100 GPU |
| CoEVFold:4D | 1.5 (CoEVRank tables use ≥ 1) | up to 20 Å | Monomer and multimer files must match the input sequence |
| CoEVMapper | relative to each protein's own (self) coevolution | – | Include proteins known not to interact as controls |

For the alignment we recommend MMseqs2 with at least 50% coverage. Coevolution needs depth: the effective number of sequences divided by the length (Neff/L) should ideally be 1 or more, and the notebooks warn when it is lower.

## What coevolution can and cannot tell you

In our benchmark of 18 complexes of known structure, coevolution within proteins was always recovered more clearly than coevolution between proteins. The interface was resolved, with a top-L/5 precision of 0.8 or more, in 7 of the 18 complexes, mostly only after applying a Z-score threshold of about 2–3. In homo-oligomers (CoEVFold:4D), 91–92% of the top-ranked pairs of SpoIIIE and MlaD were monomer contacts. Of the pairs the monomer did not explain, 25% (SpoIIIE) and 47% (MlaD) were contacts between protomers, compared with 2% or less expected by chance.

Predictions are most reliable for:
- buried contacts between packed helices or paired β-strands;
- conserved domains with deep alignments (Neff/L above 1).

They are less reliable for:
- solvent-exposed contacts, disordered or poorly covered regions and flexible linkers;
- contacts present in only one conformation, and transient interactions;
- paralogous families, where pairing sequences by species is ambiguous;
- homo-oligomeric interfaces, unless a monomer structure is used to separate them (CoEVFold:4D).

Please also keep in mind:
- **Structure models can be wrong.** AlphaFold produces a considerable fraction of false-positive interfaces, especially in low-ipTM models and for hydrophobic or transmembrane surfaces. Mapping coevolution onto an incorrect model can make a false interface look supported.
- **Look for clusters, not single pairs.** An interface is more convincing when several strong pairs cluster together and the cluster is seen across alternative models.
- **Coevolution is evidence, not proof.** It can also reflect shared pathways or environments, alternative conformations, or alignment artefacts. Treat it as supporting evidence and test key pairs experimentally where possible.

## Computing requirements

All notebooks run in a free Google Colab session (tested on an NVIDIA T4 GPU with 12.7 GB of system RAM). For a ~950-residue complex, the coupling step took 114 s with GREMLIN, 116 s with CCMpred and 2,060 s with plmc. Across the benchmark complexes (159–833 residues), GREMLIN took 3.6–160 s and used at most 8.8 GB of RAM. For most queries, the MMseqs2 search takes longer than the coupling calculation. Memory grows with the square of the length, which limits a standard session to about 1,000 residues.

## Related tools

- [ConservFold2](https://colab.research.google.com/drive/1Lv-akfLE7kTCFCWaEyHAtsPCeXYD3xvH?usp=sharing): conservation mapped onto structures, with WebLogo.
- [AlphaMatrix](https://colab.research.google.com/drive/1HU_KFWKyVLtz6u4MEMnZlXayaCYrQMw8?usp=sharing): runs batches of AlphaFold predictions to screen for interactions (no coevolution).

## Citation

If you use the CoEVFold suite, please cite:

> Graham CLB, Cremona L, Robin L, Rodrigues CDA. CoEVFold suite: user-friendly pipelines to visually represent protein coevolution. 2026.

Please also cite the methods you use: GREMLIN (Balakrishnan et al. 2011; Kamisetty et al. 2013), MMseqs2 (Steinegger & Söding 2017), and, where used, CCMpred (Seemayer et al. 2014), plmc (Hopf et al. 2017), PyCoM (Glass et al. 2024) and STRING (Szklarczyk et al. 2023).

## Credits and licences

The GREMLIN code used here is adapted from GREMLIN_TF by Sergey Ovchinnikov and Peter Koo (Beerware licence, Revision 42); the original MATLAB GREMLIN was written by Hetu Kamisetty (Baker lab). The alignment set-up uses ColabFold/ColabDesign and MMseqs2. CCMpred and plmc are distributed under their own licences. The notebooks were designed by Chris L. B. Graham (Rodrigues lab, University of Warwick); contact christopher.graham@pasteur.fr

*Update: Conda has been replaced by Mamba in these notebooks, which reduces crashes in recent Colab versions.*


