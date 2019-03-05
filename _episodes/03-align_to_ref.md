---
title: "Aligning to reference"
teaching: 30
exercises: 25
questions:
- "How can align reads to a reference genome"
keypoints:
- You will have to adpat programs to suit your biological questoins
- Check the biological plausability of any result coming from a compuational
  pipeline
---
# Aligning reads

In the previous episode, we took a high-level look at the quality
of each of our samples. Often the next step in our analyses would be to filter
the data to exclude low quality sequences or files. In our example data, none of
the reads are so bad we should exclude them, so we will continute with the next
step: aligning the reads to a reference genome.


## The tophat suite

There are a lot of programs available for aligning sequencing reads. For genomic
data, you might consider using `bwa`. For RNAseq data from eukaruotes, it's
important to use an aligner that is "intron-aware", which `bwa` is not.

For today's lesson, we are using and older but reliable program called `tophat`.

Tophat2 should be installed on your computer, so type `tophat2`.

~~~
$ tophat2
~~~
{: .bash}

Which will give you a lot of output, starting with something like this
TopHat maps short sequences from spliced transcripts to whole genomes.

~~~
Usage:
    tophat [options] <bowtie_index> <reads1[,reads2,...]> [reads1[,reads2,...]] \
                                    [quals1,[quals2,...]] [quals1[,quals2,...]]

Options:
    -v/--version
    -o/--output-dir                <string>    [ default: ./tophat_out         ]
    --bowtie1                                  [ default: bowtie2              ]
    -N/--read-mismatches           <int>       [ default: 2                    ]
    --read-gap-length              <int>       [ default: 2                    ]
    --read-edit-dist               <int>       [ default: 2                    ]
    --read-realign-edit-dist       <int>       [ default: "read-edit-dist" + 1 ]
    -a/--min-anchor                <int>       [ default: 8                    ]
    -m/--splice-mismatches         <0-2>       [ default: 0                    ]
    -i/--min-intron-length         <int>       [ default: 50                   ]
    -I/--max-intron-length         <int>       [ default: 500000               ]
    -g/--max-multihits             <int>       [ default: 20                   ]
    --suppress-hits
    -x/--transcriptome-max-hits    <int>       [ default: 60                   ]
    -M/--prefilter-multihits                   ( for -G/--GTF option, enable
.
.
.
~~~
{: .output}

This output diplays two things 

    - Usage: a basic schematic of how the program is meant to be used
    - Options: a lit of the flags and arguments we can pass to tophat to change
      its behaviour

As you can tell from all of the data that printed to screen, there is _a lot_ we
can change about any run of tophat2. You will probably need to take a little bit
of time to think about your organism's biology to tweak guess the best paramater
values. (Remember, automating the analyses with for loops and scripts make it
very easy to re-run any anylsis with different values!). 



The usage string tells use that tophat expects first a "bowtie-index"( which is
a reference genome) and then a set of sequencing reads. We should also set some
options. I know that fungi generally have much smaller introns than humans (most genomics
programs are written for humans...) so we should change the intron-length
argumetns. Let's call tophat 

~~~
$ tophat2 -i 30 -I 500 ref/Ecan_ref_genes fq/Ecan_01.fq 
~~~
{: .bash}

This shuold take a little while to running, printing information to your screen
as it goes. Eventually, it will finish producing a new directory called
`tophat_out`.  Use `ls` to see what the directory contains.


~~~
$ ls tophat_out/
~~~
{: .bash}

~~~
accepted_hits.bam  align_summary.txt  deletions.bed  insertions.bed  junctions.bed  logs  prep_reads.info  unmapped.bam
~~~
{: .output}


> ## Exercise
> Using your skills form earlier in teh workhop, how can you find out which of
> the files in this directory are the largest? Is `unmapped.bam` or
> `accepted_hits.bam` larger?


`.bam` files contain sequence alignments, while `.bed` files contain informatoin
abotu particualr regions in a genome. `align_summary.txt` seems like a useful
file, what does that tell us?


~~~
$ cat tophat_out/align_summary.txt 
~~~
{: .bash}

~~~
Reads:
          Input     :     74269
           Mapped   :        81 ( 0.1% of input)
 0.1% overall read mapping rate.
~~~
{: .output}

0.1 pecent of our reads mapped! This is not good...

When I saw this result while preparing the lab I was very suprised, I would
expect most of the reads in these files to align to our reference genome. Then I 
realised ther wassomething special about our experiment: we have data from multiple
 species. The default values in tophat2 expect you to be working on a single species with 
 a good reference genome. So, all of the sequences should be very similar. If we
 want `tophat` to  work with our data we'll have to allow a few more mismatches


~~~
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 ref/Ecan_ref_genes fq/Ecan_01.fq 
~~~
{: .bash}

~~~
Reads:
          Input     :     74269
           Mapped   :     40386 (54.4% of input)
54.4% overall read mapping rate
~~~
{: .output}

## Customize the output

That's a bit more like it! But note that re-runnng `tophat` has over-written the
output directory. If we want to run tophat for all 14 sequence files we'll need
to save teh important data from each one. 

> ## Exercise
>
> read the tophat2 help message and find out how to change the name of the
> output directory



We can use the `-o` argument to specify a particular 
~~~
$ mkdir logs
$ mkdir logs/tophat
$ mkdir tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_01/ \ 
         ref/Ecan_ref_genes fq/Ecan_01.fq 
~~~
{: .bash}

~~~
$ls logs/tophat
~~~
{: .bash}

~~~
Ecan_01
~~~
{: .output}

> ## Exercise
>
>  1. Re-run tophat2 using different values for the intron-length options (-i and -I).
> What difference does this make to the alignment-rate? What other 
>  2. Can you use the skills you've learned so far today to extract only the
>     "mapping rate" data from `align_summary.txt`?



