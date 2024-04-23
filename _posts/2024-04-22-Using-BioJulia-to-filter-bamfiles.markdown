---
layout: post
title:  "Getting Started With BioJulia Part 2: Copying samtools and samtools like things"
date:   2024-04-22 11:04:46 -0400
categories: jekyll update
---

## Welcome back

Hopefully the last blog post made some sense. In this blog post I will be talking about how to use Julia to essentially filter bamfiles and perform some of the 
useful tasks that samtools might do for you with just julia. This will be essentially scripting filtering a bamfile.
Despite initially being much more complicated my hope is that this will be of some use to those of you that end up piping bamfiles through multiple samtools commands and end
up having Snakemake files with massive rules for filtering out a certain kind of read.

Last time we worked out how to go through and analyze reads in a pluto notebook a special kind of computational notebook that is used a lot with julia scripts. All of this code could
also be run in a jupyter notebook if the author so desired. One caveat of this post is that it is likely to exist in a state of being half finished for some time.
Hopefully at the minimum this code will provide a jumping off point for scripts to allow someone to work with reads in julia and perform some level of processing on them that might save 
them some time.


##

Starting off last time we were able to access a single read and see what we were looking at with the following code.

```{julia}

using XAM, BioSequences,
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
```

We could look at the sequence as well as a few other characteristics of the read and print those out to the console. But lets say instead that we want to filter
out the file and write this to a new bamfile instead. If this is the case then there are a few steps that we need to do in order to get this to work in the BioJulia ecosystem.


We need to create a new header file for the new bamfile and then we need to open up a new bgzf stream like below.

```{julia}
#this libary is needed to write to a bamfile in julia
using BGZFStreams
function filter_bam(file_in, file_out)
    

    reader = open(BAM.Reader, file_in)
    record = BAM.Record()

    #get the header from the bam file.
    bam_file_header = BAM.header(reader)
    writer = BAM.Writer(BGZFStream(open(file_out, "w"), "w"), bam_file_header)
    
   #keep going through until the reader hits the end of the file.
    while !eof(reader)
        #make sure that the record is empty beforew overwriting it
        empty!(record)

        #check the new read and get it from the reader
        read!(reader, record)

        #check whether the read is mapped
        condition  = XAM.ismapped(record)

        #if the read is mapped then we want to writ it to the stream.
        if condition
            write(writer, record) 
        end

    end

    #close both the reader and the writer so that no issues exist
    close(reader)
    close(writer)
end
```


Notice how we could change the condition to whatever we want. We can also add combinations conditions as well.
For example we could check whether the read is properly paired as well as whether it is mapped and only look at reads that have this characteristic.
But the advantage is that any function that takes in a read can be used as your conditional. We can remake samtools filtering with arbitrary many rules!

Notably this would be just as doable using either pysam or the HTSeq library in C or through its bindings in a bunch of different languages.
The main advantages here are just all of the other advantages that you get with using Julia. I think that these are enough that it makes it worth it to use Julia because of its combination of speed and ease of writing.


Below is one toy example of filtering out one very bizzare read type. 

```{julia}
#this libary is needed to write to a bamfile in julia
using BGZFStreams
function filter_bam(file_in, file_out)
    

    reader = open(BAM.Reader, file_in)
    record = BAM.Record()

    #get the header from the bam file.
    bam_file_header = BAM.header(reader)
    writer = BAM.Writer(BGZFStream(open(file_out, "w"), "w"), bam_file_header)
    
   #keep going through until the reader hits the end of the file.
    while !eof(reader)
        #make sure that the record is empty beforew overwriting it
        empty!(record)

        #check the new read and get it from the reader
        read!(reader, record)

        #check whether the read is mapped
        condition  = (BAM.sequence(record) == "AAAA")

        #if the read is mapped then we want to writ it to the stream.
        if condition
            write(writer, record) 
        end

    end

    #close both the reader and the writer so that no issues exist
    close(reader)
    close(writer)
end

```

In the above example we look for reads that match the exact sequence that we supply ` "AAAA" `.
While a bizarre choice if you really did have reads like this it would be a nightmare to try and come up with all of the conditionals using general SAMtools.
The other advantage is that computations on these functions will also be quite fast yet easy to read as opposed to if you were using plain python or C.

Happy hacking!
