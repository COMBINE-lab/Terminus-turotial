---
title:  "Running Terminus"
date:   2020-02-01 15:04:23
categories: [tutorial]
tags: [Terminus]
---

## A challenge for accurate RNA-seq analysis

RNA-seq analysis is most often carried out at the gene level, which can provide robust estimates of the total
transcriptional output of a set of related transcripts, but which can mask differences in differential isoform usage
between conditions.  Moreover, in the presence of highly-sequence-similar gene families, these estimates may not 
be accurate.  On the other hand, transcript-level analysis provides exquisite biological resolution, but is often beyond
the bounds of robust inference for some fraction of the transcriptome.  In such cases, maximum likelihood estimation (MLE)-based models are prone to return a high-likelihood but likely incorrect solution.  Such ambiguity is inherent in the RNA-seq data and in the inference challenge it poses, and results chiefly from multi-mapping reads induced by sequence similarity between transcripts &mdash; due either to alternative splicing or to sequence-similarity among related genes within larger gene families.


## What is Terminus?

Terminus is a program that implements a new method for analyzing transcript-level abundance estimates from RNA-seq data. Terminus works downstream of salmon, and collapses individual transcripts into groups whose total transcriptional output can be estimated more accurately and robustly.

## Approach of Terminus

Terminus attempts to solve this problem of grouping transcripts by using information drawn from the posterior distribution of transcript abundance estimates (i.e. Gibbs samples), not just the point estimate. The groups computed by terminus represent abundance estimation reported at the resolution that is actually supported by the underlying experimental data. In a typical experiment, this is neither at the gene level nor the transcript level. Some transcripts, even from complex, multi-isoform genes, can have their abundances confidently estimated, while other transcripts cannot. Rather than pre-defining the resolution at which the analysis will be performed, and subjecting the results to unnecessary uncertainty or insufficient biological resolution, terminus allows the determination of transcriptional groups that can be confidently ascertained in a given sample, and represents, in this sense, a data-driven approach to transcriptome analysis.

## How to build terminus

Terminus uses the [cargo](https://github.com/rust-lang/cargo) build system and package manager. To build terminus from source, you will need to have rust (ideally v1.40 or greater) installed. Then, you can build terminus by executing:
```bash
$ cargo build --release
```

## How Terminus works
At this moment Terminus works on the output of [salmon](https://github.com/COMBINE-lab/salmon). Additionally 
salmon should be run with `--numGibbsSamples <number of samples>` so that it writes the posterior gibbs samples. Terminus
works with two subcommands, `group` and `collapse`.
```bash
terminus group -m <float> --tolerance <float> -d <salmon_dir> -o <out_dir>
```
For collapsing,

```bash
terminus collapse -c <float> -d <salmon_dir> -o <out_dir>
```

Note that terminus first writes some auxiliary files in the `<out_dir>` then in the collapsing phase writes down the final
quantification values along with the
