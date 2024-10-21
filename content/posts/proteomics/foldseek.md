+++
title = "foldseek usage (straightforward)"
bookToc=true
+++

- [foldseek github](https://github.com/steineggerlab/foldseek)
- [Foldseek User Guide](https://github.com/steineggerlab/foldseek/wiki)

## Installation: Conda installer (Linux and macOS)
```
conda install -c conda-forge -c bioconda foldseek
```

## Customize Database
```
folsseek createdb myDB_dir/ myDB
# myDB_dir has lots of pdb files 
```

## foldseek easy-search
```
usage: foldseek easy-search <i:PDB|mmCIF[.gz]> ... <i:PDB|mmCIF[.gz]>|<i:stdin> <i:targetFastaFile[.gz]>|<i:targetDB> <o:alignmentFile> <tmpDir> [options]
 By Martin Steinegger <martin.steinegger@snu.ac.kr>
options: prefilter:
 --comp-bias-corr INT           Correct for locally biased amino acid composition (range 0-1) [1]
 --comp-bias-corr-scale FLOAT   Correct for locally biased amino acid composition (range 0-1) [1.000]
 --seed-sub-mat TWIN            Substitution matrix file for k-mer generation [aa:3di.out,nucl:3di.out]
 -s FLOAT                       Sensitivity: 1.0 faster; 4.0 fast; 7.5 sensitive [9.500]
 -k INT                         k-mer length (0: automatically set to optimum) [6]
 --target-search-mode INT       target search mode (0: regular k-mer, 1: similar k-mer) [0]
 --k-score TWIN                 k-mer threshold for generating similar k-mer lists [seq:2147483647,prof:2147483647]
 --max-seqs INT                 Maximum results per query sequence allowed to pass the prefilter (affects sensitivity) [1000]
 --split INT                    Split input into N equally distributed chunks. 0: set the best split automatically [0]
 --split-mode INT               0: split target db; 1: split query db; 2: auto, depending on main memory [2]
 --split-memory-limit BYTE      Set max memory per split. E.g. 800B, 5K, 10M, 1G. Default (0) to all available system memory [0]
 --diag-score BOOL              Use ungapped diagonal scoring during prefilter [1]
 --exact-kmer-matching INT      Extract only exact k-mers for matching (range 0-1) [0]
 --mask INT                     Mask sequences in k-mer stage: 0: w/o low complexity masking, 1: with low complexity masking [0]
 --mask-prob FLOAT              Mask sequences is probablity is above threshold [1.000]
 --mask-lower-case INT          Lowercase letters will be excluded from k-mer search 0: include region, 1: exclude region [1]
 --min-ungapped-score INT       Accept only matches with ungapped alignment score above threshold [30]
 --spaced-kmer-mode INT         0: use consecutive positions in k-mers; 1: use spaced k-mers [1]
 --spaced-kmer-pattern STR      User-specified spaced k-mer pattern []
 --local-tmp STR                Path where some of the temporary files will be created []
 --exhaustive-search BOOL       Turns on an exhaustive all vs all search by by passing the prefilter step [0]
align:
 --min-seq-id FLOAT             List matches above this sequence identity (for clustering) (range 0.0-1.0) [0.000]
 -c FLOAT                       List matches above this fraction of aligned (covered) residues (see --cov-mode) [0.000]
 --cov-mode INT                 0: coverage of query and target
                                1: coverage of target
                                2: coverage of query
                                3: target seq. length has to be at least x% of query length
                                4: query seq. length has to be at least x% of target length
                                5: short seq. needs to be at least x% of the other seq. length [0]
 --max-rejected INT             Maximum rejected alignments before alignment calculation for a query is stopped [2147483647]
 --max-accept INT               Maximum accepted alignments before alignment calculation for a query is stopped [2147483647]
 -a BOOL                        Add backtrace string (convert to alignments with mmseqs convertalis module) [0]
 --sort-by-structure-bits INT   sort by bits*sqrt(alnlddt*alntmscore) [1]
 --alignment-mode INT           How to compute the alignment:
                                0: automatic
                                1: only score and end_pos
                                2: also start_pos and cov
                                3: also seq.id [3]
 --alignment-output-mode INT    How to compute the alignment:
                                0: automatic
                                1: only score and end_pos
                                2: also start_pos and cov
                                3: also seq.id
                                4: only ungapped alignment
                                5: score only (output) cluster format [0]
 -e DOUBLE                      List matches below this E-value (range 0.0-inf) [1.000E+01]
 --min-aln-len INT              Minimum alignment length (range 0-INT_MAX) [0]
 --seq-id-mode INT              0: alignment length 1: shorter, 2: longer sequence [0]
 --alt-ali INT                  Show up to this many alternative alignments [0]
 --gap-open TWIN                Gap open cost [aa:10,nucl:10]
 --gap-extend TWIN              Gap extension cost [aa:1,nucl:1]
profile:
 --num-iterations INT           Number of iterative profile search iterations [1]
misc:
 --tmscore-threshold FLOAT      accept alignments with a tmsore > thr [0.0,1.0] [0.000]
 --tmalign-hit-order INT        order hits by 0: (qTM+tTM)/2, 1: qTM, 2: tTM, 3: min(qTM,tTM) 4: max(qTM,tTM) [0]
 --tmalign-fast INT             turn on fast search in TM-align [1]
 --lddt-threshold FLOAT         accept alignments with a lddt > thr [0.0,1.0] [0.000]
 --alignment-type INT           How to compute the alignment:
                                0: 3di alignment
                                1: TM alignment
                                2: 3Di+AA [2]
 --exact-tmscore INT            turn on fast exact TMscore (slow), default is approximate [0]
 --prefilter-mode INT           prefilter mode: 0: kmer/ungapped 1: ungapped, 2: nofilter [0]
 --cluster-search INT           first find representative then align all cluster members [0]
 --mask-bfactor-threshold FLOAT mask residues for seeding if b-factor < thr [0,100] [0.000]
 --input-format INT             Format of input structures:
                                0: Auto-detect by extension
                                1: PDB
                                2: mmCIF
                                3: mmJSON
                                4: ChemComp
                                5: Foldcomp [0]
 --file-include STR             Include file names based on this regex [.*]
 --file-exclude STR             Exclude file names based on this regex [^$]
 --format-mode INT              Output format:
                                0: BLAST-TAB
                                1: SAM
                                2: BLAST-TAB + query/db length
                                3: Pretty HTML
                                4: BLAST-TAB + column headers
                                5: Calpha only PDB super-posed to query
                                BLAST-TAB (0) and BLAST-TAB + column headers (4)support custom output formats (--format-output)
                                (5) Superposed PDB files (Calpha only) [0]
 --format-output STR            Choose comma separated list of output columns from: query,target,evalue,gapopen,pident,fident,nident,qstart,qend,qlen
                                tstart,tend,tlen,alnlen,raw,bits,cigar,qseq,tseq,qheader,theader,qaln,taln,mismatch,qcov,tcov
                                qset,qsetid,tset,tsetid,taxid,taxname,taxlineage,
                                lddt,lddtfull,qca,tca,t,u,qtmscore,ttmscore,alntmscore,rmsd,prob
                                complexqtmscore,complexttmscore,complexu,complext,complexassignid
                                 [query,target,fident,alnlen,mismatch,gapopen,qstart,qend,tstart,tend,evalue,bits]
 --greedy-best-hits BOOL        Choose the best hits greedily to cover the query [0]
common:
 --db-load-mode INT             Database preload mode 0: auto, 1: fread, 2: mmap, 3: mmap+touch [0]
 --threads INT                  Number of CPU-cores used (all by default) [40]
 -v INT                         Verbosity level: 0: quiet, 1: +errors, 2: +warnings, 3: +info [3]
 --sub-mat TWIN                 Substitution matrix file [aa:3di.out,nucl:3di.out]
 --max-seq-len INT              Maximum sequence length [65535]
 --compressed INT               Write compressed output [0]
 --remove-tmp-files BOOL        Delete temporary files [1]
 --mpi-runner STR               Use MPI on compute cluster with this MPI command (e.g. "mpirun -np 42") []
 --force-reuse BOOL             Reuse tmp filse in tmp/latest folder ignoring parameters and version changes [0]
 --prostt5-model STR            Path to ProstT5 model []
expert:
 --zdrop INT                    Maximal allowed difference between score values before alignment is truncated  (nucleotide alignment only) [40]
 --taxon-list STR               Taxonomy ID, possibly multiple values separated by ',' []
 --chain-name-mode INT          Add chain to name:
                                0: auto
                                1: always add
                                 [0]
 --write-mapping INT            write _mapping file containing mapping from internal id to taxonomic identifier [0]
 --coord-store-mode INT         Coordinate storage mode:
                                1: C-alpha as float
                                2: C-alpha as difference (uint16_t) [2]
 --write-lookup INT             write .lookup file containing mapping from internal id, fasta id and file number [1]
 --db-output BOOL               Return a result DB instead of a text file [0]

examples:
 # Search a single/multiple PDB file against a set of PDB files
 foldseek easy-search examples/d1asha_ examples/ result.m8 tmp
 # Format output differently
 foldseek easy-search examples/d1asha_ examples/ result.m8 tmp --format-output query,target,qstart,tstart,cigar
 # Align with TMalign (global)
 foldseek easy-search examples/d1asha_ examples/ result.m8 tmp --alignment-type 1
 # Skip prefilter and perform an exhaustive alignment (slower but more sensitive)
 foldseek easy-search examples/d1asha_ examples/ result.m8 tmp --exhaustive-search 1
```