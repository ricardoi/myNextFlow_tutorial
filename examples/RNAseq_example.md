# RNA seq Example

A toy example of an RNAseq dataset.

Original `script1.nf` file to be modified 
```nextflow
params.reads = "$projectDir/data/ggal/gut_{1,2}.fq"
params.transcriptome_file = "$projectDir/data/ggal/transcriptome.fa"
params.multiqc = "$projectDir/multiqc"

println "reads: $params.reads"
```
Each script 1 to 7 will have an additional functionality.

