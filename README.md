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
  cmd                vawk command syntax is exactly the same as awk syntax with
                          a few additional features. The INFO field can be split using
                          the I$ prefix and the SAMPLE field can be split using
                          the S$ prefix. For example, I$AF prints the allele frequency of
                          a variant and S$NA12878 prints the entire SAMPLE field for the
                          NA12878 individual

                          The SAMPLE field can be further split based on the keys in the
                          FORMAT field of the VCF (column 9). For example, S$NA12877$GT
                          returns the genotype of the NA12878 individual.
                          
                          ex: '{ if (I$AF>0.5) print $1,$2,$3,I$AN,S$NA12878,S$NA12877$GT }'

  vcf                VCF file (default: stdin)

optional arguments:
  -h, --help         show this help message and exit
  -v VAR, --var VAR  declare an external variable (e.g.: SIZE=10000)
  --header           print VCF header
  --debug            debugging level verbosity
```


##Examples

```
$ zcat /mnt/hall14_local/cc2qe/platinum/ceph1463.lumpy.vcf.gz | vawk '{if (S$NA12878$GT=="0/1" && $6>6) print S$NA12891$GT, I$EVTYPE}'  | head
# 0/1   PE_ALONE
# 0/0   PE_ALONE
# 0/1   PE_ALONE
# 0/1   PE_ALONE
# 0/0   PE_ALONE
# 0/1   SR_ALONE
# 0/0   PE_ALONE
# 0/1   PE_SR
# 0/0   PE_ALONE
# 0/1   PE_ALONE
```

```
$ zcat ceph1463.lumpy.vcf.gz | vawk 'S$NA12878$GT=="0/1"'
```

```
$ zcat ceph1463.lumpy.vcf.gz | vawk '{ if (I$SRSUP>3) print $1,$4,$5,$2,I$END, I$END-$2, I$SVLEN, I$EVTYPE, I$PESUP, I$SRSUP} ' | head
# 1 A   <DEL>   988595  988622  27  -27 PE_SR   1   9
# 1 C   <DEL>   991860  991981  121 -121    SR_ALONE    0   21
# 1 G   <DEL>   1142719 1143139 420 -420    PE_SR   1   12
# 1 C   <DEL>   1362765 1362821 56  -56 SR_ALONE    0   6
# 1 A   <DEL>   1530383 1530685 302 -302    PE_SR   2   86
# 1 T   <DUP>   1880413 1880554 141 141 SR_ALONE    0   8
# 1 G   <DUP>   1912966 1913504 538 538 SR_ALONE    0   4
# 1 T   <DEL>   2054429 2055813 1384    -1384   SR_ALONE    0   40
# 1 C   <DUP>   2212237 2213976 1739    1739    SR_ALONE    0   9
# 1 G   <DEL>   2837835 2837901 66  -66 SR_ALONE    0   113
```
