---
start: false
title: "Bacterial GWAS"
teaching: 0
exercises: 90
questions:
- "Which genes are associated with patient mortality"
objectives:
- "Determine genes significantly associated with patient mortality and speculate why"
keypoints:
- "Contigency testing for gene presence absence to associate a genotype with a phenotype, similar to GWAS in clinical genetics is possible with bacterial genomes"
---

## Introduction

As mentioned in the first part of this exercise series, the 62 *S. pneumoniae* isolates were from patients who suffered from a invasive pneumococcal disease (IPD). IPD, especially in elderly is associated with high mortality. The outcome is partially explained by co-morbidities (e.g. diabetes) but there are some indications that it matters which strain of *S. pneumoniae* is causing the infection. 

We will therefore try to find out if there are specific virulence factors that are associated with patient survival. 

### Bacterial Genome Wide Association Studies (GWAS)

An excellent primer on bacterial GWAS is available here: [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3743258/](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3743258/) . We will make use of the tool "Scoary" [https://github.com/AdmiralenOla/Scoary](https://github.com/AdmiralenOla/Scoary), a tool complementary to Roary. Scoary has implemented several GWAS methods into one tool. Scoary needs a **comma** separated list of strain characteristics or phenotypes called traits.csv. 

Copy and paste the columns with the names and patient outcome from the tab "annotations.txt" in a file from the assembly statistics excel file, including the "Name<tab>Mortality" header. Any texteditor can be used but in this example nano is used. You can also get the file from the previous lesson. The file is here:  [https://klif.uu.nl/klif/mgen/orthology/](https://klif.uu.nl/klif/mgen/orthology/), right click on annotations.txt, "save link as". The file can also be made on your own computer and uploaded using your webbrowser. The header (first line) should start with "Name" (replace the word "Isolate") followed by the phenotype (Mortality). 

~~~
$ cd ~/orthology
$ wget https://klif.uu.nl/klif/mgen/orthology/annotations.txt
$ nano annotations.txt #change Isolate to Name
$ cat annotations.txt |tr "\t" "," > traits.csv
~~~
{: .bash}

We can now combine our gene presence absence table with the patient data and check if there are indeed specific virulence factors associated with patient mortality. We're using the special keyword "ALL" the include all annotation data. The P-value cutoff is 0.01 to limit the output. The annotation of the reference genome OXC141 can be found here: [https://klif.uu.nl/klif/mgen/annotation/OXC141/](https://klif.uu.nl/klif/mgen/annotation/OXC141/) . The sequences can be found in the .faa file and the genbank (.gbk) contains all information. Investigate the targets you found to be significant by blasting them and checking their location on the genome. 

~~~
$ cd ~/orthology
$ scoary -t traits.csv -g gene_presence_absence.csv  -p 0.01 --include_input_columns ALL
~~~
{: .bash}


> ## Discussion: Which genes are associated with patient mortality. Where are they located? what is their function?
> Download the file mortality_<date>.results.csv and open in Google Spreadsheet or Excel or any other spreadsheet tool. If you have Dutch regional settings, Excel won't recognize the file properly, you may have to adjust these (or use Google Sheets). Investigate the outcome, look at the column with OXC141 as this is a completed reference genome and the order of the genes is known for all genes. Is there something special about the order and location of the genes? It is also possible to open the OXC141 genbank file  [https://klif.uu.nl/klif/mgen/annotation/OXC141/](https://klif.uu.nl/klif/mgen/annotation/OXC141/)  Click to view and search through the text file or right click and save to download and open in IGV ( https://software.broadinstitute.org/software/igv/ ) and view the best score gene(s) there, however it should already be clear from the results.csv file. Also: Remember the figtree visualization of the tree and the coloring of the phenotypes? Generate a similar annotation file with the locus you think is involved in patient mortality and display it in figtree. Does a specific clone carry the virulence factor(s)? 
{: .discussion}

