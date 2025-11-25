# Python_Script_for_GEO_Dataset_Retrieval_and_Differential_Gene_Expression_DEG_Analysis
Python_Script_for_GEO_Dataset_Retrieval_and_Differential_Gene_Expression_DEG_Analysis
# 🧬 Differential Gene Expression (DEG) Analysis with GEOparse

This repository contains a Python script for performing Differential Gene Expression (DEG) analysis on publicly available microarray data from the **Gene Expression Omnibus (GEO)** database using the `GEOparse` and `scipy` libraries.

The script automates the process of:
1.  Downloading a specified GEO Series (GSE) dataset.
2.  Constructing a gene expression matrix.
3.  Performing a two-sample t-test to calculate p-values and log fold changes (logFC).
4.  Identifying statistically and biologically significant differentially expressed genes (DEGs).
5.  Generating a **Volcano Plot** for visualization.

## 🚀 Getting Started

### Prerequisites

You need **Python 3** and the following libraries installed. You can install them using `pip`:

```bash
pip install GEOparse pandas matplotlib seaborn scipy numpy
Usage

    Clone the repository:
git clone [Your Repository URL]
cd [Your Repository Folder]
Run the Jupyter Notebook/Python Script: The core analysis is performed in the provided Jupyter Notebook or Python script.

The script is configured to run analysis on GSE57691 by default, which studies differential gene expression in human abdominal aortic aneurysm and atherosclerosis.

To run a different dataset, modify the gse_id variable in the script:
# Step 1: Download GEO dataset
gse_id = "GSE57691"   # <-- Change this to your desired GEO Series ID
gse = GEOparse.get_GEO(geo=gse_id, destdir=".")
# ... rest of the script
Review Outputs: Running the script will generate the following output files in your directory:
File Name,Description
expression.csv,"The raw, normalized gene expression matrix for all samples and probes."
DEG.csv,"Full results of the Differential Expression Analysis, including logFC and pval for all genes."
significant.csv,A filtered list of genes that meet the significance criteria.
volcano_plot.png,A visual representation of the DEG results.
Analysis Results (Example: GSE57691)

The following results were generated using the default GSE57691 dataset, which compared 34 control samples (first half of the dataset) to 34 treated samples (second half of the dataset).

Key Findings

    Total Samples: 68

    Total Probes/Genes: 39,426

    Significance Thresholds:

        P-value: <0.05

        Fold Change: ∣logFC∣>1 (2-fold change)

    Total Significant DEGs: 42 genes were identified as statistically and biologically significant.

Volcano Plot

The volcano plot is a graphical representation of the magnitude and statistical significance of the changes in gene expression.

Note: The volcano_plot.png is generated when the script is run.

Interpretation

    X-axis (Log Fold Change): Measures the magnitude of expression difference between the treated and control groups. Positive values indicate up-regulation in the treated group, and negative values indicate down-regulation.

    Y-axis (-log10(p-value)): Represents the statistical significance. Higher values mean lower (more significant) p-values.

    Dashed Lines:

        Horizontal Blue Line: The p-value significance threshold (p-value=0.05, which is -log10(0.05)≈1.3).

        Vertical Green Lines: The logFC threshold (∣logFC∣=1).

    Red Points (Significant Genes): These 42 genes fall outside the vertical green lines AND above the horizontal blue line, indicating they are the most interesting candidates for further research.

🛠️ Methodology Details

Data Processing

The script uses GEOparse to download the SOFT file for the specified GSE and extracts the 'VALUE' column (typically the normalized expression level) from each sample (GSM). It then merges these into a single Pandas DataFrame (expr_df), using the 'ID_REF' as the index.

Differential Expression Analysis (DEA)

    Group Splitting: The script makes a simple assumption: the first half of the samples (n//2) are the control group, and the second half are the treated group.

    Statistical Test: A two-sample independent t-test (scipy.stats.ttest_ind) is performed for each gene to calculate the p-value.

    Log Fold Change (logFC): The logFC is calculated as the difference between the mean expression of the treated group and the mean expression of the control group:
    logFC=Mean(Treated)−Mean(Control)

Filtering

The final set of significant DEGs is defined by applying the following standard thresholds:
Significance=(p-value<0.05)AND(∣logFC∣>1)

🔗 Project Link

For more details on the GEO dataset used in the example, visit: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE57691
