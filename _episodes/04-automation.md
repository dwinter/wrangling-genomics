---
title: "Automating the alignment process"
teaching: 35
exercises: 25
questions: 
- How can align data from multiple different sequence files
keypoints:
- using `echo` we can test a complex for loop command
- using for loops and `basename` can automate the naming of files to keep our
  data tidy
---

We have now aligned read from one of our files to a referecne genome. Usually,
working out how to do something right once is the hardest part of the job, and 
doing it 13 more times is relatively easy. So, let's automate our alignments.

# Aligning all 14 files

# Setting up

Once again, we can use a for-loop to iterate over all of the fastq files. Before
you run very large for loops, it is a good idea to use `echo` to print out
the commands that would be exectured. 

~~~
$for filename in fq/*.fq; 
do   
    echo tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/$filename ref/Ecan_ref_genes $filename
done

~~~
{: .bash}


~~~
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_01.fq ref/Ecan_ref_genes fq/Ecan_01.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_02.fq ref/Ecan_ref_genes fq/Ecan_02.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_03.fq ref/Ecan_ref_genes fq/Ecan_03.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_04.fq ref/Ecan_ref_genes fq/Ecan_04.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_05.fq ref/Ecan_ref_genes fq/Ecan_05.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_06.fq ref/Ecan_ref_genes fq/Ecan_06.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_07.fq ref/Ecan_ref_genes fq/Ecan_07.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_08.fq ref/Ecan_ref_genes fq/Ecan_08.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_09.fq ref/Ecan_ref_genes fq/Ecan_09.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_10.fq ref/Ecan_ref_genes fq/Ecan_10.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_11.fq ref/Ecan_ref_genes fq/Ecan_11.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_12.fq ref/Ecan_ref_genes fq/Ecan_12.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_13.fq ref/Ecan_ref_genes fq/Ecan_13.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/fq/Ecan_14.fq ref/Ecan_ref_genes fq/Ecan_14.fq
~~~
{: .output}

Hmm, we have moved through every file, just as we wanted, but not the output
directory is not quite what we want. Let's use `basename` to extract only the
relevant parts of the name


~~~
$for filename in fq/*.fq; 
do   
    sample=$(basename -s .fq $filename)  
    echo tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/${sample} ref/Ecan_ref_genes ${filename}
done

~~~
{: .bash}
~~~
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_01 ref/Ecan_ref_genes fq/Ecan_01.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_02 ref/Ecan_ref_genes fq/Ecan_02.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_03 ref/Ecan_ref_genes fq/Ecan_03.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_04 ref/Ecan_ref_genes fq/Ecan_04.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_05 ref/Ecan_ref_genes fq/Ecan_05.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_06 ref/Ecan_ref_genes fq/Ecan_06.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_07 ref/Ecan_ref_genes fq/Ecan_07.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_08 ref/Ecan_ref_genes fq/Ecan_08.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_09 ref/Ecan_ref_genes fq/Ecan_09.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_10 ref/Ecan_ref_genes fq/Ecan_10.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_11 ref/Ecan_ref_genes fq/Ecan_11.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_12 ref/Ecan_ref_genes fq/Ecan_12.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_13 ref/Ecan_ref_genes fq/Ecan_13.fq
tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/Ecan_14 ref/Ecan_ref_genes fq/Ecan_14.fq
~~~
{: .output}


That looks better. I'm now convinced that we can run this for loop and get the 
results we expected. Let's delete `echo` can get on with it.


~~~
$for filename in fq/*.fq; 
do   
    sample=$(basename -s .fq $filename)  
    tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/${sample} ref/Ecan_ref_genes ${filename}
done

~~~
{: .bash}

You should now get a lot of information streaming on to your page. Once
everthing is finished, confirm that you have a bunch of new directories in the
`logs/tophat` directory

~~~
$ ls logs/tophat/
~~~
{: .bash}

~~~
Ecan_01  Ecan_02  Ecan_03  Ecan_04  Ecan_05  Ecan_06  Ecan_07  Ecan_08  Ecan_09  Ecan_10  Ecan_11  Ecan_12  Ecan_13  Ecan_14   
~~~
{:. bash}

## Move the main output to a new directory

It's great that we have produced all these results, but using them is still a
little cumbersome. In order to get teh main output, the `accepted_hits.bam`
file, for any sample we have have to navigate through the `logs` directory. 

Let's modify the script so that it plucks the accepted_hits.bam file from the
tophat output and moves it into a new directory with one bam file per input
file:

~~~
$ mkdir bams
$for filename in fq/*.fq; 
do   
    sample=$(basename -s .fq $filename)  
    tophat2 -N 20 --read-edit-dist 20 -i 30 -I 500 -o logs/tophat/${sample} ref/Ecan_ref_genes {filename}
    mv logs/tophat/$sample/accepted_hits.bam bams/${sample}.bam
done

~~~
{: .bash}

## Record our progress!

Remeember, update the README file to describe how the .bam files were produced!

> ## Exercise 
> 
> It would be very nice to know the average mapping rate for each sample. Either
> write a new for-loop or modify this existing one to extract the last line of
> the `align_summary.txt` text for sample and rediret it to a new file called
> `qc/mapping_rate.tsv`. 
