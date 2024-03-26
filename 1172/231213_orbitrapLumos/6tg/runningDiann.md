## Running diann for peptide matching

This project is for Eric Pringle where he is trying to compare between two conditions. 

Do this using a shell script, as below.

```shell
#!/bin/bash
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
baseLibraryLocation="/mnt/d/bmsProjects/diaTesting/"
baseProjectLocation="/mnt/d/requests/1172/231213_orbitrapLumos/project_6tg/"

##run diann for lib generation
printf "processing diann analysis"
echo
eval mkdir ${baseProjectLocation}libGeneration
eval $diannLocation --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf1.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf2.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf3.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf4.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf5.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf6.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf7.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf8.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf9.mzML" --f "${baseLibraryLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf10.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repA_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repA_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repA_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repB_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repB_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repB_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repC_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repC_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_6TG_repC_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repA_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repA_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repA_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repB_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repB_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repB_dia3.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repC_dia1.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repC_dia2.mzML" --f "${baseProjectLocation}240112_231213_1172_DMSO_repC_dia3.mzML" --lib "" --threads 28 --verbose 1 --out "${baseProjectLocation}libGeneration/report.tsv" --qvalue 0.01 --matrices  --out-lib "${baseProjectLocation}libGeneration/generatedSpecLib.tsv" --gen-spec-lib --predictor --fasta "${baseProjectLocation}2024-02-21-reviewed-UP000005640-spikein.fasta" --fasta-search --min-fr-mz 200 --max-fr-mz 1800 --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --individual-mass-acc --individual-windows --rt-profiling --peak-center --no-ifs-removal


##run diann on the individual files
for i in 240112_231213_1172_6TG_rep{A,B,C}_dia{1..3} 240112_231213_1172_DMSO_rep{A,B,C}_dia{1..3}
do
    printf "processing diann analysis for mass fraction ${i}.mzML."
    echo
    eval mkdir ${baseProjectLocation}${i}
    eval $diannLocation --f "${baseProjectLocation}${i}.mzML" --lib "${baseProjectLocation}libGeneration/generatedSpecLib.tsv" --threads 28 --verbose 1 --out "${baseProjectLocation}${i}/report.tsv" --qvalue 0.01  --matrices  --reannotate --fasta "${baseProjectLocation}2024-02-21-reviewed-UP000005640-spikein.fasta" --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --smart-profiling --peak-center --no-ifs-removal
##filter the output file
    echo
    printf "filtering the output file for Lib.PG.Q.Value <= 0.01"
    echo
    eval "awk 'BEGIN {FS="\t"}; NR==1; NR > 1{ if($41 <= 0.01) { print }}' ${baseProjectLocation}${i}/report.tsv > ${baseProjectLocation}${i}/reportFiltered.tsv"
done

##combine the output files
head -n 1 ${baseProjectLocation}240112_231213_1172_6TG_repA_dia1/report.tsv > ${baseProjectLocation}combinedFilteredReport.tsv; tail -n +2 -q ${baseProjectLocation}240112_231213_1172_6TG_repA_dia1/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repA_dia2/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repA_dia3/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repB_dia1/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repB_dia2/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repB_dia3/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repC_dia1/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repC_dia2/report.tsv ${baseProjectLocation}240112_231213_1172_6TG_repC_dia3/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repA_dia1/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repA_dia2/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repA_dia3/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repB_dia1/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repB_dia2/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repB_dia3/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repC_dia1/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repC_dia2/report.tsv ${baseProjectLocation}240112_231213_1172_DMSO_repC_dia3/report.tsv >> ${baseProjectLocation}combinedFilteredReport.tsv
```