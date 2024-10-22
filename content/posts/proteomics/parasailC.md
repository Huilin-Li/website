+++
title = "parasail in C usage (straightforward)"
bookToc=true
+++
I want to use its **Tracebacks** functions, however, this function has not been involved in Python version. Therefore, I am exploring its C usage.

## installation ("building from a git clone")
{{< figure src="../img/parasailC.png" title="Installation instructions on the parasail Github">}}

**straightforward steps:**
1. `cd /storage/lihuilin/` 
2. `git clone https://github.com/jeffdaily/parasail`
3. `cd parasail`
4. `autoreconf -fi`
<details>
  <summary><i>output</i></summary>

```
libtoolize: putting auxiliary files in AC_CONFIG_AUX_DIR, 'build-aux'.
libtoolize: copying file 'build-aux/ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'm4'.
libtoolize: copying file 'm4/libtool.m4'
libtoolize: copying file 'm4/ltoptions.m4'
libtoolize: copying file 'm4/ltsugar.m4'
libtoolize: copying file 'm4/ltversion.m4'
libtoolize: copying file 'm4/lt~obsolete.m4'
configure.ac:109: installing 'build-aux/compile'
configure.ac:78: installing 'build-aux/config.guess'
configure.ac:78: installing 'build-aux/config.sub'
configure.ac:70: installing 'build-aux/install-sh'
configure.ac:70: installing 'build-aux/missing'
Makefile.am: installing 'build-aux/depcomp'
parallel-tests: installing 'build-aux/test-driver'
```
</details>



5. `mkdir -p /storage/lihuilin/parasail/local`
6. `./configure --prefix=/storage/lihuilin/parasail/local/parasail`
<details>
  <summary><i>output</i></summary>

```
... ...
-=-=-=-=-=-=-=-=-=-= Configuration Complete =-=-=-=-=-=-=-=-=-=-=-

  Configuration summary :

    parasail version : .................... 2.6.2

    Host CPU : ............................ x86_64
    Host Vendor : ......................... pc
    Host OS : ............................. linux-gnu

  Toolchain :

    CC : .................................. gcc (gnu, 8.3.1)
    CXX : ................................. g++ (gnu, 8.3.1)

  Flags :

    CFLAGS : .............................. -g -O2
    CXXFLAGS : ............................ -g -O2
    CPPFLAGS : ............................
    LDFLAGS : .............................
    LIBS : ................................

  Intrinsics :

    SSE2 : ................................ auto (yes)
    SSE2_CFLAGS : .........................

    SSE4.1 : .............................. auto (yes)
    SSE41_CFLAGS : ........................ -msse4.1

    AVX2 : ................................ auto (yes)
    AVX2_CFLAGS : ......................... -mavx2

    AVX512 : .............................. auto (yes)
    AVX512F_CFLAGS : ...................... -mavx512f
    AVX512BW_CFLAGS : ..................... -mavx512bw
    AVX512VBMI_CFLAGS : ................... -mavx512vbmi

    Altivec : ............................. auto (no)
    ALTIVEC_CFLAGS : ...................... not supported

    ARM NEON : ............................ auto (no)
    NEON_CFLAGS : ......................... not supported
    EXTRA_NEON_CFLAGS : ................... -fopenmp-simd -DSIMDE_ENABLE_OPENMP

  Dependencies :

    OPENMP_CFLAGS : ....................... -fopenmp
    OPENMP_CXXFLAGS : ..................... -fopenmp

    CLOCK_LIBS : ..........................
    MATH_LIBS : ........................... -lm
    Z_CFLAGS : ............................
    Z_LIBS : .............................. -lz

  Installation directories :

    Program directory : ................... /storage/lihuilin/parasail/local/parasail/bin
    Library directory : ................... /storage/lihuilin/parasail/local/parasail/lib
    Include directory : ................... /storage/lihuilin/parasail/local/parasail/include
    Pkgconfig directory : ................. /storage/lihuilin/parasail/local/parasail/lib/pkgconfig

Compiling some other packages against parasail may require
the addition of '/storage/lihuilin/parasail/local/parasail/lib/pkgconfig' to the
PKG_CONFIG_PATH environment variable.
```
</details>

7. `make`
<details>
  <summary><i>output</i></summary>

```
make  all-am
make[1]: Entering directory '/storage/lihuilin/parasail'
... ...
make[1]: Leaving directory '/storage/lihuilin/parasail'
```
</details>

8. `make install`
<details>
  <summary><i>output</i></summary>

```
make[1]: Entering directory '/storage/lihuilin/parasail'
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/lib'
 /bin/sh ./libtool   --mode=install /usr/bin/install -c   libparasail.la '/storage/lihuilin/parasail/local/parasail/lib'
libtool: install: /usr/bin/install -c .libs/libparasail.so.8.1.2 /storage/lihuilin/parasail/local/parasail/lib/libparasail.so.8.1.2
libtool: install: (cd /storage/lihuilin/parasail/local/parasail/lib && { ln -s -f libparasail.so.8.1.2 libparasail.so.8 || { rm -f libparasail.so.8 && ln -s libparasail.so.8.1.2 libparasail.so.8; }; })
libtool: install: (cd /storage/lihuilin/parasail/local/parasail/lib && { ln -s -f libparasail.so.8.1.2 libparasail.so || { rm -f libparasail.so && ln -s libparasail.so.8.1.2 libparasail.so; }; })
libtool: install: /usr/bin/install -c .libs/libparasail.lai /storage/lihuilin/parasail/local/parasail/lib/libparasail.la
libtool: install: /usr/bin/install -c .libs/libparasail.a /storage/lihuilin/parasail/local/parasail/lib/libparasail.a
libtool: install: chmod 644 /storage/lihuilin/parasail/local/parasail/lib/libparasail.a
libtool: install: ranlib /storage/lihuilin/parasail/local/parasail/lib/libparasail.a
libtool: finish: PATH="/home/lihuilin/miniconda3/condabin:/home/lihuilin/.local/bin:/home/lihuilin/bin:/opt/slurm/sbin:/opt/slurm/bin:/soft/modules/modules-4.7.0/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/lihuilin/software/silent_tools:/sbin" ldconfig -n /storage/lihuilin/parasail/local/parasail/lib
----------------------------------------------------------------------
Libraries have been installed in:
   /storage/lihuilin/parasail/local/parasail/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/bin'
  /bin/sh ./libtool   --mode=install /usr/bin/install -c apps/parasail_aligner apps/parasail_stats '/storage/lihuilin/parasail/local/parasail/bin'
libtool: install: /usr/bin/install -c apps/.libs/parasail_aligner /storage/lihuilin/parasail/local/parasail/bin/parasail_aligner
libtool: install: /usr/bin/install -c apps/.libs/parasail_stats /storage/lihuilin/parasail/local/parasail/bin/parasail_stats
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/include'
 /usr/bin/install -c -m 644 parasail.h '/storage/lihuilin/parasail/local/parasail/include'
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/include'
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/include/parasail/matrices'
 /usr/bin/install -c -m 644  parasail/matrices/blosum100.h parasail/matrices/blosum30.h parasail/matrices/blosum35.h parasail/matrices/blosum40.h parasail/matrices/blosum45.h parasail/matrices/blosum50.h parasail/matrices/blosum55.h parasail/matrices/blosum60.h parasail/matrices/blosum62.h parasail/matrices/blosum65.h parasail/matrices/blosum70.h parasail/matrices/blosum75.h parasail/matrices/blosum80.h parasail/matrices/blosum85.h parasail/matrices/blosum90.h parasail/matrices/blosum_map.h parasail/matrices/blosumn.h parasail/matrices/dnafull.h parasail/matrices/nuc44.h parasail/matrices/pam10.h parasail/matrices/pam100.h parasail/matrices/pam110.h parasail/matrices/pam120.h parasail/matrices/pam130.h parasail/matrices/pam140.h parasail/matrices/pam150.h parasail/matrices/pam160.h parasail/matrices/pam170.h parasail/matrices/pam180.h parasail/matrices/pam190.h parasail/matrices/pam20.h parasail/matrices/pam200.h parasail/matrices/pam210.h parasail/matrices/pam220.h parasail/matrices/pam230.h parasail/matrices/pam240.h parasail/matrices/pam250.h parasail/matrices/pam260.h parasail/matrices/pam270.h parasail/matrices/pam280.h '/storage/lihuilin/parasail/local/parasail/include/parasail/matrices'
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/include/parasail/matrices'
 /usr/bin/install -c -m 644  parasail/matrices/pam290.h parasail/matrices/pam30.h parasail/matrices/pam300.h parasail/matrices/pam310.h parasail/matrices/pam320.h parasail/matrices/pam330.h parasail/matrices/pam340.h parasail/matrices/pam350.h parasail/matrices/pam360.h parasail/matrices/pam370.h parasail/matrices/pam380.h parasail/matrices/pam390.h parasail/matrices/pam40.h parasail/matrices/pam400.h parasail/matrices/pam410.h parasail/matrices/pam420.h parasail/matrices/pam430.h parasail/matrices/pam440.h parasail/matrices/pam450.h parasail/matrices/pam460.h parasail/matrices/pam470.h parasail/matrices/pam480.h parasail/matrices/pam490.h parasail/matrices/pam50.h parasail/matrices/pam500.h parasail/matrices/pam60.h parasail/matrices/pam70.h parasail/matrices/pam80.h parasail/matrices/pam90.h parasail/matrices/pam_map.h '/storage/lihuilin/parasail/local/parasail/include/parasail/matrices'
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/include/parasail'
 /usr/bin/install -c -m 644  parasail/cpuid.h parasail/io.h parasail/function_lookup.h parasail/matrix_lookup.h '/storage/lihuilin/parasail/local/parasail/include/parasail'
 /usr/bin/mkdir -p '/storage/lihuilin/parasail/local/parasail/lib/pkgconfig'
 /usr/bin/install -c -m 644 parasail-1.pc '/storage/lihuilin/parasail/local/parasail/lib/pkgconfig'
make[1]: Leaving directory '/storage/lihuilin/parasail'
```
</details>

## usage
1. `cd /storage/lihuilin/parasail/local/parasail/bin`
```
.
├── myseqs.fasta
├── parasail_aligner
└── parasail_stats
```
`myseqs.fasta` is 
```
>sequence1
CYAMYGSSTHLVLTLGDGVDGFTLDTNLGEFILTHPNLRIPPQKAIYSINEGPCDKKSPNGKLRLLYEAFPMAFLMEQAGGKAVNDRGERILDLVPSHIHDKSSIWLGSSGEIDKFLDHIGKSQ
>sequence2
IKFPNGVQKYIKFCQEEDKSTNRPYTSRYIGSLVADFHRNLLKGGIYLYPSTASHPDGKLRLLYECNPMAFLAEQAGGKASDGKERILDIIPETLHQRRSFFVGNDHMVEDVERFIREFPDA
```
2. In terminal, `./parasail_aligner -f myseqs.fasta -O EMBOSS -a sw_trace_scan_16`
```
sequence2           50 PSTASHPDGKLRLLYECNPMAFLAEQAGGKA-SDGKERILDIIPETLHQR      98
                       |.....|:||||||||..|||||.||||||| :|..|||||::|..:|.:
sequence1           53 PCDKKSPNGKLRLLYEAFPMAFLMEQAGGKAVNDRGERILDLVPSHIHDK     102

sequence2           99 RSFFVGNDHMVEDVERFI     116
                       .|.::|:.   .::::|:
sequence1          103 SSIWLGSS---GEIDKFL     117

Length: 68
Identity:        33/68 (48.5%)
Similarity:      47/68 (69.1%)
Gaps:             4/68 ( 5.9%)
Score: 162

```
## `./parasail_aligner -h`
```

usage: parasail_aligner [-a funcname] [-c cutoff] [-x] [-e gap_extend] [-o gap_open] [-m matrix] [-t threads] [-d] [-M match] [-X mismatch] [-k band size (for nw_banded)] [-l AOL] [-s SIM] [-i OS] [-v] [-V] -f file [-q query_file] [-g output_file] [-O output_format {EMBOSS,SAM,SAMH,SSW}] [-b batch_size] [-r memory_budget] [-C] [-A alphabet_aliases]

Defaults:
        funcname: sw_stats_striped_16
          cutoff: 7, must be >= 1, exact match length cutoff
              -x: if present, don't use suffix array filter
      gap_extend: 1, must be >= 0
        gap_open: 10, must be >= 0
          matrix: blosum62
              -d: if present, assume DNA alphabet ACGT
           match: 1, must be >= 0
        mismatch: 0, must be >= 0
      threads: system-specific default, must be >= 1
             AOL: 80, must be 0 <= AOL <= 100, percent alignment length
             SIM: 40, must be 0 <= SIM <= 100, percent exact matches
              OS: 30, must be 0 <= OS <= 100, percent optimal score
                                              over self score
              -v: verbose output, report input parameters and timing
              -V: verbose memory output, report memory use
            file: no default, must be in FASTA format
      query_file: no default, must be in FASTA format
     output_file: parasail.csv
   output_format: no default, must be one of {EMBOSS,SAM,SAMH,SSW}
      batch_size: 0 (calculate based on memory budget),
                  how many alignments before writing output
   memory_budget: 2GB or half available from system query (100.970 GB)
              -C: if present, use case sensitive alignments, matrices, etc.
alphabet_aliases: traceback will treat these pairs of characters as matches,
                  for example, 'TU' for one pair, or multiple pairs as 'XYab'
```