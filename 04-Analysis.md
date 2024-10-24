# Analysis and report

## Create database

```bash
#Metadata for all bacterial and archaea genomes including GTDB and NCBI taxonomies, completeness and contamination estimates, assembly statistics, and genomic properties.
wget https://data.gtdb.ecogenomic.org/releases/latest/ar53_metadata.tar.gz
wget https://data.gtdb.ecogenomic.org/releases/latest/bac120_metadata.tar.gz

#Nucleotide and aminoacids FASTA files for all protein-coding gene sequences predicted with Prodigal.
wget https://data.gtdb.ecogenomic.org/releases/latest/genomic_files_all/ar53_marker_genes_all.tar.gz
wget https://data.gtdb.ecogenomic.org/releases/latest/genomic_files_all/bac120_marker_genes_all.tar.gz
wget https://data.ace.uq.edu.au/public/gtdb/data/releases/latest/genomic_files_reps/gtdb_proteins_aa_reps.tar.gz
wget https://data.ace.uq.edu.au/public/gtdb/data/releases/latest/genomic_files_reps/gtdb_proteins_nt_reps.tar.gz
```

## Remove eukaryote hits

```bash
for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305 P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005 T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do
echo $Sample
cat "$Sample".emapper.annotations | grep -v '@2759'> "$Sample".emapper.annotations_wo_euk
done
```

## CoverM Coverage calculator

```bash
for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305 P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005 T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do


for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305; do
echo $Sample
Read_F=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/F/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/R/"$Sample"_R.clean_qc_pair_trim.fq.gz)
Assembly=assembly/megahit/METAS/$Sample/"$Sample".contigs.fa
OutDir=assembly/megahit/METAS/$Sample/CoverM
mkdir -p $OutDir
sbatch -p bigmem git_repo/coverm.CBGP.sh $Read_F $Read_R $Assembly $OutDir
done

for Sample in M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305; do
echo $Sample
Read_F=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/F/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/R/"$Sample"_R.clean_qc_pair_trim.fq.gz)
Assembly=assembly/megahit/METAS/$Sample/"$Sample".contigs.fa
OutDir=assembly/megahit/METAS/$Sample/CoverM
mkdir -p $OutDir
sbatch -p medium git_repo/coverm.CBGP.sh $Read_F $Read_R $Assembly $OutDir
done

for Sample in P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005; do
echo $Sample
Read_F=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/F/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/R/"$Sample"_R.clean_qc_pair_trim.fq.gz)
Assembly=assembly/megahit/METAS/$Sample/"$Sample".contigs.fa
OutDir=assembly/megahit/METAS/$Sample/CoverM
mkdir -p $OutDir
sbatch -p medium git_repo/coverm.CBGP.sh $Read_F $Read_R $Assembly $OutDir
done

for Sample in T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do
echo $Sample
Read_F=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/F/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/R/"$Sample"_R.clean_qc_pair_trim.fq.gz)
Assembly=assembly/megahit/METAS/$Sample/"$Sample".contigs.fa
OutDir=assembly/megahit/METAS/$Sample/CoverM
mkdir -p $OutDir
sbatch -p bigmem git_repo/coverm.CBGP.sh $Read_F $Read_R $Assembly $OutDir
done


# 6 to 8 hours run

for Sample in M1022305; do
echo $Sample
ORFs=$(ls analysis/ORF/nucleotide_sequence/$Sample/megahit_nt_ORF.fa)
OutDir=analysis/ORF/nucleotide_sequence/$Sample
mkdir -p $OutDir
ProgDir=scripts/
sbatch scripts/MMseqs.CBGP.sh $ORFs $OutDir
done

OutDir=Projects/2023_METAS/analysis/eggnog-profiler
mkdir -p $OutDir
sbatch -p long Projects/2023_METAS/analysis/eggnog_outputs sample1.txt $OutDir

mv Projects/2023_METAS/analysis/eggnog_outputs/M1012305_coverage_values.txt Projects/2023_METAS/analysis/eggnog_outputs/M1012305_coverage_values


for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305 P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005 T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do
echo $Sample
mv Projects/2023_METAS/analysis/eggnog_outputs/"$Sample"_coverage_values.txt Projects/2023_METAS/analysis/eggnog_outputs/"$Sample"_coverage_values
done

python /home/sbodi/TFM-main/emapper-profiler/emapper_profiler.py --inputdir Projects/2023_METAS/analysis/eggnog_outputs -s sample1.txt -k /home/sbodi/TFM-main/emapper-profiler -e -o Projects/2023_METAS/analysis/eggnog-profiler/tpm -u rpkm
python /home/sbodi/TFM-main/emapper-profiler/emapper_profiler.py --inputdir Projects/2023_METAS/analysis/eggnog_outputs -s sample2.txt -k /home/sbodi/TFM-main/emapper-profiler -e -o Projects/2023_METAS/analysis/eggnog-profiler/Patata -u rpkm
python /home/sbodi/TFM-main/emapper-profiler/emapper_profiler.py --inputdir Projects/2023_METAS/analysis/eggnog_outputs -s sample3.txt -k /home/sbodi/TFM-main/emapper-profiler -e -o Projects/2023_METAS/analysis/eggnog-profiler/Tomate -u rpkm
```