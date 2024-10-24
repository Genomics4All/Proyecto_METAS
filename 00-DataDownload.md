# SEQdata QC

## External sequencing data transfer

### 01/08/2023

```info
Date: 2023-08-01
http://data-deliver.novogene.com/batchfiles/X204SC23065283-Z01-F001
Sequencing project: H204SC23065283 - NVUK2023050826-EU-ES-Genomics4All-145-Shotgun-10Gb-WOBI  - X204SC23065283-Z01-F001
BatchNo.: X204SC23065283-Z01-F001
Sample quantity: 115
Data size (GB): 765.03
```


```bash
# Copy csv file to HPC
scp -r -P 33322 X204SC23065283-Z01-F001.csv agomez@138.4.139.7:/home/agomez/raw_data
# Create folder
ssh CBGP
cd /home/agomez
mkdir -p raw_data/X204SC23065283-Z01-F001
cd !$
# Screen
screen -S raw_data/X204SC23065283-Z01-F001
srun --partition long --mem-per-cpu 10G --cpus-per-task 10 --time=83:00:00 --pty bash
# Download
wget -i ./X204SC23065283-Z01-F001.csv

# Checksums MD5 file
md5sum --check ./MD5.txt > md5sum_check 2>&1


# Check file size 
stat --format="%s" ./X204SC23065283-Z01-F001_01.tar > size_check1 2>&1
stat --format="%s" ./X204SC23065283-Z01-F001_02.tar > size_check2 2>&1
stat --format="%s" ./X204SC23065283-Z01-F001_03.tar > size_check3 2>&1
stat --format="%s" ./X204SC23065283-Z01-F001_04.tar > size_check4 2>&1
stat --format="%s" ./X204SC23065283-Z01-F001_05.tar > size_check5 2>&1
stat --format="%s" ./X204SC23065283-Z01-F001_06.tar > size_check6 2>&1
unzip -l ./X204SC23065283-Z01-F001_01.tar
unzip -l ./X204SC23065283-Z01-F001_02.tar
unzip -l ./X204SC23065283-Z01-F001_03.tar
unzip -l ./X204SC23065283-Z01-F001_04.tar
unzip -l ./X204SC23065283-Z01-F001_05.tar
unzip -l ./X204SC23065283-Z01-F001_06.tar

# Extract the files
tar -xvf ./X204SC23065283-Z01-F001_01.tar
tar -xvf ./X204SC23065283-Z01-F001_02.tar
tar -xvf ./X204SC23065283-Z01-F001_03.tar
tar -xvf ./X204SC23065283-Z01-F001_04.tar
tar -xvf ./X204SC23065283-Z01-F001_05.tar
tar -xvf ./X204SC23065283-Z01-F001_06.tar

# Checksums embedded MD5 file
cd X204SC23065283-Z01-F001_01/
md5sum --check MD5.txt > md5sum_check_extracted_01 2>&1
cd X204SC23065283-Z01-F001_02/
md5sum --check MD5.txt > md5sum_check_extracted_02 2>&1
cd X204SC23065283-Z01-F001_03/
md5sum --check MD5.txt > md5sum_check_extracted_03 2>&1
cd X204SC23065283-Z01-F001_04/
md5sum --check MD5.txt > md5sum_check_extracted_04 2>&1
cd X204SC23065283-Z01-F001_05/
md5sum --check MD5.txt > md5sum_check_extracted_05 2>&1
cd X204SC23065283-Z01-F001_06/
md5sum --check MD5.txt > md5sum_check_extracted_06 2>&1
```