Apologies for not updating this since the course - please contact me if you would like the information that should be here!

# PSFP Bioinformatics:
#### computational need-to-knows and scRNA seq analysis

## Data + analysis description

[Data and analysis overview]

## System Setup:

[setup instructions]

More software stuff coming soon! Reference materials below:

## Course Intro

Today's workshop is to get you up to speed with a full start-to-finish workflow for scRNA-seq data analysis. Don't worry about retaining all of this information in great detail - my aim is to simply make you aware of the various steps involved in an experiment like this so that you can refer back to this document at a later date and have a rough idea of what's going on/where you should start.

I highly encourage you to take notes directly on this document so that you have an annotated resource sheet when you need it!

## Getting started

### Experimental planning

All RNA sequencing experiments (bulk or otherwise) should have careful consideration put towards the following:

* Number of replicates
* Timepoints collected
* Genotypes and controls
* Dissection specificity
* Batch-effects
* Sample loss due to technical errors

These variables can greatly affect downstream processing and conclusions, and mistakes/corner cutting here can result in trouble during analysis. Large enough missteps here often require re-harvesting samples and re-running a library!

Be aware that in addition to these considerations, single-cell RNA sequencing (scRNA-seq) has its own caveats:

* Drop-out events
* Lower read depth compared to bulk-sequencing
* Doublets/Multiplets
* Did I mention **batch-effect**??

My advice when getting started with a sequencing experiment is to treat the wet-lab and dry-lab as *equal halves* of your experimental process. Data analysis is a critical component of your project and you should be prepared to spend considerable time on it.

Some things I wish I knew when I ran my first sequencing experiment:

* Pay careful attention to your controls and replicates
* Research and understand the sequencing throughput of the technology you'll use, plan accordingly
* Data analysis can reveal flaws in your experimental design no matter how much thought goes into it:
    * Run a pilot experiment or two
    * Lean on your colleagues' experience
    * Get really good at Googling stuff

In short: don't overthink things, and definitely don't try to knock your experiment out of the park on the first pass!

## Read mapping to analysis

There are two ways we can include sequencing experiments in our projects:

1. Run your own experiment
2. Grab someone else's data and run with it

Today we will work with data from [this paper](https://www.ebi.ac.uk/ena/browser/view/PRJNA489304):

![](https://ars.els-cdn.com/content/image/1-s2.0-S2211124719308721-fx1_lrg.jpg)
*Figure 1:* "Single-Cell RNA-Seq of the Developing Cardiac Outflow Tract Reveals Convergent Development of the Vascular Smooth Muscle Cells"

This data was chosen for a project in my lab that is focused on cellular mechanisms underlying the formation of the cardiac outflow-tract. Today we will use one timepoint from this paper as our example.

### Step 1: Data preprocessing (UNIX)
#### 1a. bcl to fastq
This step is really down in the depths of preprocessing and is more often required when running your own experiment. The sequencer will output `.bcl` (basecall) files, and these need to be converted to a `.fastq/.fasta/.fa` file for downstream processes.

If you use a sequencing core facility, it is possible they can/will run this for you. You can read more about demultiplexing and `bcl` to `fastq` conversion [here](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/bcl2fastq-direct) on the 10x Genomics website.

If you are downloading data from a publication/receiving data from another group, it is likely that this has been taken care of. Publications vary, but many will publish their sequencing data to NCBI's [Sequence Read Archive](https://www.ncbi.nlm.nih.gov/sra).

Our example data was downloaded using a [UNIX-based tool](https://ncbi.github.io/sra-tools/) called `SRA-toolkit`. You may be able to access SRA data via the [European Nucleotide Archive](https://www.ebi.ac.uk/ena/browser/home) (ENA) which allows for direct downloads of `fastq` files directly from your web browser.

Links to the raw example dataset:

* SRA: https://www.ncbi.nlm.nih.gov/sra?LinkName=bioproject_sra_all&from_uid=489304
* ENA: https://www.ebi.ac.uk/ena/browser/view/PRJNA489304

#### 1b. align fastq to reference genome (UNIX)
This is the bread-and-butter of sequencing preprocessing workflows. Bulk and scRNA sequencing both follow the same general pipeline:

![](https://training.galaxyproject.org/training-material/topics/transcriptomics/images/scrna_workflow.svg)
*Figure 2:* General alignment workflow (Galaxyproject)

If you are using the 10x Genomics platform, they provide [a very powerful software suite](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/what-is-cell-ranger) called `Cellranger`. This is what we'll cover today, and I can highly recommend the 10x Genomics "ecosystem" for single-cell experiments.

There are two main steps to processing data with Cellranger:

* `cellranger count`
* `cellranger aggr`

Typically running those commands will get you what you need!

If you use another platform, such as Fluidigm, you can look into `kallisto` [alignment workflows](https://pachterlab.github.io/kallisto/manual) for this purpose.

Don't know any UNIX but want to get started *now?* Check out [www.usegalaxy.org](www.usegalaxy.org) for a web-application implementation of various preprocessing and analysis toolkits. Here is their [scRNA tutorial](https://training.galaxyproject.org/training-material/topics/transcriptomics/tutorials/scrna-preprocessing/tutorial.html).

Regardless of chosen method, the end result of this process will be a "counts matrix," and this will be the basis for nearly all downstream analyses.

Please note that though the previous steps may be able to be run on your personal computer, it is best to have a workstation or server available for this. More on that later.

### Step 2: Data analysis (Python/R)

This is the step everyone wants to jump to, and for good reason (it's where the biology comes back into focus). There are a few tools out there for this:

* R
    * [Seurat](https://satijalab.org/seurat/)
    * [Monocle](http://cole-trapnell-lab.github.io/monocle-release/)
* Python
    * [Scanpy](https://scanpy.readthedocs.io/en/stable/)

We will cover Seurat later today, but the overall steps in analysis are similar between packages.

## Where to start?

This can all seem overwhelming, but don't worry. There's a lot of support on-campus whether you find yourself at UCSF or Stanford.

So, so much goes into this work. Let's break things downs:

* **Camp A:** I just want to look at the biology!
    * Fair enough - here are the skills you will need:
        * Some *basic* UNIX
        * Proficiency in **R** or Python (I recommend R)
        * Familiarity with the preprocessing steps needed to start analysis
            * *Bonus:* ability to preprocess data using [Galaxy](www.usegalaxy.org)
            * Galaxy [tutorials](https://galaxyproject.org/learn/)!

If you choose to remain in **Camp A**, that's fine! Do your best to build an understanding of the complexity of computational work - it will help you better communicate with domain experts and core facilities, and that will really help you keep projects and analyses moving along.

* **Camp B:** I want to be self-sufficient!
    * Bioinformaticians like everyone, but we *really* like you
        * Intermediate/Advanced UNIX
        * Proficiency (but ideally strong footing) in R and/or Python
        * Intermediate to Advanced understanding of the tools and steps involved in data analysis (great for troubleshooting issues/asking for help)

Building the skillsets to be in **Camp B** takes time and support. For instance, one all-star postdoc I work with took ~2 years to get to this point. Now that they are there, though, our collaboration is quite close to that I'd have with a bioinformatics colleague (and it's been a huge boost to the project overall).

### Core facilities:

The first, fastest option to get your data ready for analysis? Lean on your institution's Bioinformatics core facilities for the preprocessing:

* UCSF's Core Search: [Bioinformatics](https://cores.ucsf.edu/bioinformatics-analysis.html)
* Stanford's [Bioinformatics-as-a-Service](https://med.stanford.edu/gbsc/baas.html)

### Skill-building for DIY (UCSF)

There are a number of workshops available at UCSF:

* UCSF [Data Science Initiative](https://www.library.ucsf.edu/ask-an-expert/data-science/)
    * [Available workshops](https://www.library.ucsf.edu/ask-an-expert/classes-catalog/) (check this regularly!)
    *  [Upcoming workshops](https://calendars.library.ucsf.edu/calendar/events/?cid=928&t=g&d=0000-00-00&cal=928&ct=27094&inc=0)
    * Recommended workshops:
        * [Intro to UNIX](https://courses.ucsf.edu/course/view.php?id=5327)
        * [Intro to R](http://tiny.ucsf.edu/dsirintro)
        * [Intro to Python](https://courses.ucsf.edu/course/view.php?id=5281)
        * [RNA-seq analysis with R Bioconductor](http://tiny.ucsf.edu/dsirnaseq)
        * [scRNA-deq analysis with R Bioconductor](http://tiny.ucsf.edu/dsiscrna) (intermediate-level, materials previewed today)

* UCSF [Bakar Computational Health Sciences Institute + Gladstone Institutes](https://github.com/gladstone-institutes/Bioinformatics-Workshops/wiki)
    * Recommended workshops:
        * [Intro to stats and experimental design](https://github.com/gladstone-institutes/Bioinformatics-Workshops/wiki/Introduction-to-Statistics-and-Experimental-Design)
        * [Intro to UNIX](https://github.com/gladstone-institutes/Bioinformatics-Workshops/wiki/Unix-Command-Line)
        * [Intro to R](https://github.com/gladstone-institutes/Bioinformatics-Workshops/wiki/Introduction-to-R-for-Data-Analysis)
        * [Intro to RNA-seq](https://github.com/gladstone-institutes/Bioinformatics-Workshops/wiki/Introduction-to-RNA-Seq-Analysis) (bulk-seq)
        * Advanced (R prereq): [Single-cell RNA-seq](https://github.com/gladstone-institutes/Bioinformatics-Workshops/wiki/Single-Cell-RNA-Seq-Analysis)
        * Advanced (UNIX prereq): [Using Wynton HPC](https://github.com/gladstone-institutes/Bioinformatics-Workshops/wiki/Working-on-Wynton-HPC)

Some of these are redundant, but it is prudent to check availability for both versions of a class as these fill up very quickly.

For my Stanford friends - I'm sorry, I've only been involved with UCSF training programs! Some great self-driven learning can be done here:

* [Software Carpentry](https://software-carpentry.org/lessons/index.html)
    * [The UNIX shell](http://swcarpentry.github.io/shell-novice)
    * [Programming with R](http://swcarpentry.github.io/r-novice-inflammation)
    * [Programming with Python](http://swcarpentry.github.io/python-novice-inflammation)
    * [R for Reproducible Scientific Analysis](http://swcarpentry.github.io/r-novice-gapminder)***
* [LinkedIn Learning](https://www.linkedin.com/learning/me)

### Compute resources:

While much of the R and Python work can likely be done on your personal computer, alignment is generally best left to heavier-duty systems like a High-Performance-Compute (HPC) cluster (fancy term of a big, powerful system - think of it as a server). A moderately powerful desktop computer/workstation can also handle these workflows pretty well.

Either way, the system you use for sequence alignment will need to be running Linux, and setup of these types of systems is often best left to your friendly IT department. Luckily, both UCSF and Stanford have HPC systems available for researchers:

- UCSF's [Wynton HPC](https://wynton.ucsf.edu/hpc/)
- Stanford's [Sherlock HPC](https://www.sherlock.stanford.edu/docs/overview/introduction/)

These types of systems have a learning curve, so having at least an intermediate-level grasp of UNIX is highly recommended before beginning to work with these.

## scRNA-seq analysis example

Here we will cover the basics of scRNA-seq analysis in Seurat using the cardiac outflow-tact dataset we learned of earlier. The workflow used will very closely resemble the Data Science Initiative's scRNA-seq workshop, which is based closely on a [Seurat example workflow](https://satijalab.org/seurat/articles/pbmc3k_tutorial.html) provided by the developers on their website.

In general, the steps involved in analysis are:

1. Data import
2. Quality Control and cell selection
3. Data Normalization/Scaling
    * Old workflow: normalize + scale data
        * Easier to understand, but not often used in practice
    * New workflow: SCTransform
        * More difficult to understand, but is the gold-standard in Seurat
4. Dimensional Reduction
5. Cell Clustering
6. Identifying cell marker genes/annotating clusters
7. Analysis to query the biology you're interested in (Differential Expression, Gene-ontology analysis, etc.)

