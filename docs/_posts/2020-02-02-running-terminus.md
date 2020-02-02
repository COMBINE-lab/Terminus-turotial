---
title:  "Running Terminus"
date:   2020-02-01 15:04:23
categories: [tutorial]
tags: [Terminus]
---

## Uncertainty - the inevitable problem of RNA-seq data analysis !

The structure of transcriptomic mapping contains many inherent ambiguousness that arises from shared exons in 
the isoforms of the same gene, paralogous genes etc. This underlying ambiguity in the sequences directly translates
in the world in domain of quantification, when there are many equally likely candidate transcript to assign a read to 
a read to. Maximum likelihood estimation (MLE) based models are prone to favor one of the candidates, leading to a
wrong solutions. 


## What is Terminus

Terminus is a program for analyzing transcript-level abundance estimates from RNA-seq data, computed using salmon, and collapsing individual transcripts into groups whose total transcriptional output can be estimated more accurately and robustly.


## Approach of Terminus

Terminus attempts to solve this problem by grouping transcripts by drawing useful information from the posterior
samples from the quantification model. The groups computed by terminus represent abundance estimation reported at the resolution that is actually supported by the underlying experimental data. In a typical experiment, this is neither at the gene level nor the transcript level. Some transcripts, even from complex, multi-isoform genes, can have their abundances confidently estimated, while other transcripts cannot. Rather than pre-defining the resolution at which the analysis will be performed, and subjecting the results to unnecessary uncertainty or insufficient biological resolution, terminus allows the determination of transcriptional groups that can be confidently ascertained in a given sample, and represents, in this sense, a data-driven approach to transcriptome analysis.

## How to build terminus

Terminus uses the [cargo](https://github.com/rust-lang/cargo) build system and package manager. To build terminus from source, you will need to have rust (ideally v1.40 or greater) installed. Then, you can build terminus by executing:
```python
$ cargo build --release
```

## How Terminus works
At this moment Terminus works on the output of [salmon](https://github.com/COMBINE-lab/salmon). Additionally 
salmon should be run with `--numGibbsSamples <number of samples>` so that it writes the posterior gibbs samples. Terminus
works with two subcommands, `group` and `collapse`.
```python
terminus group -m <float> --tolerance <float> -d <salmon_dir> -o <out_dir>
```
For collapsing,

```python
terminus collapse -c <float> -d <salmon_dir> -o <out_dir>
```

Note that terminus first writes some auxiliary files in the `<out_dir>` then in the collapsing phase writes down the final
quantification values along with the