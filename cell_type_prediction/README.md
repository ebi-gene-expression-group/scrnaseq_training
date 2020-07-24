# Generating consensus predictions for single-cell RNA-seq cell types

## Objectives
This tutorial is designed to provide instructions on running the single-cell RNA-seq cell types prediction workflow in Galaxy.

## Introduction 
When a new, unlabelled scRNA-seq expression data becomes available, it is rarely annotated with cell types. Rigorous annotation requires domain-specific expertise that is not always available in the laboratories that generate the data. To address this problem, a variety of tools have been developed for computational classification of cell types. These tools employ a variety of statistical and mathematical methods. Naturally, each method has inherent advantages and drawbacks. 

This framework employs a widely-used strategy for data classification - it uses multpile tools to obtain predictions, combines them and generates a consensus output. More specifically, it generates predictions for novel data using multiple libraries of classifiers trained on existing datasets with high-quality annotations. Predictions for each cell are initially filtered based on tool-specific scores, followed by calculating the frequency of each candidate label as well as [semantic similarity](https://en.wikipedia.org/wiki/Semantic_similarity) across predictions. These metrics allow to ascertain the reliability and agreement of predictions. Finally, a table with 3 top candidate labels, corresponding metrics per cell and datasets of origin is produced. 

## Pre-requisites 
A basic understanding of the Galaxy system is required. See [this tutorial](https://training.galaxyproject.org/training-material/topics/introduction/tutorials/galaxy-intro-short/tutorial.html) for usage instructions. 

## What you will need
* Expression data in the format of [10X Genomics directory](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/output/matrices) with the following files: 
    * `matrix.mtx` - gene-by-cell matrix holding expression data 
    * `genes.tsv` - list of genes (corresponding to rows of the matrix)
    * `barcodes.tsv` - list of cell IDs (matrix columns)
You can download an example dataset of PBMC-derived B cells from [this link](https://www.ebi.ac.uk/gxa/sc/experiments/E-MTAB-6386/downloads). Data can be uploaded to Galaxy either directly as files or via a link (`Paste/Fetch data` button) in the upload page. 

* (OPTIONAL) File with pre-trained classifiers IDs. By default, all available classifiers trained on SCXA datasets are retrieved automatically. However, if you want to restrict the range of classifiers, you can specify the corresponding training dataset accessions in a `.yaml`-formatted config file. See example [here](https://github.com/ebi-gene-expression-group/atlas-data-import/blob/master/example_user_config.yaml).   

* (OPTIONAL) Exclusions file in `.yaml` format (needed to filer out unlabelled cells and trivial terms; see example [here](https://www.ebi.ac.uk/~a_solovyev/prod_testing_data/exclusions.yaml))

## Configuring and running the workflow 
The workflow is run in Galaxy. Galaxy is a user-friendly service that provides a graphical user interface for using bioinformatics tools. In your active Galaxy instance, look for the workflow called `cell_types_prod_control_wf`. More precisely, this is a workflow-of-workflows that runs predictions from individual tools in a modular manner (see figures below). It consists of the following child workflows: 
* `garnett_prod_workflow`
* `scmap_cluster_prod_workflow`
* `scmap_cell_prod_workflow`
* `scpred_prod_workflow`

The key parameters are configurable at the level of the outer workflow, however, in cases when the individual tool parameters need to be modified, you can refer to the child workflows' configuration. 
![Fig.1 Galaxy Workflows](workflows.png)
![Fig.2 Control Workflow](control_wf.png)

To execute the workflow, press the `run` button on the top-right (view from the workflow editor). This will direct you to the configuration page, as shown below. 

1. `Garnett gene database` - name of Bioconductor database of genes used by Garnett tool (species-dependent: `org.Hs.eg.db` for _Homo Sapiens_, `org.Mm.eg.db` for _Mus Musculus_, etc.)
2. `Expression Matrix` - matrix with expression data, as explained above
3. `Barcodes` - file with cell IDs 
4. `Genes` - file with gene IDs
5. `scmap-cell: cell type column` - Name of the cell type field in scmap-cell classifier (`inferred.cell.type` by default)
6. `Exclusions` - optional yaml file with excluded terms (described above) 
7. `Cluster Column` - this parameter indicates the name of the counts field in expression matrix. Set to `counts` unless initially specified otherwise.

After all necessary parameters are set, hit the `Run Workflow` button in the top right to trigger workflow execution. 

## Results 
After the workflow is run, new entries will appear in the history on the right-hand side of the screen. The output table will appear under `Cell types - get consensus outputs` entry. 

Two output tables are produced, the first containing a list of all predicted labels for each cell and the second containing top 3 candidate labels for each cell along with corresponding statistics. The following statistics are calculated: 

1) Weighted score: for each candidate label, we multiply the proportion of predictions that nominate it by the average score of the corresponding tools. Labels with the highest weighted score are chosen as the 3 most reliable predictions. 
2) Agreement rate. Defined as 1/(number of unique predicted labels). Ideally, all tools should predict the same label for each cell. Widely diverse predictions are thus penalised. 
3) Proportion of ‘unlabelled’ in the list of predictions. We are expecting the majority of tools to find a corresponding label. Unlabelled cells are penalised. 
4) Semantic similarity across predicted labels. Correctly predicted cells are likely to have labels that are highly similar among them (think high precision). 
5) Aggregated score: again, scores  2-4 are averaged to produce combined score. Cells are sorted by this score. Those cells that have low score are less likely to be correctly predicted. 



