## Fastqc

## Trim Galore
 ```
 trim_galore \\
            $args \\
            --cores $cores \\
            --gzip \\
            $c_r1 \\
            $tpc_r1 \\
            ${prefix}.fastq.gz
 ```

## MultiQC

## Hisat2
```
INDEX=`find -L ./ -name "*.1.ht2" | sed 's/.1.ht2//'`
        hisat2 \\
            -x \$INDEX \\
            -U $reads \\
            $strandedness \\
            --known-splicesite-infile $splicesites \\
            --summary-file ${prefix}.hisat2.summary.log \\
            --threads $task.cpus \\
            $seq_center \\
            $unaligned \\
            $args \\
            | samtools view -bS -F 4 -F 256 - > ${prefix}.bam
```

```mermaid
flowchart TD

A[Check the quality - Fastqc] --> B{Trimming};
B --> C(Trimmomatic);
B --> D(Trim galore);
C --> E[MultiQC];
D --> E[MultiQC];
E --> F[Alignment];
F --> G(Star and Salmon);
F --> H(Star via sem);
F --> I(Hisat);
G & H & I  ---> J[Post aignment processing - Samtools, picard];
J --> K([Gene expression analysis])
K --> L & M & N & OA;
L[RSeQc]; M[Preseq];N[featureCounts];OA[DeSeq 2]
```

