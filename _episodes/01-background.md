---
title: "Background and Metadata"
teaching: 15
questions:
- "What data are we using?"
- "Why is this experiment important?"
objectives:
- Think about biological reality beyond .fq files
keypoints:
- Epichloe is are awesome
- Metadata associated with a project shuold contain all the key biological
  information need to make senese of the experiment and guide the analysis
---


# Background

We are going to use an RNA seq data set from  _Epichloë canadensis_, a polyploid
fungus.

 - **What is *Epichloe*?**
    - *Epicloe* are fungi that live in close association with pasture
      grasses. They form "defensive mutualisms" with their hosts, which is to
      say they exchange a place to live and plentiful supply of sugar for an
      array of chemicals that protect the grasses from pests.
    

 - **Why is *Epichloë* important?**
    - Because *Epichloë* help pasture grasses, and New Zealand grows a lot of
      cows, there is a great deal of interest n developing agricultrually
      benficial _Epichloë_ strains.
    - All of the lab, genomic and genetic resources availble to _Epichloe_ also
      make it a great model system in which to study, host-fungal interactions 
      and evolutionary biology.
    
# The Data

 - The data we are going to use is taken from 3 Epichloe species, one of which
   is a polyploid generated from the other two
 
 - The experiment was designed to assess how the gene expression changes in the
   hybrid species compared to its' parents. We grew all of the species in their
   natural grass hosts under controleld conditions, and generated whole
   transcriptome data from the infected tissue (i.e. plant and fungal reads
   together). 
 
## View the Metadata

We ahve already investigated some of the metadata. This describes both the
experimental conditions (spp, ploidy, treament, genotype) and some technical
details about the RNAseq data we generated. Note, these files contain only a
subset of the RNAseq data, we would usually produce > 10 million reads per
biological replicate.


|file       |sample_id |spp | ploidy|treatment |genotype |read_type | insert_size| nreads|
|:----------|:---------|:---|------:|:---------|:--------|:---------|-----------:|------:|
|Ecan_01.fq |Ecan_01   |Eam |      1|planta    |WT       |SE        |         500|  74269|
|Ecan_02.fq |Ecan_02   |Eam |      1|planta    |WT       |SE        |         500|  11886|
|Ecan_03.fq |Ecan_03   |Eam |      1|planta    |WT       |SE        |         500|   9359|
|Ecan_04.fq |Ecan_04   |Eam |      1|planta    |WT       |SE        |         500|   8799|
|Ecan_05.fq |Ecan_05   |Eel |      1|planta    |WT       |SE        |         500|   5932|
|Ecan_06.fq |Ecan_06   |Eel |      1|planta    |WT       |SE        |         500|  10025|
|Ecan_07.fq |Ecan_07   |Eel |      1|planta    |WT       |SE        |         500|   4983|
|Ecan_08.fq |Ecan_08   |Eel |      1|planta    |WT       |SE        |         500|   4641|
|Ecan_09.fq |Ecan_09   |H1  |      2|planta    |WT       |SE        |         500|   3681|
|Ecan_10.fq |Ecan_10   |H1  |      2|planta    |WT       |SE        |         500|  26339|
|Ecan_11.fq |Ecan_11   |H1  |      2|planta    |WT       |SE        |         500|  72633|
|Ecan_12.fq |Ecan_12   |H2  |      2|planta    |WT       |SE        |         500|  67063|
|Ecan_13.fq |Ecan_13   |H2  |      2|planta    |WT       |SE        |         500|  38359|
|Ecan_14.fq |Ecan_14   |H2  |      2|planta    |WT       |SE        |         500|   2160|

### Challenge

Based on the metadata, can you answer the following questions?

* How many biological replicates exist per species?
* Why is it important to have such replicates?

<!-- can add some additional info relevant to interplay of hypermutability and Cit+ adaptations, but keep it simple for now -->

