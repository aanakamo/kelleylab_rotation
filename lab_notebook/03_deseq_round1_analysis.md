# Analysis of Deseq Round 1 results

## Upset plots
For visualizing how much sharing of orthogroups there is between species for hibernation-related genes.
- separate plots for upregulated (positive log2FoldChange) and downregulated (negative log2FoldChange) genes
- combine each tsv into one file, adding species and tissue columns:
~~~
cd /Users/annenakamoto/ROTATION2/DESEQ2/DE_GENES
echo -e "species\ttissue\tgene_name\tgene_id\tOrthogroup\tbaseMean\tlog2FoldChange\tlfcSE\tstat\tpvalue\tpadj\tIHW_pvalue" > ALL.DEgenes.tsv
ls *.*.DEgenes.tsv | while read tsv; do
    sp=$(echo ${tsv} | awk -v FS="." '{ print $1; }')
    ts=$(echo ${tsv} | awk -v FS="." '{ print $2; }')
    awk -v s=${sp} -v t=${ts} -v OFS="\t" '!/log2FoldChange/ { print s, t, $0; }' < ${tsv} >> ALL.DEgenes.tsv
done
~~~
- R Notebook for making upset plots: [upsetplot.Rmd](https://github.com/aanakamo/kelleylab_rotation/blob/main/scripts/upsetplot.Rmd)
    - Added separate (subsetted) plots for: (filtering script: [filter_pav.py](https://github.com/aanakamo/kelleylab_rotation/blob/main/scripts/filter_pav.py))
        1. OGs with at least one gene from each species
        2. OGs with zero genes from one or more species
            - Separated by clade for OGs with zero genes from one or more species: (Bear, bat clade); (Lemur, squirrel, hamster clade)

### All species
Not subsetted

All DE genes
![All species, all DE genes](supporting_images/ALL_species.upset.png)
Upregulated DE genes
![All species, UPregulated DE genes](supporting_images/ALL_species.UP.upset.png)
Downregulated DE genes
![All species, UPregulated DE genes](supporting_images/ALL_species.DOWN.upset.png)

Notes:
- there seem to be ~3,000 OGs that contain both up and downregulated genes

### All upset plots
See here for all upset plots, including the subsetted ones:

![Upset plots](supporting_images/upset_plots.pdf)

#### DE SCOs
Table showing the total DE orthogroups and DE SCOs for each species,tissue
| species | tissue | DE_OG_count | DE_SCO_count |
| :---------------- | :------ | ----: | ----: |
| 13_lined_ground_squirrel | cerebrum | 2613 | 99 |
| 13_lined_ground_squirrel | hypothalamus | 4690 | 228 |
| 13_lined_ground_squirrel | medulla | 4836 | 213 |
| arctic_ground_squirrel | muscle | 964 | 54 |
| black_bear | bone | 8222 | 361 |
| brandts_bat | brain | 306 | 15 |
| brandts_bat | kidney | 1912 | 81 |
| brandts_bat | liver | 1907 | 91 |
| brown_bear | adipose | 6179 | 275 |
| brown_bear | liver | 4573 | 178 |
| brown_bear | muscle | 1678 | 71 |
| djungarian_hamster | blood | 174 | 7 |
| dwarf_lemur | white_adipose | 1221 | 61 |
| greater_horseshoe_bat | intestine | 1243 | 57 |
| monito_del_monte | brain | 179 | 11 |
| monito_del_monte | liver | 40 | 2 |
| monito_del_monte | muscle | 9 | 2 |
| syrian_hamster | white_adipose | 4260 | 176 |

## GO analysis
For investigating differences in hibernation-related gene function when shared across species and unique to species.

- First need to add the human proteome to the OrthoFinder results, to map human GO terms to the other species proteins
    - mapping script: [map_humanGO.py](https://github.com/aanakamo/kelleylab_rotation/blob/main/scripts/map_humanGO.py)
- Using [clusterProfiler](https://bioconductor.org/packages/release/bioc/html/clusterProfiler.html) gseGO() to find enriched terms
- R Notebook for making GO dotplots: [go_analysis.Rmd](https://github.com/aanakamo/kelleylab_rotation/blob/main/scripts/go_analysis.Rmd)
    - for each shared tissue, make dotplots for genes from shared OGs, and dotplots for genes from OGs unique to a species
    - each plot is separated by upregulated (activated) and downregulated (suppressed)

### All GO dotplots
See here for all GO dotplots:

![GO dotplots plots](supporting_images/go_plots.pdf)

### Gene trees
Observation: many OGs contain both up and down regulated genes. Take a look at some of these

