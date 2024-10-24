# Metagenomics pipelin in AWS

## MOTUS2

```bash
conda activate mOTUs

for Sample in M442305 M612305 M732305 M852305 P121005 P241005 P411005 P531005 P651005 T143105 T22905 T313105 T34905 M1022305 M1132305 M212305 M332305 M452305 M622305 M742305 M912305 P131005 P251005 P421005 P541005 T113105 T14905 T233105 T31905 T353105 M112305 M152305 M322305 M1052305 M122305 M242305 M412305 M532305 M652305 M822305 M942305 P211005 P331005 P451005 P621005 T12905 T213105 T24905 T333105 M1112305 M132305 M252305 M422305 M542305 M712305 M832305 M952305 P221005 P341005 P511005 P631005 T133105 T21905 T253105 T33905 M1122305 M142305 M312305 M432305 M552305 M722305 M842305 P111005 P231005 P351005 P521005 P641005 T13905 T223105 T25905 T343105 M1032305 M1142305 M222305 M342305 M512305 M632305 M752305 M922305 P141005 P311005 P431005 P551005 T11905 T153105 T23905 T323105 T35905 M1042305 M1152305 M232305 M352305 M522305 M642305 M812305 M932305 P151005 P321005 P441005 P611005 T123105 T15905 T243105 T32905; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=taxonomy/mOTUS/$Sample
sbatch -p onspot-welter -c 8 -C r5.2xlarge scripts/mOTU_profiler.sh $Read_F $Read_R $OutDir 8
done
``` 

## Transfer data to CBGP

```bash
scp -r -i "Arabella.pem" ubuntu@ec2-13-39-238-13.eu-west-3.compute.amazonaws.com:taxonomy analysis/

motus merge -d couns_3_70_T/ -o couns_3_70_T/T.counts_3_70_all_motus_profile.tsv 

python git_repo/mOTUs2phyloseq.py Projects/2023_METAS/analysis/Taxonomy/mOTUs/couns_3_70_T/T.counts_3_70_all_motus_profile.tsv 

less phyloseq_profile_all_motus_profile_count_g3l70.tsv | sed 's/s__\(.*\)\[.*\]/\1/' > T.phyloseq_counts_3_70_all_motus_profile.tsv

less phyloseq_counts_1_70_all_motus_profile.tsv | sed 's/s__//g' > phyloseq_counts_1_70_all_motus_profile.edited2.tsv

less phyloseq_counts_3_70_all_motus_profile.tsv | sed 's/s__\(.*\)\[.*\]/\1/' > phyloseq_counts_3_70_all_motus_profile.edited.tsv
less phyloseq_counts_3_70_all_motus_profile.tsv | sed 's/s__//g' > phyloseq_counts_3_70_all_motus_profile.edited2.tsv
```

## Megahit

```bash
conda activate assembly

### test for completeness

for Sample in M1012305; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 8 -C r5.2xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 8
done

for Sample in M442305; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 16 -C r5.4xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 16
done
```

```bash
for Sample in M1012305 M442305 M732305 M852305 P121005 P241005 P411005 P531005 P651005 T143105 T22905 T313105 T34905 M1022305 M1132305 M212305 M332305 M452305 M622305 M742305 M912305 P131005 P251005 P421005 P541005 T113105 T14905 T233105 T31905 T353105 M112305 M152305 M322305 M1052305 M122305 M242305 M412305 M532305 M652305 M822305 M942305 P211005 P331005 P451005 P621005 T12905 T213105 T24905 T333105 M1112305 M132305 M252305 M422305 M542305 M712305 M832305 M952305 P221005 P341005 P511005 P631005 T133105 T21905 T253105 T33905; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 32 -C r5.8xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 32
done

#to do
for Sample in M1122305 M142305 M312305 M432305 M552305 M722305 M842305 P111005 P231005 P351005 P521005 P641005 T13905 T223105 T25905 T343105 M1032305 M1142305 M222305 M342305 M512305 M632305 M752305 M922305 P141005 P311005 P431005 P551005 T11905 T153105 T23905 T323105 T35905 M1042305; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 32 -C r5.8xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 32
done
######


for Sample in M732305 M852305 P121005 P241005 P411005 P531005 P651005 T143105 T22905 T313105 T34905 M1022305 M1132305; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 16 -C r5.4xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 16
done

for Sample in M212305 M332305 M452305 M622305 M742305 M912305 P131005 P251005 P421005 P541005 T113105 T14905 T233105 T31905 T353105 M112305; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 32 -C r5.8xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 32
done

for Sample in M152305; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-heavy -c 96 -C r5.24xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 96
done

for Sample in M322305 M1052305 M122305 M242305 M412305 M532305 M652305 M822305 M942305 P211005 P331005 P451005 P621005 T12905 T213105 T24905; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 16 -C r5.4xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 16
done

for Sample in T333105 M1112305 M132305 M252305 M422305 M542305 M712305 M832305 M952305 P221005 P341005 P511005 P631005 T133105 T21905 T253105 T33905; do
Read_F=$(ls qc_data/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_data/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
mkdir -p $OutDir
sbatch -p onspot-welter -c 8 -C r5.2xlarge scripts/megahit.sh $Read_F $Read_R $OutDir 8
done
```

## Eggnog

```bash
conda activate eggnog

for Sample in M1012305; do
Assembly=$(ls assembly/megahit/$Sample/megahit_out/"$Sample".contigs.fa)
echo $Assembly;
OutDir=analysis/eggnog/$Sample
sbatch -p onspot-welter -c 8 -C r5.2xlarge scripts/eggnog-mapper.AWS.sh $Assembly $OutDir
done
```

## Transfer data to CBGP

```bash
for Sample in M1012305 M112305 M152305 M332305 M532305 M712305 M832305 M952305 P221005 P421005 P621005 T133105 T233105 T31905 T353105 M1022305 M1132305 M242305 M412305 M542305 M732305 M852305 P121005  P241005 P451005 P631005 T14905 T24905 T333105 M1052305 M122305 M252305 M422305 M612305 M742305 M912305 P131005  P331005  P511005 T113105 T213105 T253105 T33905 M1112305 M132305 M322305 M452305 M652305 M822305 M942305 P211005  P341005  P541005 T12905 T21905 T313105 T34905; do
mkdir -p datafromAWS/assembly/megahit/$Sample
scp -r -i "Arabella.pem" ubuntu@ec2-13-39-238-13.eu-west-3.compute.amazonaws.com:assembly/megahit/$Sample/megahit_out/"$Sample".contigs.fa datafromAWS/assembly/megahit/$Sample
scp -r -i "Arabella.pem" ubuntu@ec2-13-39-238-13.eu-west-3.compute.amazonaws.com:assembly/megahit/$Sample/megahit_out/"$Sample".log datafromAWS/assembly/megahit/$Sample
done
```

## Paralell jobs at CBGP

## Megahit

```bash
for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305 P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005 T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do
Read_F=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/F/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls qc_dna/X204SC23065283-Z01-F001/"$Sample"/R/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
OutDir=assembly/megahit/$Sample
sbatch scripts/megahit.CBGP.sh $Read_F $Read_R $OutDir 8
done
# 9 to 17 hours runs
```

## Eggnog

```bash
source /data/jhc/soft/miniconda3/bin/activate
conda activate eggnog-mapper

for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305 P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005 T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do
Assembly=$(ls assembly/megahit/$Sample/megahit_out/"$Sample".contigs.fa)
echo $Assembly;
OutDir=analysis/eggnog/$Sample
sbatch scripts/eggnog-mapper.CBGP.sh $Assembly $OutDir
done
# 10 to 23 hours run

# Rename output if needed
for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305 P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005 T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do
echo $Sample
mv analysis/eggnog/$Sample/megahit.emapper.annotations analysis/eggnog/$Sample/"$Sample".emapper.annotations
mv analysis/eggnog/$Sample/megahit.emapper.annotations.xlsx analysis/eggnog/$Sample/"$Sample".emapper.annotations.xlsx
mv analysis/eggnog/$Sample/megahit.emapper.genepred.fasta analysis/eggnog/$Sample/"$Sample".emapper.genepred.fasta
mv analysis/eggnog/$Sample/megahit.emapper.genepred.gff analysis/eggnog/$Sample/"$Sample".emapper.genepred.gff
mv analysis/eggnog/$Sample/megahit.emapper.hits analysis/eggnog/$Sample/"$Sample".emapper.hits
mv analysis/eggnog/$Sample/megahit.emapper.seed_orthologs analysis/eggnog/$Sample/"$Sample".emapper.seed_orthologs
mv assembly/megahit/$Sample/megahit_out/* assembly/megahit/$Sample/
done
```

## Bedtools. Extract ORF sequences

```bash
for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 M142305 M152305 M212305 M222305 M232305 M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305 P111005 P121005 P131005 P141005 P151005 P211005 P221005 P231005 P241005 P251005 P311005 P321005 P331005 P341005 P351005 P411005 P421005 P431005 P441005 P451005 P511005 P521005 P531005 P541005 P551005 P611005 P621005 P631005 P641005 P651005 T113105 T11905 T123105 T12905 T133105 T13905 T143105 T14905 T153105 T15905 T213105 T21905 T223105 T22905 T233105 T23905 T243105 T24905 T253105 T25905 T313105 T31905 T323105 T32905 T333105 T33905 T343105 T34905 T353105 T35905; do
echo $Sample
Assembly=$(ls assembly/megahit/$Sample/"$Sample".contigs.fa)
echo $Assembly;
GFF=analysis/eggnog/$Sample/"$Sample".emapper.genepred.gff
echo $GFF
OutDir=analysis/ORF/nucleotide_sequence/$Sample
sbatch bedtools.sh $Assembly $GFF $OutDir
done
# 20 to 30 min to run
```











```bash
for Sample in M1012305 M1022305 M1032305 M1042305 M1052305 M1112305 M1122305 M112305 M1132305 M1142305 M1152305 M122305 M132305 ; do


for Sample in M142305 M152305 M212305 M222305 M232305 

M242305 M252305 M312305 M322305 M332305 M342305 M352305 M412305 M422305 M432305 M442305 M452305 M512305 M522305 M532305 M542305 M552305 M612305 M622305 M632305 M642305 M652305 M712305 M722305 M732305 M742305 M752305 M812305 M822305 M832305 M842305 M852305 M912305 M922305 M932305 M942305 M952305; do
Read_F=$(ls Projects/2023_METAS/qc_dna/X204SC23065283-Z01-F001/$Sample/F/"$Sample"_F.clean_qc_pair_trim.fq.gz)
Read_R=$(ls Projects/2023_METAS/qc_dna/X204SC23065283-Z01-F001/$Sample/R/"$Sample"_R.clean_qc_pair_trim.fq.gz)
echo $Read_F;
echo $Read_R;
Database=PlusPFP
OutDir=Projects/2023_METAS/analysis/Taxonomy/Kraken2/PlusPFP/confidence50/$Sample
ProgDir=git_repo/CBGP
sbatch $ProgDir/Kraken2.sh $Read_F $Read_R $Database $OutDir
done
```