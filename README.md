# git-clone-https-github.com-MasoumehJB-circRNA-linux-pipeline
circRNA-linux-pipeline
*Complete circRNA + linear RNA pipeline** from SRA to quantification matrices.

##Quick Start

# Edit circRNA_Pipeline.sh (paths/samples)
# Place required files (table below)
chmod +x *.sh
./circRNA_Pipeline.sh          # Core pipeline
./Depth.sh          # Mapped reads CSV
./StringTie.sh      # Linear RNA FPKM matrix

# Workflow Overview
SRA/FastQ ──(1) SRA-Toolkit──> Raw FASTQ ──(2) FastQC/Trim──>
              
(3) TopHat2-Fusion ──> CIRCexplorer2 ──> circ_fusion.txt
                                            │
HISAT2 ──> samtools ──> sorted.bam ────────┼──> CIRIquant ──> BSJ/FL abundance
                                            │
StringTie ─────────────────────────────────┼──> merged_gene_abundance.csv (linear RNA)
                                            │
prep_CIRIquant ────────────────────────────┘

#Scripts included:
circRNA_Pipeline.sh -> Core circRNA pipeline
Depth.sh -> mapped_reads.csv (alignment QC)
StringTie.sh -> merged_gene_abundance.csv (linear RNA FPKM)

#Required Files

| Path                  | File                                | Source                                           |
| --------------------- | ----------------------------------- | ------------------------------------------------ |
| apps/                 | jdk-21_linux-x64_bin.deb            | Oracle JDK                                       |
| apps/                 | 123Fastq_v1.3_Binary.rar            | FastQ Cleaner                                    |
| apps/                 | Anaconda3-2023.09-0-Linux-x86_64.sh | Anaconda Archive                                 |
| apps/CIRIquant/       | environment.yaml                    | CIRI2 GitHub                                     |
| apps/CIRIquant/       | config.yaml                         | Edit name: SAMPLE field                          |
| apps/tophat2/         | tophat2 binary                      | TopHat2                                          |
| Genome_Index          | genome.*                            | hisat2-build hg38.fa hg38/genome                 |
| indexes/bowtie2/hg38/ | hg38.*                              | bowtie2-build hg38.fa hg38                       |
| indexes/genome/       | hg38.fa                             | GRCh38 hg38                                      |
| GRCh38\Annotation     | genes_hg38                          | genes_hg38.gtf                                   |

#Required Files

# Configuration
SAMPLES=("SRR10804438" "SRR10804440")  # Your SRA accessions
PROJECT_DIR="/path/to/project"
HISAT2_INDEX="$INDEX_DIR/hisat2/hg38/genome"

#Outputs
work/SRR10804438/
├── tophat2/accepted_hits.bam          # Fusion alignments
├── fast_Circ/annotate/circ_fusion.txt # circRNA candidates
├── Hisat2/hisat2_*.bam                # HISAT2 alignments
├── samtools/*.sorted.bam              # Sorted + indexed
└── CIRIquant/                         # BSJ/FL abundances

#Dependencies
sudo apt install sra-toolkit samtools hisat2 bc
conda: CIRCexplorer2, CIRIquant
