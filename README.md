# Network Pharmacology of Indole-3-Carbinol in Endometriosis: Target Prediction, PPI Hub-Gene Analysis and Functional Enrichment

An *in silico* network pharmacology study of **Indole-3-carbinol (I3C)** as a candidate multi-target agent for **endometriosis**.

> **Status:** computational prediction only. No experimental validation (docking, MD simulation, in vitro/in vivo) has been performed yet.

## Summary of findings

| Step | Result |
|---|---|
| Predicted I3C targets (SwissTargetPrediction) | 98 unique targets in the Venn input (kinases are the largest class among the top 15) |
| Endometriosis-associated genes (GeneCards) | 4,074 |
| Overlapping targets (I3C ∩ endometriosis) | **47** |
| PPI network (STRING) | 47 nodes, 168 edges |
| Top 10 hub genes (Cytoscape, MCC) | **PIK3CA, ESR1, AR, PARP1, HDAC1, TOP2A, CYP19A1, PGR, CHEK1, KDM1A** |
| Enrichment (DAVID, hub genes) | Steroid/estrogen receptor signalling, transcription regulation, chromatin remodelling, DNA damage response |
| ADME (SwissADME) | High GI absorption, 0 Lipinski violations, BBB permeant |
| Toxicity (ProTox 3.0) | Predicted LD50 1000 mg/kg, class 4 |

## Workflow

```
I3C (PubChem SMILES)
   ├─► SwissTargetPrediction ──► I3C targets ─────────────┐
   │                                                       ├─► Venny 2.1 ─► 47 common targets
   └─► GeneCards ─────────────► endometriosis genes ──────┘
                                                              │
                         STRING (PPI) ◄──────────────────────┘
                              │
                     Cytoscape (MCC) ─► top 10 hub genes
                              │
                     DAVID (GO BP/CC/MF) + KEGG
   I3C ─► SwissADME + ProTox 3.0 (drug-likeness & toxicity)
```

## Tools and databases

| Purpose | Tool |
|---|---|
| Target prediction | [SwissTargetPrediction](http://www.swisstargetprediction.ch/) |
| Disease genes | [GeneCards](https://www.genecards.org/) (query: "endometriosis") |
| Overlap | Venny 2.1 |
| PPI network | [STRING](https://string-db.org/) (*Homo sapiens*) |
| Network analysis | Cytoscape + cytoHubba (MCC) |
| Enrichment | [DAVID](https://davidbioinformatics.nih.gov/), ShinyGO (KEGG rendering) |
| ADME | [SwissADME](http://www.swissadme.ch/) |
| Toxicity | [ProTox 3.0](https://tox.charite.de/protox3/) |

## Results

### 1. Target prediction
Top-15 predicted target classes: kinase 46.7%, cytochrome P450 20%, oxidoreductase 20%, isomerase 6.7%, transferase 6.7%.

![Target class](01_target_prediction/top15_target_class_pie.png)

Raw data: [`SwissTargetPrediction_I3C.csv`](01_target_prediction/SwissTargetPrediction_I3C.csv)

### 2. Disease targets
Endometriosis-related genes from GeneCards: [`GeneCards_endometriosis.tsv`](02_disease_targets/GeneCards_endometriosis.tsv)

### 3. Overlap
51 I3C-only, **47 shared**, 4,027 endometriosis-only.

![Venn](03_overlap_venn/venn_I3C_vs_endometriosis.png)

### 4. PPI network
STRING network of the 47 common targets (168 edges). NR4A2, PIM2 and the CA1–CA2 pair are disconnected from the main network.

![PPI](04_PPI_STRING/PPI_network_47_nodes.png)

### 5. Hub genes
Ranked by MCC (cytoHubba):

| Rank | Gene | MCC score |
|---|---|---|
| 1 | PIK3CA | 1530 |
| 2 | ESR1 | 1428 |
| 3 | AR | 1008 |
| 4 | PARP1 | 920 |
| 5 | HDAC1 | 718 |
| 6 | TOP2A | 506 |
| 7 | CYP19A1 | 387 |
| 8 | PGR | 360 |
| 9 | CHEK1 | 336 |
| 10 | KDM1A | 284 |

![Hubs](05_hub_genes_Cytoscape/MCC_top10_hub_network.png)

### 6. Functional enrichment (hub genes)
DAVID GO analysis of the 10 hub genes. Selected terms passing FDR < 0.05:

- **BP:** positive regulation of transcription by RNA Pol II, negative regulation of gene expression, chromatin remodelling, negative regulation of transcription by RNA Pol II
- **CC:** protein-containing complex, chromatin, transcription regulator complex, nucleoplasm, transcription repressor complex
- **MF:** enzyme binding, chromatin binding, estrogen response element binding, nuclear steroid receptor activity, steroid binding

![GO](06_enrichment_GO_KEGG/GO_enrichment_barplot.png)

KEGG: PI3K (PIK3CA) mapped on the longevity-regulating pathway.

![KEGG](06_enrichment_GO_KEGG/KEGG_longevity_regulating_pathway.png)

Full tables: [`DAVID_GO_BP.csv`](06_enrichment_GO_KEGG/DAVID_GO_BP.csv), [`DAVID_GO_CC.csv`](06_enrichment_GO_KEGG/DAVID_GO_CC.csv), [`DAVID_GO_MF.csv`](06_enrichment_GO_KEGG/DAVID_GO_MF.csv)

### 7. ADME and toxicity
| Property | Value |
|---|---|
| Formula / MW | C9H9NO / 147.17 g/mol |
| TPSA | 36.02 Å² |
| Consensus LogP | 1.38 |
| GI absorption / BBB | High / Yes |
| Pgp substrate | No |
| CYP1A2 inhibitor | Yes (others: No) |
| Lipinski violations | 0 |
| Bioavailability score | 0.55 |
| Synthetic accessibility | 1.05 |
| ProTox 3.0 | LD50 1000 mg/kg, class 4; active for respiratory toxicity, BBB, AhR, aromatase, mitochondrial membrane potential, TTR |

Files: [`SwissADME_I3C.csv`](07_ADME_toxicity/SwissADME_I3C.csv), [`ProTox3_I3C_report.pdf`](07_ADME_toxicity/ProTox3_I3C_report.pdf)

## Limitations
- Predictions depend on database versions and thresholds at the date of analysis (November 2026).
- Hub genes reflect network topology and need experimental validation.
- Next steps: molecular docking of I3C against the top hubs (PIK3CA, ESR1, AR, PARP1, HDAC1), MD simulation, and expression-data validation (e.g. GEO endometriosis datasets).

## Repository structure
```
├── 01_target_prediction/
├── 02_disease_targets/
├── 03_overlap_venn/
├── 04_PPI_STRING/
├── 05_hub_genes_Cytoscape/    (includes .cys session)
├── 06_enrichment_GO_KEGG/
└── 07_ADME_toxicity/
```

## Author
Vaishnavi Parthiban

## License
   Released under the MIT License. See the LICENSE file. © 2026 Vaishnavi Parthiban.
