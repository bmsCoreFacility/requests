## Data processing workbook

For this project, gel bands were submitted to the BMS core for processing with a goal of protein identification. The protein bands were derived from Atlantic Mackerel samples. Unfortunately, no sequence database exists for Atlantic Mackerel, so we are going to use the one for [Scomber japonicus](https://www.ncbi.nlm.nih.gov/datasets/taxonomy/13676/). This database contains 31,016 protein sequences. There is a [UniProt](https://www.uniprot.org/taxonomy/13676) page for Scomber japonicus, but it seems to only have 136 proteins that appear to be repetitive entries of mitochondrial proteins. I think the NCBI database is a good starting point.

After downloading the database, it seems to have a variety of different accession formats which I think will cause issues with our data analysis routine downstream ([FragPipe](https://github.com/Nesvilab/FragPipe)), so we will need to reformat them. To start, I used Notepad++ to remove any commas or quotes from the file. I then used [Philosopher](https://github.com/Nesvilab/philosopher) to create a decoy database.

```shell
cd /mnt/d/requests/231108_1162/databasePreparation
/mnt/e/softwareTools/FragPipe-jre-20.0/fragpipe/tools/philosopher_v5.0.0_windows_amd64/philosopher.exe workspace --init
/mnt/e/softwareTools/FragPipe-jre-20.0/fragpipe/tools/philosopher_v5.0.0_windows_amd64/philosopher.exe database --custom ncbiScomberJaponicus_20231219.fasta --contam

##output
/mnt/e/softwareTools/FragPipe-jre-20.0/fragpipe/tools/philosopher_v5.0.0_windows_amd64/philosopher.exe database --custom ncbiScomberJaponicus_20231219.fasta --contam
INFO[12:00:32] Executing Database  v5.0.0
INFO[12:00:32] Generating the target-decoy database
INFO[12:00:33] Creating file
panic: runtime error: index out of range [2] with length 1

goroutine 59 [running]:
github.com/Nesvilab/philosopher/lib/dat.getProteinName({0xc0000e6300?, 0x53?}, 0xe8?, 0x0)
        D:/Projects/philosopher/philosopher/lib/dat/db.go:235 +0x334
github.com/Nesvilab/philosopher/lib/dat.ProcessHeader({0xc0000e6300, 0x53}, {0xc001fa9420, 0xd9}, 0x5, {0x8f3f01, 0
x4}, 0x0?)
        D:/Projects/philosopher/philosopher/lib/dat/db.go:50 +0x1fd
github.com/Nesvilab/philosopher/lib/dat.(*Base).ProcessDBAndSerialize.func2(0x1)
        D:/Projects/philosopher/philosopher/lib/dat/dat.go:181 +0x60b
created by github.com/Nesvilab/philosopher/lib/dat.(*Base).ProcessDBAndSerialize
        D:/Projects/philosopher/philosopher/lib/dat/dat.go:152 +0x190
```

OK, this didn't work. Not sure what the issue is. There is some nice discussion [here](https://github.com/Nesvilab/philosopher/issues/451) about potential issues with fasta headers in custom databases. I am not entirely sure if our issue is with the accession numbers or if there are duplicate headers. I think what I will do is make a reference table and a simplified fasta database using R. See the `databaseProcessing.qmd` (opens with RStudio) file to see what I did here. Try again with Philosopher. I upgraded FragPipe and Philosopher before I did this, but I don't think it mattered.

```shell
/mnt/e/softwareTools/fragpipe-jre-211/tools/philosopher_v5.1.0_windows_amd64/philosopher.exe database --custom ncbiScomberJaponicusAccessionCorrected_20231219.fasta --contam

##output
/mnt/e/softwareTools/fragpipe-jre-211/tools/philosopher_v5.1.0_windows_amd64/philosopher.exe database --custom ncbiScomberJaponicusAccessionCorrected_20231219.fasta --contam
INFO[12:41:14] Executing Database  v5.1.0
INFO[12:41:14] Generating the target-decoy database
INFO[12:41:15] Creating file
INFO[12:41:15] Done
```

OK this seems to have worked. Now we can try our database search. I am going to use the FragPipe GUI for this. You could use the [command line](https://fragpipe.nesvilab.org/docs/tutorial_headless.html) via something like:

```shell
/mnt/e/softwareTools/FragPipe-jre-20.0/fragpipe/bin/fragpipe --headless --workflow /mnt/d/requests/231108_1162/fragpipePass1/fragpipe.workflow --manifest /mnt/d/requests/231108_1162/fragpipePass1/fragpipe-files.fp-manifest --workdir /mnt/d/requests/231108_1162/fragpipePass1
```



