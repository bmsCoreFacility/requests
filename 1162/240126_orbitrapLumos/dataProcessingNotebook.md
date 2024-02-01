## Data processing workbook

For this project, gel bands were submitted to the BMS core for processing with a goal of protein identification. The protein bands were derived from Atlantic Salmon samples. I am going to use the NCBI database for [Salmo salar](https://www.ncbi.nlm.nih.gov/Taxonomy/Browser/wwwtax.cgi?id=8030). This database contains 110,306 protein sequences. There is a Uniprot for Salmo salar, but the user prefers the NCBI version.

After downloading the database, it seems to have a variety of different accession formats which I think will cause issues with our data analysis routine downstream ([FragPipe](https://github.com/Nesvilab/FragPipe)), so we will need to reformat them. To start, I used Notepad++ to remove any commas or quotes from the file. After this I will try and make a simplified fasta database using R. See the `databaseProcessing.qmd` (opens with RStudio) file to see what I did here. I then used [Philosopher](https://github.com/Nesvilab/philosopher) to create a decoy database.


```shell
/mnt/e/softwareTools/fragpipe-jre-211/tools/philosopher_v5.1.0_windows_amd64/philosopher.exe workspace --init 
/mnt/e/softwareTools/fragpipe-jre-211/tools/philosopher_v5.1.0_windows_amd64/philosopher.exe database --custom ncbiSalmoSalarProteinAccessionCorrected_202401.fasta --contam

##output
bms@DESKTOP-AN2IGM9:/mnt/d/requests/1162/240126_orbitrapLumos/databasePreparation$ /mnt/e/softwareTools/fragpipe-jre-211/tools/philosopher_v5.1.0_windows_amd64/philosopher.exe database --custom ncbiSalmoSalarProteinAccessionCorrected_202401.fasta --contam
INFO[15:55:22] Executing Database  v5.1.0
INFO[15:55:22] Generating the target-decoy database
INFO[15:55:25] Creating file
INFO[15:55:26] Done
```

OK this seems to have worked.



