## Running diann for peptide matching

This document describes DIA-NN processing of proteomics data.


```shell
#!/bin/bash
##locations
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
dataProcessingLocation="/mnt/d/requests/1172/240329_orbitrapLumos/diannLibGeneration/"
rawFileDirectory="/mnt/d/requests/1172/240329_orbitrapLumos/"


##data processing
eval mkdir ${rawFileDirectory}diannLibGeneration
eval ${diannLocation} --f "${rawFileDirectory}240112_231213_1172_3D2_repA_dia1.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repA_dia2.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repA_dia3.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repB_dia1.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repB_dia2.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repB_dia3.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repD_dia1.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repD_dia2.mzML" --f "${rawFileDirectory}240112_231213_1172_3D2_repD_dia3.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repA_dia1.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repA_dia2.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repA_dia3.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repB_dia1.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repB_dia2.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repB_dia3.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repD_dia1.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repD_dia2.mzML" --f "${rawFileDirectory}240112_231213_1172_Neg_repD_dia3.mzML" --lib "${rawFileDirectory}fragpipeFirstPass/library.tsv" --threads 28 --verbose 1 --out "${rawFileDirectory}diannLibGeneration/report.tsv" --qvalue 0.01 --matrices  --out-lib "${rawFileDirectory}diannLibGeneration/specLib.tsv" --gen-spec-lib --predictor --fasta "${rawFileDirectory}fragpipeFirstPass/2024-04-02-reviewed-UP000005640-spikein.fasta" --fasta-search --min-fr-mz 200 --max-fr-mz 1800 --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --individual-mass-acc --individual-windows --smart-profiling --peak-center --no-ifs-removal
```

Process the files individually against this library.
Averaged recommended settings for this experiment: Mass accuracy = 8ppm, MS1 accuracy = 7ppm, Scan window = 9

```shell
#!/bin/bash
##locations
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
dataProcessingLocation="/mnt/d/requests/1172/240329_orbitrapLumos/diannFirstPass/"
rawFileDirectory="/mnt/d/requests/1172/240329_orbitrapLumos/"

###################################
##run diann
for i in 240112_231213_1172_3D2_rep{A,B,D}_dia{1..3} 240112_231213_1172_Neg_rep{A,B,D}_dia{1..3}
do
    printf "processing diann analysis for mass fraction ${i}.mzML."
    echo
    eval mkdir ${dataProcessingLocation}${i}
    eval $diannLocation --f "${rawFileDirectory}${i}.mzML" --lib "${rawFileDirectory}diannLibGeneration/specLib.tsv" --threads 28 --verbose 1 --out "${dataProcessingLocation}${i}/report.tsv" --qvalue 0.01 --matrices  --fasta "${rawFileDirectory}fragpipeFirstPass/2024-04-02-reviewed-UP000005640-spikein.fasta" --met-excision --cut K*,R* --window 9 --mass-acc 8 --mass-acc-ms1 7 --smart-profiling --peak-center --no-ifs-removal 

##filter the output file
    echo
    printf "filtering the output file for Lib.PG.Q.Value <= 0.01"
    echo
    eval "awk 'BEGIN {FS="\t"}; NR==1; NR > 1{ if($41 <= 0.01) { print }}' ${dataProcessingLocation}${i}/report.tsv > ${dataProcessingLocation}${i}/reportFiltered.tsv"
done
```

Combine the output files.

```shell
#!/bin/bash
##locations
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
dataProcessingLocation="/mnt/d/requests/1172/240329_orbitrapLumos/diannFirstPass/"
rawFileDirectory="/mnt/d/requests/1172/240329_orbitrapLumos/"

##combine the output files
head -n 1 ${dataProcessingLocation}240112_231213_1172_3D2_repA_dia1/report.tsv > ${dataProcessingLocation}combinedFilteredReport.tsv; tail -n +2 -q ${dataProcessingLocation}240112_231213_1172_3D2_repA_dia1/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repA_dia2/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repA_dia3/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repB_dia1/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repB_dia2/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repB_dia3/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repD_dia1/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repD_dia2/report.tsv ${dataProcessingLocation}240112_231213_1172_3D2_repD_dia3/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repA_dia1/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repA_dia2/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repA_dia3/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repB_dia1/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repB_dia2/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repB_dia3/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repD_dia1/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repD_dia2/report.tsv ${dataProcessingLocation}240112_231213_1172_Neg_repD_dia3/report.tsv >> ${dataProcessingLocation}combinedFilteredReport.tsv
```

Remainder of processing occurs in R.




