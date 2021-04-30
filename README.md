# DeepAccessTransfer
## Data
The pretrained DeepAccess model used in the paper and executable versions of DeepAccess training and interpretation can be downloaded [here] (https://zenodo.org/record/4495606/).

## Dependencies
* [Conda](https://docs.conda.io/en/latest/) (v4.9.2)
* [bedtools](https://bedtools.readthedocs.io/en/latest/) (v2.29.2)

To run DeepAccess with regions (bedfile format) you must install bedtools and add it to your path. Bedtools binaries are available [here](https://github.com/arq5x/bedtools2/releases).

After installation, you can add bedtools to your path via the terminal or modifying your ~/.bashrc
```
export PATH="/path/to/bedtools:$PATH"
```

## Environment setup
Install and activate a Conda environment with all necessary Python dependencies by:
```
conda env create -f env/deepaccesstf2.yml
source activate deepaccesstf2
```
## Example
We include an example of training and interpreting a new DeepAccess model to predict ASCL1 and CTCF binding events from both genomic regions and genomic sequences and interpret the results:
```
sh run_ASCL1vsCTCF_DeepAccess.sh
```

## Training
To train a DeepAccess model for a new task
```
usage: python DeepAccessTrainTransfer.py [-h] 
                               -l LABELS [LABELS ...] -out OUT [-ref REFFASTA]
                               [-g GENOME] [-beds BEDFILES [BEDFILES ...]]
                               [-fa FASTA] [-fasta_labels FASTA_LABELS]
                               [-f FRAC_RANDOM] [-bg BG]
                               [-nepochs NEPOCHS] [-model MODEL] [-ho HOLDOUT]
                               [-verbose]

optional arguments:
  -h, --help            show this help message and exit
  -l LABELS [LABELS ...], --labels LABELS [LABELS ...]
  -out OUT, --out OUT
  -ref REFFASTA, --refFasta REFFASTA
  -g GENOME, --genome GENOME
                        chrom.sizes file
  -beds BEDFILES [BEDFILES ...], --bedfiles BEDFILES [BEDFILES ...]
  -fa FASTA, --fasta FASTA
  -fasta_labels FASTA_LABELS, --fasta_labels FASTA_LABELS
  -f FRAC_RANDOM, --frac_random FRAC_RANDOM
  -bg BG, --bg BG
  -nepochs NEPOCHS, --nepochs NEPOCHS
  -model MODEL, --model MODEL
  -ho HOLDOUT, --holdout HOLDOUT
                        chromosome to holdout
  -verbose, --verbose   Print training progress
```
### Arguments
| Argument   | Description | Example |
| ---------  | ----------- | -------- |
| -h, --help | show this help message and exit | NA |
| -l --labels | list of labels for each bed file | C1 C2 C3 |
| -out --out  | output folder name | myoutput |
| -ref --ref  | reference fasta; required with bed input | mm10.fa |
| -g --genome | genome chromosome sizes; required with bed input | default/mm10.chrom.sizes |
| -beds --bedfiles | list of bed files; one of beds or fa input required | C1.bed C2.bed C3.bed |
| -fa --fasta | fasta file;  one of beds or fa input required | C1C2C3.fa |
| -fasta_labels --fasta_labels | text file containing tab delimited labels (0 or 1) for each fasta line with one column for each class | C1C2C3.txt |
| -f  --frac_random | for bed file input fraction of random outgroup regions to add to training | 0.1 |
| -bg --bg | fasta file containning background sequences | default/backgrounds.fa |
| -nepochs --nepochs | number of training iterations | 1 |
| -model --model | folder containing base model to begin training | default/DeepAccessMultiMouse |
| -ho --holdout | chromosome name to hold out (only with bed input) | chr19 |
| -verbose --verbose | print training and evaluation progress | NA |

## Interpretation
To run interpretation of a DeepAccess model
```
usage: python DeepAccessInterpret.py [-h] -trainDir TRAINDIR
                           [-fastas FASTAS [FASTAS ...]]
                           [-l LABELS [LABELS ...]]
                           [-comparisons COMPARISONS [COMPARISONS ...]]
                           [-evalMotifs EVALMOTIFS]
                           [-evalPatterns EVALPATTERNS] [-saliency]
                           [-bg BACKGROUND] [-vis]

optional arguments:
  -h, --help            show this help message and exit
  -trainDir TRAINDIR, --trainDir TRAINDIR
  -fastas FASTAS [FASTAS ...], --fastas FASTAS [FASTAS ...]
  -l LABELS [LABELS ...], --labels LABELS [LABELS ...]
  -c COMPARISONS [COMPARISONS ...], --comparisons COMPARISONS [COMPARISONS ...]
  -evalMotifs EVALMOTIFS, --evalMotifs EVALMOTIFS
  -evalPatterns EVALPATTERNS, --evalPatterns EVALPATTERNS
  -saliency, --saliency
  -bg BACKGROUND, --background BACKGROUND
  -vis, --makeVis
```
### Arguments 
| Argument   | Description | Example |
| ---------  | ----------- | -------- |
| -h, --help | show this help message and exit | NA |
| -trainDir --trainDir | directory containing trained DeepAccess model | test/ASCL1vsCTCF |
| -fastas --fastas | list of fasta files to evaulate | test/ASCL1vsCTCF/test.fa |
| -l --labels | list of labels for each bed file | C1 C2 C3 |
| -c --comparisons | list of comparisons between different labels | ASCL1vsCTCF ASCL1vsNone runs differential EPE between ASCL1 and CTCF and EPE on ASCL1; C1,C2vsC3 runs differential EPE for (C1 and C2) vs C3 |
| -evalMotifs --evalMotifs | PWM or PCM data base of DNA sequence motifs | default/HMv11_MOUSE.txt |
| -evalPatterns --evalPatterns | fasta file containing DNA sequence patterns | data/ASCL1_space.fa |
| -bg --bg | fasta file containning background sequences | default/backgrounds.fa |
| -saliency --saliency | calculate per base nucleotide importance | NA |
| -vis --makeVis | to be used with saliency to make plot visualizing results | NA |