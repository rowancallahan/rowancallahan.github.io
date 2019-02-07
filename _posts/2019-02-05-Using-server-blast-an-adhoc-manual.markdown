---
layout: post
title:  "Actually getting started with Blast and Blastn"
date:   2019-02-05 11:50:46 -0400
categories: jekyll update
---

Blast is a great tool and one that people in bioinformatics use all the time. In many ways it is akin to the biological version of grep. However with that ubiquity comes some assumptions that can make it hard to get started.

Most people already know how to use the command line tool if they are using it, and often times it an be very hard to find easy quickstart documentation that both isn't too simple, and doesn't throw you in the deep end.

After using the server version of blast a while ago I came across a time recently where I needed to use it again, and had promptly forgotten all the different options that there were. For my purposes the use of blastn is the best option but I could not for the life of me find any good complete-ish quickstart guide that would allow me to actually use blast without interpreting the bizarre -help option.

Ironically getting started and using blast is quite easy if you are looking to do some simple searching scores, and it only relies upon a few different options to get started.

A basic blast search from a blast database to the sequences that you care about might look like this.

```
blastn -db /path/to/blastdb -query /path/to/fasta -out /path/to/outfile -outfmt 6 -perc_identity 80

```

Pretty basic but lest break it down why you would use these options and what other options are available

The database that I am using is a local database, non local databases can be used and some are automatically defined with blast.

However if you have a fasta file of nucleotides it is pretty easy to make a blast db with the makeblastdb command.

the query is the file that you are searching through. Blastn will search through all of the query sequences to see if any of the blastdb sequences are close to the query.

The outfile is pretty self explanatory it is just a path to where you want the path to be

outfmt is one that can be changed and it is possible that a string format is used to provide more data than just one of the premade formats that exists. The outfmt 6 specifies a tab delimited output in this order


perc\_identity is asks you how much of the db should match onto your query, if all of one of the db sequences matches onto your query then that is pretty good. Filtering by e value is also something that is a good idea just to toss out what is ~obviously~ junk or not a good match.

In order to make a quickstart a little easier and a little less painless I have also created a man page for blast that can be accessed [here][manfile]. Here is also a printout of it if grabbing from github is a little more work than its worth.

This is the output of running
```
git clone https://github.com/rowancallahan/unofficial_blastn_man
cd unofficial_blastn_man
man ./blast_unoficcial_man.1
```


```
BLASTN(1)                              General Commands Manual                              BLASTN(1)

~~How do you actually use BLASTN?~~ A practical quick start manual for BLASTN

BLASTN - Basic Linear Alignment Search Tool
       blastn [-db blast-database ] [-query file ] [-out out-file ] ...

DESCRIPTION
       BLASTN A common and popular tool used to search for nucleotide sequences that match a sequence
       in a database.  This tool uses has been in use for a long time and is quite fast for searching
       many  sequences  for  matches against multiple other sequences.  BLASTP is the version of this
       tool for proteins.  Much of this manual is taken from either the NCBI website or other website
       such as biostars or school trainings on how to use BLAST.

OPTIONS
       -evalue
              Expectation value (E) threshold for saving hits default is 10

       -outfmt
              puts  out  the file in the specified out format if you 6 is for tab separated, 7 is for
              tab separated with comments for the names of each of the columns. See blastn (2)

       -perc_identity
              What percent identity does the sequence from the database have to match onto the  query
              sequence.

OUTPUT TYPES
       The following section is a cut and paste from the BLASTN -help option

       outfmt <String>
          alignment view options:
            0 = Pairwise,
            1 = Query-anchored showing identities,
            2 = Query-anchored no identities,
            3 = Flat query-anchored showing identities,
            4 = Flat query-anchored no identities,
            5 = BLAST XML,
            6 = Tabular,
            7 = Tabular with comment lines,
            8 = Seqalign (Text ASN.1),
            9 = Seqalign (Binary ASN.1),
           10 = Comma-separated values,
           11 = BLAST archive (ASN.1),
           12 = Seqalign (JSON),
           13 = Multiple-file BLAST JSON,
           14 = Multiple-file BLAST XML2,
           15 = Single-file BLAST JSON,
           16 = Single-file BLAST XML2,
           17 = Sequence Alignment/Map (SAM),
           18 = Organism Report

          Options 6, 7, 10 and 17 can be additionally configured to produce
          a custom format specified by space delimited format specifiers.
          The supported format specifiers for options 6, 7 and 10 are:
                   qseqid means Query Seq-id
                      qgi means Query GI
                     qacc means Query accesion
                  qaccver means Query accesion.version
                     qlen means Query sequence length
                   sseqid means Subject Seq-id
                sallseqid means All subject Seq-id(s), separated by a ';'
                      sgi means Subject GI
                   sallgi means All subject GIs
                     sacc means Subject accession
                  saccver means Subject accession.version
                  sallacc means All subject accessions
                     slen means Subject sequence length
                   qstart means Start of alignment in query
                     qend means End of alignment in query
                   sstart means Start of alignment in subject
                     send means End of alignment in subject
                     qseq means Aligned part of query sequence
                     sseq means Aligned part of subject sequence
                   evalue means Expect value
                 bitscore means Bit score
                    score means Raw score
                   length means Alignment length
                   pident means Percentage of identical matches
                   nident means Number of identical matches
                 mismatch means Number of mismatches
                 positive means Number of positive-scoring matches
                  gapopen means Number of gap openings
                     gaps means Total number of gaps
                     ppos means Percentage of positive-scoring matches
                   frames means Query and subject frames separated by a '/'
                   qframe means Query frame
                   sframe means Subject frame
                     btop means Blast traceback operations (BTOP)
                   staxid means Subject Taxonomy ID
                 ssciname means Subject Scientific Name
                 scomname means Subject Common Name
               sblastname means Subject Blast Name
                sskingdom means Subject Super Kingdom
                  staxids means unique Subject Taxonomy ID(s), separated by a ';'
                                (in numerical order)
                sscinames means unique Subject Scientific Name(s), separated by a ';'
                scomnames means unique Subject Common Name(s), separated by a ';'
               sblastnames means unique Subject Blast Name(s), separated by a ';'
                                (in alphabetical order)
               sskingdoms means unique Subject Super Kingdom(s), separated by a ';'
                                (in alphabetical order)
                   stitle means Subject Title
               salltitles means All Subject Title(s), separated by a '<>'
                  sstrand means Subject Strand
                    qcovs means Query Coverage Per Subject
                  qcovhsp means Query Coverage Per HSP
                   qcovus means Query Coverage Per Unique Subject (blastn only)
          When not provided, the default value is:
          'qaccver saccver pident length mismatch gapopen qstart qend sstart send
          evalue bitscore', which is equivalent to the keyword 'std'

BUGS
       The  documentation  can  be  difficult and hard to read. So far I have not had any issues with
       blast however, more info available at https://www.ncbi.nlm.nih.gov/books/NBK279670/

AUTHOR
       Rowan Callahan and sundry internet blogs if you have actual questions please email someone who
       isn't me <blast-help at ncbi.nlm.nih.gov>

SEE ALSO
       blastn(2), blastp(1)

User Manuals                                    Rowan                                       BLASTN(1)

```

[manfile]: https://github.com/rowancallahan/unofficial_blastn_man

