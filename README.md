vawk
====

VCF parser with awk-like arithmetic

##Table of Contents

1. [Usage](#usage)
2. [Examples](#examples)

##Usage
```
usage: vawk [-h] [-v VAR] [--header] [--debug] cmd [vcf]

positional arguments:
  cmd                vawk command (using awk syntax)
                          ex: '{ if (I$AF>0.5) print $1,$2,$3,I$AN,S$NA12878,S$NA12877$GT }'
  vcf                VCF file (default: stdin)

optional arguments:
  -h, --help         show this help message and exit
  -v VAR, --var VAR  declare an external variable (e.g.: SIZE=10000)
  --header           print VCF header
  --debug            debugging level verbosity
```


##Examples