### blobtools commands and other stuff 16-jan-2020

instructions blobtools website 
```
# Download database
wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Reference_Proteomes_2017_07.tar.gz

# Unpack protein FASTAs for each kingdom
parallel -j4 'gunzip {}' ::: 'ls | grep "fasta.gz" | grep -v 'DNA' | grep -v 'additional''

# Concatenate all protein sequences into uniprot_ref_proteomes.fasta
cat */*.fasta > uniprot_ref_proteomes.fasta

# Simplify sequence IDs
cat uniprot_ref_proteomes.fasta | sed -r 's/(^>sp\|)|(^>tr\|)/>/g' | cut -f1 -d"|" > temp; mv temp uniprot_ref_proteomes.fasta

# Make Diamond database
diamond makedb --in uniprot_ref_proteomes.fasta -d uniprot_ref_proteomes.diamond

# Subset mapping file to only contain NCBI TaxID entries
cat */*.idmapping | grep "NCBI_TaxID" > uniprot_ref_proteomes.taxids
```

making the blast file for blobtools
```
blastn -db /home/jon/Working_Files/blast_db_nt/nt \
       -query out_consensusScaffold.fa \
       -outfmt "6 qseqid staxids bitscore std" \
       -max_target_seqs 10 \
       -max_hsps 1 \
       -evalue 1e-25 \
       -num_threads 40 \
       -out blast.out
```

### 17-Jan-2020

huh, blastn is spitting out a weird error. I suspect it is from the way I extracted the files. I am going to delete them and copy the backup on my backup hard drive and try again. I have not been able to figure out why the 'parallel' command is giving me weird results though. 

```
Error: (117.2) CThread::Wrapper: CThread::Main() failed(CSeqDBException::eFileErr) CSeqDBAtlas::MapMmap: While mapping file [/home/jon/Working_Files/blast_db_nt/nt.11.nsq] with 9753147998 bytes allocated, caught exception:
NCBI C++ Exception:
    T33 "/home/coremake/release_build/build/PrepareRelease_Linux64-Centos_JSID_01_80348_130.14.18.6_9008__PrepareRelease_Linux64-Centos_1433254587/c++/compilers/unix/../../src/objtools/blast/seqdb_reader/seqdbatlas.cpp", line 154: Error: BLASTDB::ncbi::SeqDB_ThrowException() - Validation failed: [end <= file_size] at /home/coremake/release_build/build/PrepareRelease_Linux64-Centos_JSID_01_80348_130.14.18.6_9008__PrepareRelease_Linux64-Centos_1433254587/c++/compilers/unix/../../src/objtools/blast/seqdb_reader/seqdbatlas.cpp:508
```

extracting and sorting the silva rna file worked fine though
```
# Download ID mapping file from RNAcentral release 14.0
wget ftp://ftp.ebi.ac.uk/pub/databases/RNAcentral/releases/14.0/id_mapping/id_mapping.tsv.gz

# extract SILVA sequence IDs (this is the TaxID-mapping file)
gunzip -c id_mapping.tsv.gz | grep SILVA | sort | uniq > id_mapping.SILVA.tsv

# generate a list of sequence IDs
cut -f1 id_mapping.SILVA.tsv > id_mapping.SILVA.names.txt

# Download FASTA from RNAcentral release 14.0 (contains all sequences)
wget ftp://ftp.ebi.ac.uk/pub/databases/RNAcentral/releases/14.0/sequences/rnacentral_active.fasta.gz
gunzip rnacentral_active.fasta.gz

# Subselect only SILVA rRNA sequences
blobtools seqfilter -i rnacentral_active.fasta -l id_mapping.SILVA.names.tsv 
```

> throws error saying id_mapping.SILVA.names.tsv does not exist. Not sure if this is because it was converted to a txt file. tweaked command to look for txt instead. seems to be working. 

```
# command threw this error. Will continue, but I may have to come back and redo this step with the correct file. 
[+] Parsing list - id_mapping.SILVA.names.txt
[+] Filtering rnacentral_active.fasta ...
[%]   100%
Traceback (most recent call last):
  File "/home/jon/anaconda3/envs/blobtools/opt/blobtools-1.0.1/lib/seqfilter.py", line 67, in <module>
    main()
  File "/home/jon/anaconda3/envs/blobtools/opt/blobtools-1.0.1/lib/seqfilter.py", line 56, in main
    print BtLog.status_d['23'] % ('{:.2%}'.format(items_parsed_count/sequences), "{:,}".format(items_count), "{:,}".format(items_parsed_count), "{:,}".format(sequences))
TypeError: not all arguments converted during string formatting

```

```

# Convert rRNA to rDNA
perl -ne 'chomp; if(m/^>/){@temp=split(" "); print $temp[0]."\n";} else {$_ =~ tr/U/T/; print $_."\n"}' rnacentral_active.filtered.fna > rnacentral_active.SILVA.rDNA.fasta
```

> threw error that .fna file wasn't there. redoing command with .fasta instead. Also, there is no rnacentral_active.filtered.fna file. So I removed the .filtered. Not sure where it was supposed to come from. 

```
# make the blastDB
makeblastdb -dbtype nucl -in rnacentral_active.SILVA.rDNA.fasta
```

makeblastdb complained about invalid residues. See below for output. I am going to ignore this for now and move on. Online comments make is seem like it is due to an makeblastdb version incompatibility. One person downgraded to an older version and it worked fine. May need to do that later. 

```
Building a new DB, current time: 01/17/2020 13:21:04
New DB name:   /home/jon/Working_Files/silva_rna/rnacentral_active.SILVA.rDNA.fasta
New DB title:  rnacentral_active.SILVA.rDNA.fasta
Sequence type: Nucleotide
Keep MBits: T
Maximum file size: 1000000000B
FASTA-Reader: Ignoring invalid residues at position(s): On line 3609794: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 9992138: 6
FASTA-Reader: Ignoring invalid residues at position(s): On line 10194855: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 10762724: 10-19
FASTA-Reader: Ignoring invalid residues at position(s): On line 11294433: 33, 35, 48
FASTA-Reader: Ignoring invalid residues at position(s): On line 17045412: 6, 11, 18-19, 23-25
FASTA-Reader: Ignoring invalid residues at position(s): On line 17298724: 9
FASTA-Reader: Ignoring invalid residues at position(s): On line 18712674: 8
FASTA-Reader: Ignoring invalid residues at position(s): On line 21265084: 34
FASTA-Reader: Ignoring invalid residues at position(s): On line 23334289: 16
FASTA-Reader: Ignoring invalid residues at position(s): On line 24728856: 7
FASTA-Reader: Ignoring invalid residues at position(s): On line 26545990: 39, 43
FASTA-Reader: Ignoring invalid residues at position(s): On line 28708024: 10
FASTA-Reader: Ignoring invalid residues at position(s): On line 30252114: 18
FASTA-Reader: Ignoring invalid residues at position(s): On line 38197856: 28
FASTA-Reader: Ignoring invalid residues at position(s): On line 39757892: 5
FASTA-Reader: Ignoring invalid residues at position(s): On line 41124993: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 43323427: 12
FASTA-Reader: Ignoring invalid residues at position(s): On line 43445939: 18
FASTA-Reader: Ignoring invalid residues at position(s): On line 44987837: 13
FASTA-Reader: Ignoring invalid residues at position(s): On line 55405563: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 57301437: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 60901146: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 62541126: 17
FASTA-Reader: Ignoring invalid residues at position(s): On line 64085147: 3
FASTA-Reader: Ignoring invalid residues at position(s): On line 65648083: 5
FASTA-Reader: Ignoring invalid residues at position(s): On line 66650400: 28
FASTA-Reader: Ignoring invalid residues at position(s): On line 75133719: 5
FASTA-Reader: Ignoring invalid residues at position(s): On line 75829195: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 76167877: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 77306609: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 79454111: 6
FASTA-Reader: Ignoring invalid residues at position(s): On line 80224838: 8
FASTA-Reader: Ignoring invalid residues at position(s): On line 81384867: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 83307431: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 84155722: 10
FASTA-Reader: Ignoring invalid residues at position(s): On line 85490092: 17
FASTA-Reader: Ignoring invalid residues at position(s): On line 86291406: 16
FASTA-Reader: Ignoring invalid residues at position(s): On line 89466243: 2
FASTA-Reader: Ignoring invalid residues at position(s): On line 90483358: 39
FASTA-Reader: Ignoring invalid residues at position(s): On line 96095333: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 99047816: 9-10
FASTA-Reader: Ignoring invalid residues at position(s): On line 102059417: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 104436517: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 104933253: 18
FASTA-Reader: Ignoring invalid residues at position(s): On line 105844313: 8
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244288: 57
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244307: 28
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244309: 16
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244311: 14
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244312: 39
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244314: 57
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244315: 60
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244316: 49
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244323: 17
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244324: 40
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244330: 54
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244331: 26
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244347: 42
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244349: 40, 57
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244353: 49
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244354: 45, 54
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244356: 36
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244357: 35, 51
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244359: 32, 43
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244360: 29
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244361: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244362: 51
FASTA-Reader: Ignoring invalid residues at position(s): On line 106244363: 10
FASTA-Reader: Ignoring invalid residues at position(s): On line 106724671: 7
FASTA-Reader: Ignoring invalid residues at position(s): On line 110700320: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 113642502: 17
FASTA-Reader: Ignoring invalid residues at position(s): On line 114539001: 24
FASTA-Reader: Ignoring invalid residues at position(s): On line 116600434: 5-8
FASTA-Reader: Ignoring invalid residues at position(s): On line 116986783: 13
FASTA-Reader: Ignoring invalid residues at position(s): On line 119155520: 1-28, 32-60
FASTA-Reader: Ignoring invalid residues at position(s): On line 119155521: 1-8
FASTA-Reader: Ignoring invalid residues at position(s): On line 120854175: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 122751774: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 128513383: 10
FASTA-Reader: Ignoring invalid residues at position(s): On line 134054008: 3, 5
FASTA-Reader: Ignoring invalid residues at position(s): On line 134162456: 3
FASTA-Reader: Ignoring invalid residues at position(s): On line 135307069: 8, 13
FASTA-Reader: Ignoring invalid residues at position(s): On line 140708281: 1-6, 8-9, 12
FASTA-Reader: Ignoring invalid residues at position(s): On line 142238050: 6, 8
FASTA-Reader: Ignoring invalid residues at position(s): On line 142510865: 23
FASTA-Reader: Ignoring invalid residues at position(s): On line 146095972: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 147613962: 16
FASTA-Reader: Ignoring invalid residues at position(s): On line 151953324: 28
FASTA-Reader: Ignoring invalid residues at position(s): On line 151953335: 19, 48
FASTA-Reader: Ignoring invalid residues at position(s): On line 151953345: 6
FASTA-Reader: Ignoring invalid residues at position(s): On line 152471131: 1, 3, 9, 11
FASTA-Reader: Ignoring invalid residues at position(s): On line 155058679: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 155693589: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 155928260: 6
FASTA-Reader: Ignoring invalid residues at position(s): On line 157334660: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 158864331: 8
FASTA-Reader: Ignoring invalid residues at position(s): On line 159399949: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 164897559: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 169777856: 27
FASTA-Reader: Ignoring invalid residues at position(s): On line 172242216: 48
FASTA-Reader: Ignoring invalid residues at position(s): On line 173106005: 9, 13
FASTA-Reader: Ignoring invalid residues at position(s): On line 175943138: 5
FASTA-Reader: Ignoring invalid residues at position(s): On line 176310888: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 176728082: 39
FASTA-Reader: Ignoring invalid residues at position(s): On line 177998520: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 178236839: 36
FASTA-Reader: Ignoring invalid residues at position(s): On line 178619186: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 178747570: 8, 10-11, 20, 22-23
FASTA-Reader: Ignoring invalid residues at position(s): On line 180032024: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 180032113: 1-28, 32-60
FASTA-Reader: Ignoring invalid residues at position(s): On line 180032114: 1-4
FASTA-Reader: Ignoring invalid residues at position(s): On line 180166455: 47
FASTA-Reader: Ignoring invalid residues at position(s): On line 180445385: 48-50
FASTA-Reader: Ignoring invalid residues at position(s): On line 181700358: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 183889196: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 189393454: 34-60
FASTA-Reader: Ignoring invalid residues at position(s): On line 189393455: 1-24
FASTA-Reader: Ignoring invalid residues at position(s): On line 190705514: 50
FASTA-Reader: Ignoring invalid residues at position(s): On line 191959843: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 192106566: 26
FASTA-Reader: Ignoring invalid residues at position(s): On line 192355878: 6
FASTA-Reader: Ignoring invalid residues at position(s): On line 192976125: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 193744352: 34-60
FASTA-Reader: Ignoring invalid residues at position(s): On line 193744353: 1-25
FASTA-Reader: Ignoring invalid residues at position(s): On line 196688899: 2, 4, 7-9, 15, 20, 22-23, 29, 31-33, 37, 41-42, 45-48, 50
FASTA-Reader: Ignoring invalid residues at position(s): On line 197068253: 21
FASTA-Reader: Ignoring invalid residues at position(s): On line 198477316: 5
FASTA-Reader: Ignoring invalid residues at position(s): On line 199247852: 17
FASTA-Reader: Ignoring invalid residues at position(s): On line 202061141: 4, 6, 8, 10, 12, 16, 18
FASTA-Reader: Ignoring invalid residues at position(s): On line 205431788: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 211457630: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 211713830: 41, 45
FASTA-Reader: Ignoring invalid residues at position(s): On line 211817646: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 212488085: 16
FASTA-Reader: Ignoring invalid residues at position(s): On line 216307179: 3, 12, 20-21, 25-26, 31
FASTA-Reader: Ignoring invalid residues at position(s): On line 216335986: 16
FASTA-Reader: Ignoring invalid residues at position(s): On line 217338526: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 220535949: 45
FASTA-Reader: Ignoring invalid residues at position(s): On line 225137288: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 225137289: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 230643020: 2-3, 6, 8, 11-13, 19, 24, 26-27, 33, 35-37, 41, 45-46, 49-52, 54, 58, 60
FASTA-Reader: Ignoring invalid residues at position(s): On line 230643021: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 230643023: 33
FASTA-Reader: Ignoring invalid residues at position(s): On line 232691851: 4
FASTA-Reader: Ignoring invalid residues at position(s): On line 234754429: 8
FASTA-Reader: Ignoring invalid residues at position(s): On line 235395775: 10
FASTA-Reader: Ignoring invalid residues at position(s): On line 237189229: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 238331567: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 240780055: 13
FASTA-Reader: Ignoring invalid residues at position(s): On line 243165888: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 245171350: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 249223275: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 249764523: 1
FASTA-Reader: Ignoring invalid residues at position(s): On line 250009712: 25
FASTA-Reader: Ignoring invalid residues at position(s): On line 253571846: 1, 3, 5, 7, 9, 11, 17
FASTA-Reader: Ignoring invalid residues at position(s): On line 254742086: 10
FASTA-Reader: Ignoring invalid residues at position(s): On line 255108116: 29
FASTA-Reader: Ignoring invalid residues at position(s): On line 257016182: 23
FASTA-Reader: Ignoring invalid residues at position(s): On line 258164389: 47
FASTA-Reader: Ignoring invalid residues at position(s): On line 258677400: 55
FASTA-Reader: Ignoring invalid residues at position(s): On line 262051721: 1
Adding sequences from FASTA; added 24654115 sequences in 1258.44 seconds.
```

> another note to self, blobtools uses python 2.7. However, conda was able to install it in the biotools environment which uses pythong 3, but it throws errors and doesn't work. 
> Note to self, extracting files in a directory recursively, just use gunzip -dr [name_of_directory] works. However, this didn't work for .tar.gz for reasons I don't know. But the internet gave me this: 

```
find -name "*tar.gz" -exec tar xvzf '{}' \;
```
which apparently works for extracting the blast_db_nt files



-------------------------------------------------------------------------------------------------------------------------------------
Ok, moving on to the uniprot database stuff

```
# Download database
wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Reference_Proteomes_2017_07.tar.gz

# Unpack protein FASTAs for each kingdom
parallel -j4 'gunzip {}' ::: 'ls | grep "fasta.gz" | grep -v 'DNA' | grep -v 'additional'
```
the parallel command thing didn't seem to work. so I used gunzip -dr instead. 



```
# Concatenate all protein sequences into uniprot_ref_proteomes.fasta
cat */*.fasta > uniprot_ref_proteomes.fasta

# Simplify sequence IDs
cat uniprot_ref_proteomes.fasta | sed -r 's/(^>sp\|)|(^>tr\|)/>/g' | cut -f1 -d"|" > temp; mv temp uniprot_ref_proteomes.fasta

# Make Diamond database
diamond makedb --in uniprot_ref_proteomes.fasta -d uniprot_ref_proteomes.diamond
```

diamond makedb threw error. see below. No idea why. 
```
diamond v0.9.29.130 | by Benjamin Buchfink <buchfink@gmail.com>
Licensed under the GNU GPL <https://www.gnu.org/licenses/gpl.txt>
Check http://github.com/bbuchfink/diamond for updates.

#CPU threads: 80
Scoring parameters: (Matrix=BLOSUM62 Lambda=0.267 K=0.041 Penalties=11/1)
Database file: uniprot_ref_proteomes.fasta
Opening the database file...  [0.000244s]
Loading sequences...  [8.02545s]
Masking sequences...  [5.31739s]
Writing sequences...  [1.6549s]
Hashing sequences...  [0.419618s]
Loading sequences...  [0.457492s]
Error: Error reading input stream at line 20017250: Invalid character (3) in sequence
```

I used ```sed '20017250q;d' uniprot_ref_proteomes.fasta``` to view the specific line that is throwing the error. which returned ``` ACCATTTTAACAGCAACATAABAB35156 has been suppressed at the sub```. This looks fishy. I used ```ack --lines=20017245-20017255 uniprot_ref_proteomes.fasta``` to get a few lines on either side, which returned

```
TCTGTTTATGGAGATGCCGCAGAATGGAACACAGCCGAATTAAGAAGAGAAATGTCGCAC
TCATAG
>P0ADE0
ATGACTAAAGTACGTAATTGCGTTCTTGATGCACTTTCCATCAACGTCAACAACATCATT
AGCTTGGTCGTGGGTACTTTCCCTCAGGACCCGACAGTGTCAAAAACGGCTGTCATCCTA
ACCATTTTAACAGCAACATAABAB35156 has been suppressed at the sub
mitter's request on 2004-10-23 15:50:21.0BAB35486 has been s
uppressed at the submitter's request on 2004-10-23 15:50:22.
0BAB37895 has been suppressed at the submitter's request on 
2004-10-23 15:50:28.0
>P0ADE5
```
and that looks like a incorrectly formated fasta file. This tells me that one of the above formatting commands did something incorrectly. That's what I get for trusting the internet. 

Ok, so here is the plan of attack. I have to remake the uniprot_ref_proteomes.fasta file. Then I will check to see if the same lines are formatted as above. If not, I think this tells me either the sed command or the cut command is what is improperly subsetting the file. 
```
the options selected on the cut -f1 -d"|" commands do:
  -d, --delimiter=DELIM   use DELIM instead of TAB for field delimiter
  -f, --fields=LIST       select only these fields;  also print any line that contains no delimiter character, unless the -s option is specified
```

If I understand this correctly then, the ```cut -f1 -d"|"``` command nabs only the first identifier for each sequence in the file. As in ```>POADEO``` which makes me thing that it's the sed command which is messing this up. 

whelp, checking the file again returned
```
TCTGTTTATGGAGATGCCGCAGAATGGAACACAGCCGAATTAAGAAGAGAAATGTCGCAC
TCATAG
>P0ADE0
ATGACTAAAGTACGTAATTGCGTTCTTGATGCACTTTCCATCAACGTCAACAACATCATT
AGCTTGGTCGTGGGTACTTTCCCTCAGGACCCGACAGTGTCAAAAACGGCTGTCATCCTA
ACCATTTTAACAGCAACATAABAB35156 has been suppressed at the sub
mitter's request on 2004-10-23 15:50:21.0BAB35486 has been s
uppressed at the submitter's request on 2004-10-23 15:50:22.
0BAB37895 has been suppressed at the submitter's request on 
2004-10-23 15:50:28.0
>P0ADE5
```
which means that the problem is in the fasta file merging thing or the the decompression step? 

ok, so because I didn't use the parallel command, everything was unzipped, not just the specific files they wanted. So when I go to use the "combine all fasta files" command I need to make sure to specific only the files that actually wanted to combine. This should look like 

```
ls Archaea/ Eukaryota/ Viruses/ Bacteria/ | grep "fasta" | grep -v 'DNA' | grep -v 'additional' | gunzip -
```

trying a different approach. This works if I am in the folder containing all the files
```
ls | grep "fasta" | grep -v 'DNA' | grep -v 'additional' | xargs gzip -d

# Concatenate all protein sequences into uniprot_ref_proteomes.fasta
cat */*.fasta > uniprot_ref_proteomes.fasta

ls | grep "idmapping" | xargs gzip -d
```


```
# Subset mapping file to only contain NCBI TaxID entries
cat */*.idmapping | grep "NCBI_TaxID" > uniprot_ref_proteomes.taxids
```


```
diamond blastx \
        --query out_consensusScaffold.fa \
        --db /home/jon/Working_Files/uniprot/Reference_Proteomes_2019_11/uniprot_ref_proteomes.diamond.dmnd \
        --outfmt 6 qseqid staxids bitscore qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore \
        --sensitive \
        --max-target-seqs 1 \
        --evalue 1e-25 \
        --threads 35 \
        > diamond.out


```

threw error ```Error: Output format requires taxonomy mapping information built into the database (use --taxonmap parameter for the makedb command).``` I am going to add --taxonmap parameter to the the makedb

```
diamond makedb --taxonmap uniprot_ref_proteomes.taxids --in uniprot_ref_proteomes.fasta -d uniprot_ref_proteomes.diamond


Writing taxon id lists...  [15.9726s]
0 sequences mapped to taxonomy, 0 total mappings.
Closing the input file...  [3.8e-05s]
Closing the database file...  [4.4e-05s]
Database hash = 35a66d4a1faf09a3cca5b58b3702067d
Processed 45174572 sequences, 16650131828 letters.
Total time = 443.934s

```

interesting results....

running diamond blastx command as seen above. 