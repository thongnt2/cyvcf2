cyvcf2
======

cyvcf2 is a cython wrapper around [htslib](https://github.com/samtools/htslib) built for fast parsing of VCF files.
It is targetted toward our use-case in [gemini](http://gemini.rtfd.org) but should also be of general utility.

On a file with 189 that takes [cyvcf](https://github.com/arq5x/cyvcf) **21 seconds** to parse and extract all sample information, it takes `cyvcf2` **1.4 seconds**.

Attributes like `variant.gt_ref_depths` return a numpy array directly so they are immediately ready for downstream use.

Example
=======

```Python
from cyvcf2 import VCF

for variant in VCF('some.vcf.gz'):

	variant.gt_types # numpy array
	variant.gt_ref_depths, variant.gt_alt_depths # numpy arrays
	variant.gt_phases, variant.gt_quals # numpy arrays
	variant.gt_bases # numpy array
	variant.CHROM, variant.start, variant.end, variant.ID, \
				variant.REF, variant.ALT, variant.FILTER, variant.QUAL
	variant.INFO.get('DP') # int
	variant.INFO.get('FS') # float
	variant.INFO.get('AC') # float
    a = variant.gt_phred_ll_homref # numpy array
    b = variant.gt_phred_ll_het # numpy array
    c = variant.gt_phred_ll_homalt # numpy array
```

See Also
========

Pysam also [has a cython wrapper to htslib](https://github.com/pysam-developers/pysam/blob/master/pysam/cbcf.pyx) and one block of code here is taken directly from that library. But, the optimizations that we want for gemini are very specific so we have chosen to create a separate project.
