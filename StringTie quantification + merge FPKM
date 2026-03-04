#!/bin/bash
# Complete: StringTie quantification + merge FPKM

BAM_DIR="/Depth"
GTF_FILE="Index_genome/Index_hg38/gene.gtf"
OUT_DIR="/stringtie"
start_id=10804438  # example
end_id=10804443

# StringTie for each BAM
for i in $(seq $start_id $end_id); do
    bam_file="accepted_hits_SRR$i.bam"
    bam_path="$BAM_DIR/$bam_file"
    
    if [[ -f "$bam_path" ]]; then
        output_gtf="$OUT_DIR/output_SRR$i.gtf"
        abundance_tsv="$OUT_DIR/gene_abundance_SRR$i.tsv"
        
        stringtie "$bam_path" -G "$GTF_FILE" -o "$output_gtf" -A "$abundance_tsv"
        echo "Done SRR$i"
    fi
done

# Step 2: Merge FPKM into matrix (Code 1)
cd "$OUT_DIR"
echo -e "GeneID\tSampleID\tFPKM" > merged_gene_abundance.csv
for file in gene_abundance_SRR*.tsv; do
    sample_id=$(echo "$file" | grep -oP 'SRR\d+')
    tail -n +2 "$file" | awk -v sample="$sample_id" '{print $1"\t"sample"\t"$4}' >> merged_gene_abundance.csv
done

echo "Merged matrix: $OUT_DIR/merged_gene_abundance.csv"
