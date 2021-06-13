# kite_for_dummies
Reminders to myself because I keep forgetting how to use kite efficiently

### Get ATAC barcode list
```
wget https://teichlab.github.io/scg_lib_structs/data/737K-cratac-v1.txt
```

### Mini processing pipeline
```
do
i="tag_${j}"
(kallisto bus -i Universal_TSA.idx -o "${i}_kb" -x 10xv2 -t 8 "${i}_processed_R1.fastq.gz" "${i}_processed_R2.fastq.gz")2>"${i}_taglog.txt"
(bustools correct -w 737K-cratac-v1.txt "${i}_kb/output.bus" -o "${i}_kb/output_corrected.bus")2>>"${i}_taglog.txt"
(bustools sort -t 8 -o "${i}_kb/output_sorted.bus" "${i}_kb/output_corrected.bus")2>>"${i}_taglog.txt"
bustools count -o "tag_counts/${i}_counts" --genecounts -g Universal_TSA.t2g -e "${i}_kb/matrix.ec" -t "${i}_kb/transcripts.txt" "${i}_kb/output_sorted.bus"
done
```

### Getting t2g file
In the very beginning when you process with kite, you specify this, so you kinda have to start with it from the raw barcode .csv

