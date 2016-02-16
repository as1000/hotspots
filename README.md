![foobar](http://i.giphy.com/LwhhZsEHFQgSs.gif)

# Identifying recurrent mutations in cancer reveals widespread lineage diversity and mutational specificity
## Software and dataset

### Description: 
This is a method to identify population-scale recurrent mutations in cancer, based on a binomial
statisical model incoporating underlying mutational processes including nucleotide context
mutability, gene-specific mutation rates, and major expected patterns of hotspot mutation emergence

### Dependencies:
Need R Version 3.0.2 or higher
Install dependent packages (data.table, IRanges, BSgenome.Hsapiens.UCSC.hg19) as follows:

install.packages("data.table")
source("http://bioconductor.org/biocLite.R")
biocLite("IRanges","BSgenome.Hsapiens.UCSC.hg19")

Usage:
```
./hotspot_algo.R
    --input-maf=[REQUIRED: mutation file]
    --rdata=[REQUIRED: Rdata object with necessary files for algorithm]
    --true-positive=[REQUIRED: List of known true-positives]
    --output-file=[REQUIRED: output file to print statistically significant hotspots]
    --gene-query=[OPTIONAL (default=all genes in mutation file): List of Hugo Symbol in which to query for hotspots]
    --homopolymer=[OPTIONAL: BED file of homopolyer regions in hg19 for false positive filtering]
    --align100mer=[OPTIONAL: BED file of hg19 UCSC alignability track for 100-mer length sequences for false positive filtering]
    --align24mer=[OPTIONAL: BED file of hg19 UCSC alignability track for 24-mer length sequences for false positive filtering]
    --filter-centerbias=[OPTIONAL (default=FALSE): TRUE|FALSE to identify false positive filtering based on mutation calling center bias]
```
Command to run hotspot algorithm on genes listed in file genes_of_interest.txt:
```
./hotspot_algo.R \
	--input-maf=pancancer_unfiltered.maf \
	--rdata=hotspot_algo.Rdata \
	--gene-query=genes_of_interest.txt \
	--true-positive=true_positive_hotspots.txt \
	--output-file=sig_hotspots.txt
```

### Contents:
`hotspot_algo.R` - R script to execute hotspot detection algorithm

`hotspot_algo.Rdata` - Rdata object with necessary files for algorithm (mutability, expression/germline filters, etc)

`funcs.R` - R script of functions necessary for proper execution of hotspot_algo.R

`true_positive_hotspots.txt` - List of true positive hotspots used as a part of putative false positives filtering

`genes_of_interest.txt` - Sample list of genes for hotspot detection

`homopolymer_repeats.bed` - BED file with start, stop, sequence composition, and homopolymer length of simple repeats found in codng regions (hg19)

`minimalist_test_maf.txt` - minimalist MAF needed from maf2maf (https://github.com/mskcc/vcf2maf)

## Notes:
`--align100mer` and `--align24mer` are optional filters based on how uniquely k-mer sequences align to a region of the hg19 genome. Note, both filters were used as part of this analysis. See more information at http://genome.ucsc.edu/cgi-bin/hgFileUi?db=hg19&g=wgEncodeMapability.
The use of these filters will require downloading the 100-mer and 24-mer alignability tracks from UCSC that are not included here:
	http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeMapability/wgEncodeCrgMapabilityAlign100mer.bigWig
	http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeMapability/wgEncodeCrgMapabilityAlign24mer.bigWig
Convert these downloaded bigWig to bedgraph format, following instructions here: http://genome.ucsc.edu/goldenpath/help/bigWig.html
