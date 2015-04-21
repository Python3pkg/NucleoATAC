The nucleoatac command line function contains several subcommands used for calling nucleosomes.  The "run" subcommand is a wrapper for the other three using standard options-- for more flexibility, run "occ" then "vprocess" then "nuc". For each subcommand, options can be viewed as `nucleoatac subcommand --help`.

###Necessary Files
Prior to using nucleoatac to call nucleosomes, you will need at least three types of files:

1) Bam file with aligned reads.  These generally will be filtered for reads not mapping to mitochondria & reads with high mapping quality.

2) Fasta file with genome used in alignment.  This file must be indexed (using [samtools](http://www.htslib.org/) faidx)

3) Sorted bed file with regions for which nucleosome analysis is to be performed.  These regions will generally be broad open-chromatin regions (i.e. regions called by MACS2 with the --broad flag).  It is potentially advisable to extend these regions a bit (e.g. using bedtools slop).  Regions should not overlap so it is advisable to use bedtools merge on these regions.

###run
For calling nucleosomes with mostly default parameters, use
```
nucleoatac run --bed <bedfile> --bam <bamfile> --fasta <fastafile> --out <output_basename>
```

Outputs:

See outputs for occ, vprocess, and nuc


###occ
```
nucleoatac occ --bed <bedfile> --bam <bamfile> --out <output_basename>

```

Outputs:

* *output_basename.occ.bedgraph.gz*: Bedgraph track with nucleosome occupancy score

* *output_basename.nuc_dist.txt* and *output_basename.nuc_dist.eps*: text file and EPS plot with estimate of fragment size at nucleosomes
    
* *output_basename.fragmentsizes.txt*: Text file with fragment size distribution within input peaks

* *output_basename.occ_fit.txt* and *output_basename.occ_fit.eps*: text file and EPS plot of model for NFR and nucleosomal distributions

###vprocess
```
nucleoatac vprocess --sizes <sizes_file> --out <output_basename>
```
The <sizes_file> will generally be the output_basename.nuc_dist.txt output from `occ`.  

Outputs:

* *output_basename.VMat*: text file with normalized V-plot for cross-correlation

###nuc
```
nucleotac nuc
```


* *output_basename.nucpos.bed*:  Nucleosome dyad calls text file.  Columns are: (1) chrom, (2) dyad position (0-based), (3) dyad position(1-based), (4) z-score, (5) log likelihood ratio, (6) normalied nucleoatac signal value, (7) smoothed nucleoatac signal value, (8) cross-correlation signal value before normalization, (9) number of potentially nucleosome-sized fragments, (10) number of fragments smaller than nucleosome sized, (11) "fuzziness"  (measure of how wide signal peak is)

* *output_basename.nucpos.redundant.bed*: Includes nucleosome position calls that were within the minimum separation for non-redundant calls. 

* *output_basename.nucleoatac_signal.bedgraph.gz*: Bedgraph track with normalized cross-correlation signal

* *output_basename.nucleoatac_signal.smooth.bedgraph.gz*: Smoothed version of output_basename.nucleoatac_signal.bedgraph.gz
Also includes only positive signal

* *output_basename.occ.bedgraph.gz*: Bedgraph track with nucleosome occupancy score



