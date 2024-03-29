# Exploration of COVID-19 Genome (under construction)

This project was completed as with a team of students in my Computer Science Engineering (CSE) 182 course on Biological Databases. This page will outline and guide readers through the challenges our team faced and demonstrate the results we arrived at. There will be a distinction in text that will indicate updates/enhanced apporaches taken after the project deadline from the original project submission.

Prerequisites:

1. BLAST: Download and install Blast on local machine. Blast can be accessed from the National Center for Biotechnology Information (NCBI) using this link: [NCBI] (https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download) . Select the appropriate package to download to your computer in order to install Blast for your system. Options include a .dmg file for macOS, .exe file for Windows, and a compressed file tar.gz for Linux systems. 

What is Blast? (From Readme file) The NCBI Basic Local Alignment Search Tool (BLAST) finds regions oflocal similarity between sequences. The program compares nucleotide or protein sequences to sequence databases and calculates the statistical significance of matches. BLAST can be used to infer functional and evolutionary relationships between sequences as well as help identify members of gene families.

How to use Blast? Blast is a command link application 

If you prefer not to use the command link application you can use the web-based Blast tool through your browser by here: [Blast](https://blast.ncbi.nlm.nih.gov/Blast.cgi)

2. REFERENCE GENOME: The reference genome of the SARS-CoV-19 virus is critical when using Blast to align against the sample genomes we will collect later. Go to the following [Resource Page](https://www.ncbi.nlm.nih.gov/sars-cov-2/). To access the genome sequence find the link to 'NCBI RefSeq SARS-CoV-2 genome sequence record' or go to the [Reference Genome](https://www.ncbi.nlm.nih.gov/nuccore/NC_045512). The link will give access to all the information related to the reference genome including metadata and database attributes which are unnecessary at this juncture. For the essential sequence information we wish for just the [FASTA format](https://www.ncbi.nlm.nih.gov/nuccore/NC_045512.2?report=fasta)

3. ANNOTATION FILE: The annotation file will outline the different protein regions of the SARS-CoV-2 genome. To access the [annotation file (.gff)](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/009/858/895/GCF_009858895.2_ASM985889v3/GCF_009858895.2_ASM985889v3_genomic.gff.gz). Due to the file type/extension of the annotation file, we will access and read the file using Python. 

1. Create FASTA formatted database of SARS-CoV-2 proteins. Once we access the .gff annotation file we want to identify the protein/genes of interest and organize the information according to our goals.

Lets first open and view the annotation file. The information in the annotation file is tab separated, so we will need to write a Python script that will parse and compile the information by recognizing the tabs. We will use Pandas to organize and view the information by importing it into a Dataframe. The columns of file are: 'Sequence Name', 'Source', 'Feature', 'Start', 'End', 'Score', 'Strand', 'Frame', 'Attribute'. 

        import pandas as pd

        dataHeader= ['Sequence Name', 'Source', 'Feature', 'Start', 'End', 'Score', 'Strand', 'Frame', 'Attribute']

        hold=[]
        with open('GCF_009858895.2_ASM985889v3_genomic.gff', 'r') as f:
            for line in f:
                line=line.split("\t")
                if(line[0][0]!="#"):
                    hold.append(line)

        annotationdf = pd.DataFrame(putintDF, columns=dataHeader)            
        annotationdf

          Sequence Name 	Source 	Feature 	Start 	End 	Score 	Strand 	Frame 	Attribute
        0 	NC_045512.2 	RefSeq 	region 	1 	29903 	. 	+ 	. 	ID=NC_045512.2:1..29903;Dbxref=taxon:2697049;c...
        1 	NC_045512.2 	RefSeq 	five_prime_UTR 	1 	265 	. 	+ 	. 	ID=id-NC_045512.2:1..265;gbkey=5'UTR\n
        2 	NC_045512.2 	RefSeq 	gene 	266 	21555 	. 	+ 	. 	ID=gene-GU280_gp01;Dbxref=GeneID:43740578;Name...
        3 	NC_045512.2 	RefSeq 	CDS 	266 	13468 	. 	+ 	0 	ID=cds-YP_009724389.1;Parent=gene-GU280_gp01;D...
        4 	NC_045512.2 	RefSeq 	CDS 	13468 	21555 	. 	+ 	0 	ID=cds-YP_009724389.1;Parent=gene-GU280_gp01;D...
        5 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	266 	805 	. 	+ 	. 	ID=id-YP_009724389.1:1..180;Note=nsp1%3B produ...
        6 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	806 	2719 	. 	+ 	. 	ID=id-YP_009724389.1:181..818;Note=produced by...
        7 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	2720 	8554 	. 	+ 	. 	ID=id-YP_009724389.1:819..2763;Note=former nsp...
        8 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	8555 	10054 	. 	+ 	. 	ID=id-YP_009724389.1:2764..3263;Note=nsp4B_TM%...
        9 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	10055 	10972 	. 	+ 	. 	ID=id-YP_009724389.1:3264..3569;Note=nsp5A_3CL...
        10 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	10973 	11842 	. 	+ 	. 	ID=id-YP_009724389.1:3570..3859;Note=nsp6_TM%3...
        11 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	11843 	12091 	. 	+ 	. 	ID=id-YP_009724389.1:3860..3942;Note=produced ...
        12 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	12092 	12685 	. 	+ 	. 	ID=id-YP_009724389.1:3943..4140;Note=produced ...
        13 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	12686 	13024 	. 	+ 	. 	ID=id-YP_009724389.1:4141..4253;Note=ssRNA-bin...
        14 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	13025 	13441 	. 	+ 	. 	ID=id-YP_009724389.1:4254..4392;Note=nsp10_Cys...
        15 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	13442 	13468 	. 	+ 	. 	ID=id-YP_009724389.1:4393..5324;Note=nsp12%3B ...
        16 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	13468 	16236 	. 	+ 	. 	ID=id-YP_009724389.1:4393..5324;Note=nsp12%3B ...
        17 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	16237 	18039 	. 	+ 	. 	ID=id-YP_009724389.1:5325..5925;Note=nsp13_ZBD...
        18 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	18040 	19620 	. 	+ 	. 	ID=id-YP_009724389.1:5926..6452;Note=nsp14A2_E...
        19 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	19621 	20658 	. 	+ 	. 	ID=id-YP_009724389.1:6453..6798;Note=nsp15-A1 ...
        20 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	20659 	21552 	. 	+ 	. 	ID=id-YP_009724389.1:6799..7096;Note=nsp16_OMT...
        21 	NC_045512.2 	RefSeq 	CDS 	266 	13483 	. 	+ 	0 	ID=cds-YP_009725295.1;Parent=gene-GU280_gp01;D...
        22 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	266 	805 	. 	+ 	. 	ID=id-YP_009725295.1:1..180;Note=nsp1%3B produ...
        23 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	806 	2719 	. 	+ 	. 	ID=id-YP_009725295.1:181..818;Note=produced by...
        24 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	2720 	8554 	. 	+ 	. 	ID=id-YP_009725295.1:819..2763;Note=former nsp...
        25 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	8555 	10054 	. 	+ 	. 	ID=id-YP_009725295.1:2764..3263;Note=nsp4B_TM%...
        26 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	10055 	10972 	. 	+ 	. 	ID=id-YP_009725295.1:3264..3569;Note=nsp5A_3CL...
        27 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	10973 	11842 	. 	+ 	. 	ID=id-YP_009725295.1:3570..3859;Note=nsp6_TM%3...
        28 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	11843 	12091 	. 	+ 	. 	ID=id-YP_009725295.1:3860..3942;Note=produced ...
        29 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	12092 	12685 	. 	+ 	. 	ID=id-YP_009725295.1:3943..4140;Note=produced ...
        30 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	12686 	13024 	. 	+ 	. 	ID=id-YP_009725295.1:4141..4253;Note=ssRNA-bin...
        31 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	13025 	13441 	. 	+ 	. 	ID=id-YP_009725295.1:4254..4392;Note=nsp10_Cys...
        32 	NC_045512.2 	RefSeq 	mature_protein_region_of_CDS 	13442 	13480 	. 	+ 	. 	ID=id-YP_009725295.1:4393..4405;Note=produced ...
        33 	NC_045512.2 	RefSeq 	stem_loop 	13476 	13503 	. 	+ 	. 	ID=id-GU280_gp01;Dbxref=GeneID:43740578;functi...
        34 	NC_045512.2 	RefSeq 	stem_loop 	13488 	13542 	. 	+ 	. 	ID=id-GU280_gp01-2;Dbxref=GeneID:43740578;func...
        35 	NC_045512.2 	RefSeq 	gene 	21563 	25384 	. 	+ 	. 	ID=gene-GU280_gp02;Dbxref=GeneID:43740568;Name...
        36 	NC_045512.2 	RefSeq 	CDS 	21563 	25384 	. 	+ 	0 	ID=cds-YP_009724390.1;Parent=gene-GU280_gp02;D...
        37 	NC_045512.2 	RefSeq 	gene 	25393 	26220 	. 	+ 	. 	ID=gene-GU280_gp03;Dbxref=GeneID:43740569;Name...
        38 	NC_045512.2 	RefSeq 	CDS 	25393 	26220 	. 	+ 	0 	ID=cds-YP_009724391.1;Parent=gene-GU280_gp03;D...
        39 	NC_045512.2 	RefSeq 	gene 	26245 	26472 	. 	+ 	. 	ID=gene-GU280_gp04;Dbxref=GeneID:43740570;Name...
        40 	NC_045512.2 	RefSeq 	CDS 	26245 	26472 	. 	+ 	0 	ID=cds-YP_009724392.1;Parent=gene-GU280_gp04;D...
        41 	NC_045512.2 	RefSeq 	gene 	26523 	27191 	. 	+ 	. 	ID=gene-GU280_gp05;Dbxref=GeneID:43740571;Name...
        42 	NC_045512.2 	RefSeq 	CDS 	26523 	27191 	. 	+ 	0 	ID=cds-YP_009724393.1;Parent=gene-GU280_gp05;D...
        43 	NC_045512.2 	RefSeq 	gene 	27202 	27387 	. 	+ 	. 	ID=gene-GU280_gp06;Dbxref=GeneID:43740572;Name...
        44 	NC_045512.2 	RefSeq 	CDS 	27202 	27387 	. 	+ 	0 	ID=cds-YP_009724394.1;Parent=gene-GU280_gp06;D...
        45 	NC_045512.2 	RefSeq 	gene 	27394 	27759 	. 	+ 	. 	ID=gene-GU280_gp07;Dbxref=GeneID:43740573;Name...
        46 	NC_045512.2 	RefSeq 	CDS 	27394 	27759 	. 	+ 	0 	ID=cds-YP_009724395.1;Parent=gene-GU280_gp07;D...
        47 	NC_045512.2 	RefSeq 	gene 	27756 	27887 	. 	+ 	. 	ID=gene-GU280_gp08;Dbxref=GeneID:43740574;Name...
        48 	NC_045512.2 	RefSeq 	CDS 	27756 	27887 	. 	+ 	0 	ID=cds-YP_009725318.1;Parent=gene-GU280_gp08;D...
        49 	NC_045512.2 	RefSeq 	gene 	27894 	28259 	. 	+ 	. 	ID=gene-GU280_gp09;Dbxref=GeneID:43740577;Name...
        50 	NC_045512.2 	RefSeq 	CDS 	27894 	28259 	. 	+ 	0 	ID=cds-YP_009724396.1;Parent=gene-GU280_gp09;D...
        51 	NC_045512.2 	RefSeq 	gene 	28274 	29533 	. 	+ 	. 	ID=gene-GU280_gp10;Dbxref=GeneID:43740575;Name...
        52 	NC_045512.2 	RefSeq 	CDS 	28274 	29533 	. 	+ 	0 	ID=cds-YP_009724397.2;Parent=gene-GU280_gp10;D...
        53 	NC_045512.2 	RefSeq 	gene 	29558 	29674 	. 	+ 	. 	ID=gene-GU280_gp11;Dbxref=GeneID:43740576;Name...
        54 	NC_045512.2 	RefSeq 	CDS 	29558 	29674 	. 	+ 	0 	ID=cds-YP_009725255.1;Parent=gene-GU280_gp11;D...
        55 	NC_045512.2 	RefSeq 	stem_loop 	29609 	29644 	. 	+ 	. 	ID=id-GU280_gp11;Dbxref=GeneID:43740576;functi...
        56 	NC_045512.2 	RefSeq 	stem_loop 	29629 	29657 	. 	+ 	. 	ID=id-GU280_gp11-2;Dbxref=GeneID:43740576;func...
        57 	NC_045512.2 	RefSeq 	three_prime_UTR 	29675 	29903 	. 	+ 	. 	ID=id-NC_045512.2:29675..29903;gbkey=3'UTR\n
        58 	NC_045512.2 	RefSeq 	stem_loop 	29728 	29768 	. 	+ 	. 	ID=id-NC_045512.2:29728..29768;Note=basepair e...

From the output above we see that the annotation file has 58 rows. The first column provides the reference ID 'NC_045512.2'. The addition '.2' indicates the second version. The second column informs us that this is the reference sequence we are accessing. The third column describes the type of region on the genome. By using the DaaFrame function unique() we can see all the different types of 'Features' on the reference genome.

        annotationdf['Feature'].unique()
        
        array(['region', 'five_prime_UTR', 'gene', 'CDS',
       'mature_protein_region_of_CDS', 'stem_loop', 'three_prime_UTR'],
      dtype=object)


The fourth and fifth columns indicate the index positions (start and stop) of the region on the reference genome.  Notice that the CDS (coding sequence). The sixth, seventh, and eigth column are 'Score', 'Strand', and 'Frame', respectively, which we can ignore at this point. However, the final column 'Attribute' will be critical to access and record. The 'Attribute' column contains all the information provided the columns with additional metadata. 

During the project this was very confusing for the entire class. Despite being an early and initial procedure, the ambiguity of the project left many groups uncertain of the particular characteristics of the genome to focus on in the annotation file. After careful examination, you might notice the the gene, CDS, and mature protein region of CDS overlap with each other and often times are subsets of one another. 

For now what we need to do is decide what aspects of the reference genome we want to focus on. By understanding the technical biological details will understand the appropriate choice of feature to focus on is the CDS. So lets extract the details regarding the CDS features of the reference genome.

        CDSdf = annotationdf.loc[annotationdf['Feature'] == 'CDS']
        CDSdf.shape
        
        (13, 9)
        
From the above lines of code we have 13 difference CDS regions.


2. Collecting Sample SARS-CoV-2 Genomes from NCBI 

Each SARS-CoV-2 genome sample has an accession number that acts as a ID that we will need in order to use to query its genome from the NCBI database. We will need to compile a list of accession numbers. We will go to the resource page to download the [accession numbers](https://www.ncbi.nlm.nih.gov/sars-cov-2/download-nuccore-ids/). Currently as of December 20, 2021 there are about 10,000 accession numbers with over 15 prefixes. The prefixes with largest collection are 'MW' with 1483, 'MZ' with 2892, 'OL' with 3334, and 'OU' with '1068'. For now we will focus on these accession numbers with the prefix 'MW','MZ', 'OL', and 'OU'.

We will use our dictionary with the accession number to run a loop to feed our querying script.

To access the NCBI database we will use the NCBI BioPython module by importing 'Bio' by using its 'Entrez' library. We will write a Python script to query the database for nucleotide sequences in FASTA format. Our python script will use our accession number dictionary and access the keys of the prefixes we are interested in ('MW','MZ', 'OL', 'OU'). Each key will provide a list of accession numbers that will be passed through a function that will run repeat queries to the NCBI database to download each accession number's record in FASTA format. The downloaded record will have attributes (description, sequence) that we will save into a dictionary. 

It took approximately 15 minutes to query 1500 accession numbers from the NCBI database.


3. Create Protein Database in Blast from Reference Genome

This step in the work flow is where things will start to intesify with coding and planning for the database.

The reference genome sequence is currently coded as nucleotides. We need to first understand how we would like to align our samples to the reference genome. There are various approaches as we have different versions of Blast we could use. 

| BLAST Option | Query Type | Database Type | Comparison | Notes |
|:------------:|:----------:|:-------------:|:-----------:|:------------:|
| blastn | Nucleotide | Nucleotide | Nucleotide-Nucleotide | | 
| blastp | Protein | Protein | Protein-Protein| |
| tblastn | Protein | Nucleotide | Protein-Protein | The database is translated into protein |
| blastx | Nucleotide| Protein | Protein-Protein | The queries are translated into protein |
| tblastx | Nucleotide | Nucleotide | Protein-Protein | The queries and database are translated into protein |

We can actually use any of the options listed above but since we are ambitious to the amount of samples we want to process, we want to be intentional about which one. We can convert our reference genome sequences into amino acids/protein as well as our samples. However, once we translate our nucleotides into amino acids we lose any information to the nucleotides that were possibly deleted or inserted that resulted from mutation. However, processing 30,000 possible mutation locations would be overwhelming and those individual nucleotide mutations might not have big ramifications to the overall protein production. 

The option we will ultimately choose is 'tblastx' as we will use BLAST to convert our nucleotide sequences in our reference genome and our samples and only when an amino acid change is present. 

So what we will need to do create a database of each of the CDS regions represented with nucleotides.

We will need to parse the reference genome according to the different regions. Using the information from the annotation file we can parse our reference genome relative to the CDS regions of interest.

To create a database we will use 'makeblastdb' Unix Executable File. 

        ./makeblastdb -in cds-YP009724389_1.fasta -parse_seqids -blastdbversion 5 -title "SARS-CoV-2 DNA DB YP0097243891" =dbtype nucl

There are a few scripts that we would like to write (I will be writing them in python) that will do some common taks for us throughout this project. 

First let talk about FASTA format. I was unfamiliar with FASTA format prior to enrolling in my first bioinformatics course but came to learn that it was just another format used in the bioformatics field when handling files storing information on genetic information. It generally is a format with simple text where the sequences are in segments of 60 characters per line. In addition prior to providing the genetic information a header with any pertinent information about the organism is provided to the user. This line is a continuous text that terminates  with a new line, and is not restricted to the 60 characters like the genetic information. This header line also begins with a ">" to signal that the line is a header. Thus a fast file could contain multiple entries for different organisms. (Note that I use the term organism very generally and loosely as the genetic infroamtion doesn't have to be from an organism but just a protein found in certain cell or tissue.) 

One repetitive task that we will encounter is reading from a FASTA file and writing a file in FASTA format. Lets tackle the reading from a FASTA file. Lets take the example of reading from the reference genome. The first line will contain the header and the metadata and the line proceeding it will have its roughly 30,000 nucleotide sequence.

  1. To read the file we simple run this code:

          with open("sequence.fasta", "r") as reference_file:
            seq=''
            for line in reference_file:
              if(line[0]==">"):
                line = line.split("|")
                id = line[3]
              else:
                line.strip("\n")
                seq=seq+line

What the simple code above does is retrieve the accession number or identification number of the reference genome (used by NCBI) and its genome sequence and saves them in the declared varialbes 'id' and 'seq'. The few things we needed to do was to strip the new line character from the sequence each time and append/concatenate it to the existing sequence as we iterate through each line of the FASTA file. 

  2. To write the information to file we run this code (assuming already have the information held in a variable or data structure of some sort, let us assume that we have a dictionary of sequences where the key is the 'id' and the value is the sequence information):

          with open("writeSequence2File.fasta", "w") as writeSeq2File:
            for key, seq in sequence_dictionary.items():
              write='>'+str(key)+'\n'
              writeSeq2File.write(write)
              seqSize = len(seq)
              while(seqSize>60):
                write=seq[0:60]+'\n'
                writeSeq2File.write(write)
                seq=seq[60:]
                seqLen=len(seq)
              writeSeq2File.write(seq+'\n')

The two code snippets of code will guide you to be able to extract and save information throughout this project.               


5. Running Blast Alignment on the Sample Genomes 

Once we have created our database we will run the 'tblastx' Unix Executible File.

        ./blastx -query MT_query_completeOnly.fasta -out MToutput.txt -outfmt 0 -num_alignments 2 -evalue 0.00001
        
Now that we have run our alignments, we will need to scan through the results to see if the results are reasonable.

6. Collecting Information and Creating Database of Results

This next part of our project will require us to think through the data we need and wish to use and create a database taht will allow us to query information based on a number of parameters. 

We will use the output format of  __ where any mismatch or gaps or insertions are identified with characters that are not "." 

We will need to write a script that will comb through all the results and retrieve these mismatches and record their location and mismatch type so we can create a database we can access later to create our figures and analyze the results further.


1. open file
2. retrieve queryID
3. retrieve query line
4. retrieve database reerne seq line
5. find if database line is not "."
6. identify location (use strip)
7. all errors are in refernece to the refernece genome
