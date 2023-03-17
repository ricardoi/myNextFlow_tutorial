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

## Modifying params

NextFlow script can have fixed params, by hardcoding the `$PATH`., this example is script `script3.nf` modified in class 1.
```nextflow
/*
 * input parameters
 */
param.reads = $projectID/data/ggal/gut_{1,2}.fq"
params.transcriptome_file = "$projectDir/data/ggal/transcriptome.fq"
params.multiqc = "$projectID/multiqc"
params.outdir = "results"

log.ingo """\
        R N A S E Q - N F  P I P E L I N E
        ==================================
        trasnscriptome: ${params.transcriptome_file}
        reads         : ${params.reads}
        outdir        : ${params.outdir}
        """
        .stripIndent()
        
Channel  ## we created a channel for read pairs using from File and set
        .fromFilePairs(params.reads, checkIfExists: true) ## added checkIfExists for sanity check
        .set{reads_pairs_ch}
read_pairs_ch.view()  ## This is to watch the channeal read pairs
```

You can run the script with `nextflow run script3.nf`, which will read the parameters within the regular `$PATH` speciefied in the script, and a way to modify the parameters for reads, and specify to read a different set of fastq file or all of them using this syntax \
`$ nextflow run script3.nf --reads "data/ggal/*_{1,2}.fq"`

    
