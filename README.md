# dash

dash sketches and computes distances between fasta and fastq data.

# Build
Clone this repository recursively, and use make.

```bash
git clone --recursive https://github.com/dnbaker/dash
cd dash && make dash
```

# Usage

To see all usage options, use `./dash <subcommand>`, for subcommand in `[sketch, dist, hll, setdist]`.
Of most interest is probably the dist command, which can take either genomes or pre-built sketches as arguments.


## dist
For the simplest case of unspaced, unminimized kmers for a set of genomes with `k = 31` and 13 threads:

```
dash dist -k31 -p13 -Odistance_matrix.txt -osize_estimates.txt genome1.fna.gz genome2.fna genome3.fasta <...>
```

The genomes can be omitted as positional arguments if `-F genome_paths.txt` is provided, where `genome_paths.txt` is a file containing a path to a genome per line.
This can avoid system limits on the number of arguments in a shell command.

These can be cached with `-c`, which saves the sketches for later use. These sketch filenames are based on spacing, kmer size, and sketch size, so there is no risk of overwriting each other.

## sketch
The sketch command largely mirrors dist, except that only sketches are computed.

```
dash sketch -k31 -p13 -F genome_paths.txt
```

## setdist
The setdist command is similar to the dist command, but uses a hash set to provide exact distance values, eliminating approximation from the operation.
This comes at significant runtime and memory costs, but is useful for establishing a ground truth.

## hll
The hll command simply estimates the number of unique elements in a set of files. This can be useful for estimating downstream database sizes based on spacing schemes.
