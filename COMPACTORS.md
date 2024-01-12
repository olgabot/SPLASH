# SPLASH: compactors

## Usage

The software is available as a separate binary named `compactors`. A short help is printed after running the executable without arguments. 

`compactors [options] <fastq_list> <anchors_tsv> <compactors_tsv>`

Positional parameters:
* `fastq_list`        	- input file with a list of FASTQ files to be queried for anchors (one per line)
* `anchors_tsv`        	- input tsv file with anchors
* `compactors_tsv`  		- output tsv with compactors
   
Options:
* `--num_kmers <int>` - number of tiling kmers following an anchor (default: 2)
* `--kmer_len <int>` - length of tiling kmers (default: 27)
* `--epsilon <real>` - sequencing error (default: 0.05)
* `--beta <real>` - beta parameter for active set generation, lower values increase sensitivity (default: 5)
* `--lower_bound <int>` - minimum kmer abundance to add it to an active set, lower values increase sensitivity (default: 10)
* `--max_mismatch <int>` - maximum mismatch count for compactor candidates (default: 4)

* `--no_extension` - disable recursive extension (default: enabled)
* `--max_length <int>` - maximum compactor length in bases (used only with recursion; default: 2000)
* `--min_extender_specificity <real>` - minimum extender specificity for current anchor to allow extensions (default: 0.9)
* `--max_anchor_compactors <int>` - maximum number of compactors that can originate from an anchor (default: 1000)
* `--max_child_compactors <int>` - maximum number of child compactors produced at each extension step (default: 20)
* `--extend_all` - if multiple compactors for a given anchor ends with same extender, all are extended (default: off)

* `--num_threads <int>` - number of threads
* `--out_fasta <name>` - name of the optional compactor FASTA (not generated by default)
* `--reads_buffer_gb <int>` - size of the read buffer in GB (default: 24)
* `--keep_temp` - keep temporary files after the analysis, use previously generated temporary files (if exist) during the analysis (default: off)

The output of the program is a TSV table with compactors ordered increasingly by length and then alphabetically w.r.t anchor and then decreasingly w.r.t support. The table contains the following columns:
* `anchor` - the anchor sequence,
* `compactor` - the compactor sequence,
* `exact_support` - how many times the compactor was observed in reads exactly (if the compactor was extended, it concerns the part of the compactor added in the last extension),
* `support` - how many times the compactor was observed in reads up to max_mismatch errors (if the compactor was extended, it concerns the part of the compactor added in the last extension,
* `extender_specificity` determines how last k-mer in the compactor is specific for the investigated anchor (if specificity > 0.9, an extension is performed),
* `num_extended` - how many times the compactor was extended.

The optional FASTA file can also be created by using `--out_fasta` option. The values of all the parameters from the pseudocode (epsilon, beta, etc.) can be specified by a command line. 