---
title: "Assessing Read Quality"
teaching: 30
exercises: 20
questions:
- "How can I describe the quality of my data?"
objectives:
- "Use `for` loops to automate operations on multiple files."
keypoints:
- "`for` loops let you perform the same set of operations on multiple files with a single command."
---

# Bioinformatics workflows

Typically, bioinformatics analyses require us to run a series of programs on
each of several input files. Results from one analysis will feed into the next,
creating a "pipeline" of commands producing results, papers, degrees and fame!

The sad truth, is that you will often end up repeating steps from any given
pipeline as new data and new programs become available or your collaborators and
peer reviewers ask you to alter things. Thankfully, the skills you learn today
will help you to automate your analyses and make these re-runs of the pipeline
much less painful. 

## Your first pipeline: quality control.

The first step of almost any bioinformatics pipeline is checking out the quality
of the data. For an RNAseq project, we typically want to know how many sequences
are in each library, how good the "base quality" scores are for each read and 
whether we need to trim of low-quality bases from the ends of reads. 

There are several programs for exactly these steps, including fastqc and
solexaQA (developed by Murray and Patrick!). Today, we are going to use a much
simpler approach: simply counting the number of "N"s in each file.

The shell-data directory contains a script called `count_n_fastq.sh`.
Run the script on one `.fq` file to check out it's output

~~~
$ scripts/count_n_fastq.sh fq/Ecan_01.fq 
~~~
{: .bash}

~~~
2287
~~~
{: .output}


> ## Exercise
> `scripts/count_n_fastq.sh` is a shell script, just like the one we created in
>  the last lesson. Use `less` or `cat` to read the script. The script uses some
>  shell commands you haven't seen before. Can you reverse
>  engineer how the script is working? Hint: `$1` is taking the place of any .fq
>  file in this case.


We counted the number of "N"s in one file, which is nice but it would take a
long time to run this script on all 14 files...

~~~

$ scripts/count_n_fastq.sh fq/Ecan_01.fq 
$ scripts/count_n_fastq.sh fq/Ecan_02.fq 
$ scripts/count_n_fastq.sh fq/Ecan_03.fq 
$ scripts/count_n_fastq.sh fq/Ecan_04.fq 
...
~~~
{: .bash}

Let's use a for loop!

~~~
$for filename in fq/*.fq; 
do 
    scripts/count_n_fastq.sh $filename
done
~~~
{: .bash}

~~~
2287
286
232
193
99
267
123
91
64
578
1858
1763
1191
61
~~~
{: .output}


## Recording the results

Anytime you want to done simple thing to each of several inputs (e.g. files or
paramater values), a for loop is going to be the way to go. 

So far we have only printed the output to the screen. We should also record the
information in an output file. Create a new directory for `qc_data` and redirect
the outputs from the for loop into a file call `Ns_per_file.tsv`:

~~~
$ mkdir qc_data
$ for filename in fq/*.fq; 
do 
    scripts/count_n_fastq.sh $filename >> qc_data/Ns_per_file.tsv
done
~~~
{: .bash}


Finally, to make sure we are able to link the data in the .fq files to data our
biological samples, we should also record the name of each file in out output.
This is where the `basename` function comes in handy. Also, note that we assign
the output of the script to a variable inside the for loop.


~~~
$ rm qc_data/Ns_per_file.tsv
$ for filename in fq/*.fq; 
do 
    number_of_Ns=$(scripts/count_n_fastq.sh $filename)
    echo $(basename $filename) $number_of_Ns >> qc_data/Ns_per_file.tsv
done
~~~
{: .bash}

Use `less` or `cat` to check the output of the file. Is it what you expected?

## Document our progress. 

Nice. We have down (a mini example of) the quality control checks for all out
RNAseq files. We better make a record of the code taht we used to produce these
files so we can write it up for our chapters and manuscripts and come back and
fix it if we messed something up. 

Use `nano` to open up the file README and write some notes to yourself,
including 
 - A heading say "QC steps"
 - A brief description of what files were used and created
 - The actual code used to generate `qc_data/Ns_per_file.tsv


