---
layout: post
title:  "Getting Started With BioJulia Part 1: Analyzing Bam Files?"
date:   2024-04-10 17:10:46 -0400
categories: jekyll update
---

## Who is this for?

The following may be a weird mish mash of different levels of complexity, hopefully a biologist who is just getting started in programming can use this to write some of their own genomics
analysis scripts. Or someone from computer science getting started working with sequences can get a little bit of use of what parts of sequencing reads are of use and interesting to biologists.
However, this post may require a teensy bit of googling or asking mr GPT if you are less familiar with next generation sequencing or have no experience with programming (Sorry!).


## Why should I care about Julia?

Using Julia for bioinformatics has a few advantages which is the main reason I am choosing to use it for a project in which I am analyzing BAM files. 
Julia is also getting more popular among datascientists in general and has some cool features. Some of these features make it an ideal alternative to Python or R when we are trying
to make command line tools to analyze BAM files.
I like BAM files its a great format, and its pretty ubiquitous in bioinformatics. As I have gotten more into working with sequencing data I have found that I often want to work
directly with BAM files and access all of the information that they provide before doing further analysis. One example of this is filtering out reads based off of really complex criteria.
If I have a complicated decision process over whether to use or throw out a read this might change all the time but filtering this out with samtools might be too hard.
In fact a number of bioinformatic tools are essentially scripts with wrappers over PySam or HTSeq for filtering out reads based off of overlaps with GTF locations and mapping quality.

Originally I thought it might be a good idea to learn C and use the awesome HTSeq written by Heng Li and coauthors. 
But learning C was pretty hard and I thought it might end up being a waste of time for the uses that I had. 
While Julia isn't a C replacement it can offer some of the speed when working with very large sequencing data and has helped me to iterate over experiments quickly 
where PySam may have made this more difficult to do without having to learn cython, or where using R might have been impossible without writing RCpp code.

The main downside compared to C, GO and Rust is that Julia doesn't quite have a static compiler available yet, meaning that in some ways it will still be pretty similar
to python tools in terms of ease of installation. So if you do want to create a tool you will need to supply it as a script.
There are a lot of tools that might be nice to have in the bioinformatics ecosystem for working with BAM files. DeepTools, Samtools, Bedtools, Picard, and GATK
( as well as many others that I am blanking on here, whoops, thanks for your work in the ecosystem!) are some very popular tools that perform read filtering and are essentially toolkits for working with bamfiles.
Notably people often do not write scripts to process bamfiles themselves.


Often instead of creating custom scripts I seem to approximate this using piping and using many of the above tools.
However I want to be able to start working with reads directly and also process them quickly. 
When I saw that BioJulia existed and had a number of packages I thought that this could potentially be a much easier avenue than learning C and or trying to work with PySam, pyRanges or GenomicRanges and dealing with speed issues when I have to deal with the parts of the program that won't end up just being a wrapper around a C library.
While GenomicRanges, PyRanges, and PySam are all extremely fast as they are wrappers of C once you get the data out you are always going to have some level of bottleneck working with Python and R. You can always try using packages like DataTable and Numpy but its easier if you can just program in a "dumb" style without having to use someone elses libraries sometimes.

Again this was an assumption that I made which may not be quite true, I also wanted an excuse to learn Julia and complete the Python, R, Julia trifecta.
This blog post is my attempt at writing some of the missing "introductory walkthroughs" for BioJulia usage.
I intend to update this and my other posts and add more information for working with Julia. For today the main questions we will be asking are;
how would I get genecounts from a bamfile using BioJulia? A popular tool to do this is htseq-count written in python because it allows to filter by strict coverage. 
Another faster tool is called FeatureCounts which is written in C.

We will be working with just Julia in order to read in the bam file and count overlaps with genes. As you will see we also are able to filter out Bamfiles like we might do before hand with samtools
Finally we can do some extra "tricks" fairly easily that might be something we would do with Picard.
Depending on time I may add a final post showing how to make graphics similar to DeepTools and RseqQC using BioJulia as well.


## Writing your first few lines

During this walkthrough I will be using Pluto.jl a julia package for making interactive notebooks to create and work with sequences.
The main packages that I am using in this analysis is `XAM.jl` for processing BamFiles and Samfile records.
I am also using BioSequences in order to potentially do some processing of the sequences. Currently the head of your Pluto notebook should have a cell that looks like this:

```
using XAM, BioSequences, GenomicFeatures
```


Next we are going to try and open a bamfile and look at the first read. For our purposes lets say we have a bam file called ```~/path/to/dir/example.bam``` .
We are going to open it and take a look at the first few reads, and what information they contain. Here is an example of what we are going to run next. 


```julia
begin
filename = "/path/to/dir/example.bam"
#create a reader object using the file we have supplied
reader = open(BAM.Reader, filename)
	
record = BAM.Record()
#make sure that the record is empty
#( this would be useful when  looping over multiple reads)
empty!(record)

#get the read from the reader and push it to the empty record object.
read!(reader, record)

#close the reader after we have read one sequence
close(reader)

#Use the BAM.sequence method to get the sequence from the BAM record object
bam_sequence = BAM.sequence(record)
end
```

Note that I have wrapped the entire command in a ```begin your code here end``` block because pluto makes sure that code can be placed in any order and automatically updates if you want to paste a code block it needs to be wrapped with this.
However, this code would work the exact same if you separated each line of code into a new line when pasting it into pluto.

We can now see that there has been a sequence printed out to the main plot. This is great we can access the direct sequences from our bam file! What if we want to see where it mapped to?
Lets check the documentation for ```XAM.jl``` 

[link to documentation of xam](https://biojulia.dev/XAM.jl/dev/man/hts-files/#SAM-and-BAM-Records)

We can use ```XAM.BAM.refname``` to get the chromosome or contig (big genomic chunk you get which you might not be sure what it is or if its complete when assembling the genome) name.
If we scroll down we can find even more ways to access the information that is stored for each read in the Bam file. ```XAM.BAM.position``` will give us the leftmost mapping position of the read.
Great now we can maybe see if it overlaps with a gene, lets go get the rightmost position as well! ```XAM.BAM.rightposition```

Now lets take this information and turn our read into a "genomic range" that we can check for overlaps with other ranges.

```julia
begin
end_position = BAM.rightposition(record)
start_position = BAM.position(record)
chromosome_name = BAM.refname(record)
end

```

Excellent we can now follow the documentation for GenomicIntervals and use the above variables to create a new range: [link to genomic intervals documentation](https://biojulia.dev/GenomicFeatures.jl/latest/intervals/#overlap-query)


```julia
begin
#putting the strand as "." means that we don't include any strand information here
#if you want you can look for this and add it from the bam reader function in XAM.jl
our_interval = Interval(chromosome_name, start_position, end_position, '.', bam_sequence)
example_gene_interval = Interval(chromosome_name, start_position+50, end_position+50, '.', "EXAMPLE_GENE_ABC")
end
```

Finally if we call an overlap query we can iterate over all the overlaps. Because the example gene has the same location of the other interval just offset by only 50 basepairs 
we should be able to find an overlap. Our read is probably around 100bp to 150bp long if it came from a classic next generation sequencing technology.
Before we do this we are first going to make an interval collection. Often times you will want to overlap multiple read against multiple locations, for example when you want to calculate gene counts. In order to do this you first need to add both your reads as well as your gene locations to separate interval collections so that genomicFeatures can quickly
Search them both for overlaps.

Below is the code that
```julia
begin
#we are going to keep the string metadata that we have added for each gene
gene_collection = IntervalCollection{String}()
push!(gene_collection, example_gene_interval)

#we are going to keep the sequence of the string just for fun
#this might be useful if we wanted to see if all of the reads that map to our gene have some odd feature that
#we need to worry about.
read_collection = IntervalCollection{String}()
push!(read_colelction, our_interval)

for (x, y) in eachoverlap(gene_collection, read_collection)
    println("intersection found between ", x, " and ", y)
end

end
```

Our output should look something like this, ignoring the exact details.


```
intersection found between GenomicFeatures.Interval{String}:
  sequence name: chr1
  leftmost position: 10052
  rightmost position: 10160
  strand: .
  metadata: EXAMPLE_GENE_ABC and GenomicFeatures.Interval{String}:
  sequence name: chr1
  leftmost position: 10002
  rightmost position: 10110
  strand: .
  metadata: AACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCTAACCCAAAACAAAA

```


And thats our introduction to working with bam files in Julia. Hopefully this should be helpful to anyone looking to filter reads before performing gene counts or work more closely with bam files in julia.
There are numerous data science tools that can be used to group_by and perform other data oriented verbs. packages such as DataFrames and Pipe are great for copying a lot of what the tidyverse offers in terms of features. There is also a separate package called Tidier which aims to directly copy much of what the tidyverse has but in julia.

Between DataFrames, Pipe and Tidier the rest of the analysis should be pretty straightforward for analysing a large number of reads and or creating a CSV file that you can then analyze using R or Python.




