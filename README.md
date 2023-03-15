# NextFlow tutorial

Installing NextFlow
```bash
wget -qO- https://get.nextflow.io | bash
curl -s https://get.nextflow.io | bash
chmod +x nextflow
```
> Note: Add the nextflow executable into your $PATH (e.g. /usr/local/bin or /bin/)

You can clone the traning git repository from:
```bash
git clone https://github.com/nextflow-io/training.git
cd nf-trainin
```
## NextFlow Reproducibility 

Reproducibility is central to NextFlow, to achieve it, you need to check what version are you running `nextflow -v`, which could be `nextflow version 22.10.4.5836`. In the example, we need to configure nextfow version to `22.04.5`, exporting the nextflow version to specify `export NXF_VER=22.04.5`. Type `nextflow -v` to see depency changes.

```bash
$nextflow -v
$export NXF_VER=22.04.5
$nextflow -v
...
CAPSULE: Downloading dependency org.eclipse.jgit:org.eclipse.jgit:jar:5.2.1.201812262042-r
CAPSULE: Downloading dependency commons-codec:commons-codec:jar:1.10
nextflow version 22.04.5.5708
```

----------
## Running NextFlow
An example of `Hello Word!` using nextflow
```nextflow
#!/usr/bin/env nextflow                    ## shebang declaration

params.greeting = 'Hello world!'           ## declaring parameter greeting
greeting_ch = Channel.of(params.greeting). ## initialize a channel called greeting_ch

process SPLITLETTERS {                     ## begins the first process block defined SPLITLETTERS                               
    input:                                 ## inputs can be values (val), files or paths (path)or other qualifyers
    val x                                  ## tells the process to expect an input value (val) assigned to the variable 'x'


    output:                               ## output declaration for SPLITLETTERS
    path 'chunk_*'                        ## expect an output file(s) (path), with a filename starting with 'chunk_'

    """                                   ## three double quotes start and end the code block
    printf '$x' | split -b 6 - chunk_     ## command to execute a function
    """
}

process CONVERTTOUPPER {                  ## begins the second process block
    input:
    path y

    output:
    stdout

    """
    cat $y | tr '[a-z]' '[A-Z]'
    """
}

workflow {                               ## start of the workflow for each process
    letters_ch = SPLITLETTERS(greeting_ch)            ## execute the process SPLITLETTERS on the greeting_ch
    results_ch = CONVERTTOUPPER(letters_ch.flatten()) ## execute the process CONVERTTOUPPER on the letters channel letters_ch, which is flattened using the operator .flatten()
    results_ch.view{ it }                             ## The final output (in the results_ch channel) is printed to screen using the view operator
}
```

The script can run using `nextflow run hello.nf`
Generating the following output!
```nextflow
N E X T F L O W  ~  version 22.04.5
Launching `hello.nf` [silly_visvesvaraya] DSL2 - revision: 197a0e289a
executor >  local (3)
[5c/85e7d3] process > SPLITLETTERS (1)   [100%] 1 of 1 ✔
[3e/f3125a] process > CONVERTTOUPPER (2) [100%] 2 of 2 ✔
HELLO
WORLD!
```

You can also modify the declared parameter, in this case `greeting`, using \
`nextflow run hello.nf --greeting "Hola mundo\!"`
```nextflow
N E X T F L O W  ~  version 22.04.5
Launching `hello.nf` [compassionate_curran] DSL2 - revision: 197a0e289a
executor >  local (3)
[a2/7f471a] process > SPLITLETTERS (1)   [100%] 1 of 1 ✔
[44/87f21f] process > CONVERTTOUPPER (1) [100%] 2 of 2 ✔
UNDO!
HOLA M
```
You can see that the `Hola mundo!` string is slightly larger, so the commands split the word differently. Also, notice that for mac, `!` needed a `\` scape variable.





