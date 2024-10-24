# Setup AWS HPC

## Transfer data to EC2 instance

```bash
ssh CBGP

for Sample in M1012305 M442305 M612305 M732305 M852305 P121005 P241005 P411005 P531005 P651005 T143105 T22905 T313105 T34905 M1022305 M1132305 M212305 M332305 M452305 M622305 M742305 M912305 P131005 P251005 P421005 P541005 T113105 T14905 T233105 T31905 T353105 M112305 M152305 M322305 M1052305 M122305 M242305 M412305 M532305 M652305 M822305 M942305 P211005 P331005 P451005 P621005 T12905 T213105 T24905 T333105 M1112305 M132305 M252305 M422305 M542305 M712305 M832305 M952305 P221005 P341005 P511005 P631005 T133105 T21905 T253105 T33905 M1122305 M142305 M312305 M432305 M552305 M722305 M842305 P111005 P231005 P351005 P521005 P641005 T13905 T223105 T25905 T343105 M1032305 M1142305 M222305 M342305 M512305 M632305 M752305 M922305 P141005 P311005 P431005 P551005 T11905 T153105 T23905 T323105 T35905 M1042305 M1152305 M232305 M352305 M522305 M642305 M812305 M932305 P151005 P321005 P441005 P611005 T123105 T15905 T243105 T32905; do
    RawDataF=$(ls qc_dna/X204SC23065283-Z01-F001/$Sample/F/"$Sample"_F.clean_qc_pair_trim.fq.gz)
    RawDataR=$(ls qc_dna/X204SC23065283-Z01-F001/$Sample/R/"$Sample"_R.clean_qc_pair_trim.fq.gz)
    echo $RawDataF;
    echo $RawDataR;
scp -r -i "Arabella.pem" $RawDataF ubuntu@ec2-13-39-238-13.eu-west-3.compute.amazonaws.com:qc_data
scp -r -i "Arabella.pem" $RawDataR ubuntu@ec2-13-39-238-13.eu-west-3.compute.amazonaws.com:qc_data
done
```

## Build env

```bash
# miniconda
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh
bash ~/miniconda.sh -b -p ~/miniconda
rm miniconda.sh
export PATH=~/miniconda/bin:$PATH

conda create --name assembly
conda create --name mOTUs
conda create --name eggnog

conda config --set auto_activate_base false
conda init bash
# Add channels
conda config --add channels defaults
conda config --add channels conda-forge 
conda config --add channels bioconda
conda install mamba -n base -c conda-forge
```

## Install dependencies

```bash
conda activate mOTUs
mamba install -c bioconda motus #v3.1.0
motus downloadDB

conda activate assembly
mamba install megahit #1.2.9

conda activate eggnog
mamba install -c bioconda eggnog-mapper #2.1.11
export PATH=/home/ubuntu/miniconda/envs/eggnog:/home/ubuntu/miniconda/envs/eggnog/bin:"$PATH"
mkdir eggnog-mapper-data
export EGGNOG_DATA_DIR=/home/ubuntu/eggnog-mapper-data
download_eggnog_data.py
```