## Running diann for peptide matching

This project is for Eric Pringle where he is trying to compare between two conditions. 

Do this using a shell script, as below.

```shell
#!/bin/bash
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
baseLibraryLocation="/mnt/d/bmsProjects/diaTesting/"
baseProjectLocation="/mnt/d/requests/231213_1172/project_6tg/"

##run diann
printf "processing diann analysis"
echo
eval mkdir ${baseProjectLocation}libGeneration
eval $diannLocation --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf1.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf2.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf3.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf4.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf5.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf6.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf7.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf8.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf9.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf10.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repA_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repA_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repA_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repB_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repB_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repB_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repC_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repC_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repC_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repA_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repA_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repA_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repB_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repB_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repB_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repC_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repC_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repC_dia3.mzML" --lib "" --threads 28 --verbose 1 --out "${baseProjectLocation}libGeneration/report.tsv" --qvalue 0.01 --matrices  --out-lib "${baseProjectLocation}libGeneration/generatedSpecLib.tsv" --gen-spec-lib --predictor --fasta "${baseProjectLocation}uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta" --fasta "${baseProjectLocation}crapDatabaseReannotated.fasta" --fasta-search --min-fr-mz 200 --max-fr-mz 1800 --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --individual-mass-acc --individual-windows --rt-profiling --peak-center --no-ifs-removal
```

Library construction went well. Output is below.

```shell

```

Search the mass fractionated DIA runs against the library we just constructed and see how the output data looks. I think it is a good idea to search this as one mass range at a time that we can combine into a single set later on in the analysis portion handled in R. Use a shell script to process through the individual files.

```shell
#!/bin/bash

##define the location of tools you will need
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
baseLibraryLocation="/mnt/d/bmsProjects/diaTesting/"
baseProjectLocation="/mnt/d/requests/231213_1172/project_6tg/"

##run diann
for i in 240112_231213_1172_6TG_rep{A,B,C}_dia{1..3} 240112_231213_1172_DMSO_rep{A,B,C}_dia{1..3}
do
    printf "processing diann analysis for mass fraction ${i}.mzML."
    echo
    eval mkdir ${baseProjectLocation}${i}
    eval $diannLocation --f "${baseProjectLocation}${i}.mzML" --lib "${baseProjectLocation}libGeneration/generatedSpecLib.tsv" --threads 28 --verbose 1 --out "${baseProjectLocation}${i}/report.tsv" --qvalue 0.01  --matrices  --reannotate --fasta "${baseProjectLocation}uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta" --fasta "${baseProjectLocation}crapDatabaseReannotated.fasta" --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --smart-profiling --peak-center --no-ifs-removal
##filter the output file
    echo
    printf "filtering the output file for Lib.PG.Q.Value <= 0.01"
    echo
    eval "awk 'BEGIN {FS="\t"}; NR==1; NR > 1{ if($41 <= 0.01) { print }}' ${baseProjectLocation}${i}/report.tsv > ${baseProjectLocation}${i}/reportFiltered.tsv"
done
```

Output of the command above is below.

```shell

```

Combine the reports output by the tool so that we can use this file as an input for the iq R package. 

```shell
##combine the output files
head -n 1 /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repA_dia1/report.tsv > /mnt/d/requests/231213_1172/project_6tg/combinedFilteredReport.tsv; tail -n +2 -q /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repA_dia1/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repA_dia2/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repA_dia3/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repB_dia1/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repB_dia2/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repB_dia3/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repC_dia1/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repC_dia2/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_6TG_repC_dia3/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repA_dia1/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repA_dia2/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repA_dia3/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repB_dia1/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repB_dia2/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repB_dia3/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repC_dia1/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repC_dia2/report.tsv /mnt/d/requests/231213_1172/project_6tg/240112_231213_1172_DMSO_repC_dia3/report.tsv >> /mnt/d/requests/231213_1172/project_6tg/combinedFilteredReport.tsv

```