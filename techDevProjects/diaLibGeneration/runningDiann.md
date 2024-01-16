## Running diann for lib generation

The goal of this project is to optimize library generation from DIA-NN such that we can use a standard library to help our analysis. I ran our MB231 standard cell lysate using a GPF setup covering a range of 430-930mz across a total of 10 files (50mz for each file). Now I want to process these into a spectral library using DIA-NN. To do this, I will call it from Linux. 

```shell
##execute diann
./mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8 --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf1.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf2.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf3.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf4.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf5.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf6.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf7.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf8.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf9.mzML" --f "/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf10.mzML" --lib "" --threads 28 --verbose 1 --out "/mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.tsv" --qvalue 0.01 --matrices  --out-lib "/mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv" --gen-spec-lib --predictor --fasta "/mnt/d/bmsProjects/diaTesting/uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta" --fasta-search --min-fr-mz 200 --max-fr-mz 1800 --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --var-mods 1 --var-mod UniMod:35,15.994915,M --var-mod UniMod:1,42.010565,*n --monitor-mod UniMod:1 --individual-mass-acc --individual-windows --rt-profiling --peak-center --no-ifs-removal

```

Do this using a shell script, as below.

```shell
#!/bin/bash
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
baseProjectLocation="/mnt/d/bmsProjects/diaTesting/"

##run diann
printf "processing diann analysis"
eval $diannLocation --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf1.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf2.mzML" --f "/${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf3.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf4.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf5.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf6.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf7.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf8.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf9.mzML" --f "${baseProjectLocation}231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf10.mzML" --lib "" --threads 28 --verbose 1 --out "${baseProjectLocation}libGeneration/libGenerate.tsv" --qvalue 0.01 --matrices  --out-lib "${baseProjectLocation}libGeneration/gpfMb231HumanSpecLib.tsv" --gen-spec-lib --predictor --fasta "${baseProjectLocation}uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta" --fasta-search --min-fr-mz 200 --max-fr-mz 1800 --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --var-mods 1 --var-mod UniMod:35,15.994915,M --var-mod UniMod:1,42.010565,*n --monitor-mod UniMod:1 --individual-mass-acc --individual-windows --rt-profiling --peak-center --no-ifs-removal

```

Library construction went well. Output is below.

```shell
bms@DESKTOP-AN2IGM9:/mnt/d/bmsProjects/diaTesting$ ./diannLibGeneration.sh |& tee diannLibGenerationLog.txt
processing diann analysisDIA-NN 1.8.2 beta 8 (Data-Independent Acquisition by Neural Networks)
Compiled on Dec  1 2022 14:47:06
Current date and time: Mon Jan 15 17:30:32 2024
Logical CPU cores: 56
Thread number set to 28
Output will be filtered at 0.01 FDR
Precursor/protein x samples expression level matrices will be saved along with the main report
A spectral library will be generated
Deep learning will be used to generate a new in silico spectral library from peptides provided
Library-free search enabled
Min fragment m/z set to 200
Max fragment m/z set to 1800
N-terminal methionine excision enabled
In silico digest will involve cuts at K*,R*
Maximum number of missed cleavages set to 1
Min peptide length set to 7
Max peptide length set to 30
Min precursor m/z set to 430
Max precursor m/z set to 930
Min precursor charge set to 2
Max precursor charge set to 4
Cysteine carbamidomethylation enabled as a fixed modification
Maximum number of variable modifications set to 1
Modification UniMod:35 with mass delta 15.9949 at M will be considered as variable
Modification UniMod:1 with mass delta 42.0106 at *n will be considered as variable
Mass accuracy will be determined separately for different runs
Scan windows will be inferred separately for different runs
The spectral library (if generated) will retain the original spectra but will include empirically-aligned RTs
Fixed-width center of each elution peak will be used for quantification
Interference removal from fragment elution curves disabled
DIA-NN will optimise the mass accuracy separately for each run in the experiment. This is useful primarily for quick initial analyses, when it is not yet known which mass accuracy setting works best for a particular acquisition scheme.
Exclusion of fragments shared between heavy and light peptides from quantification is not supported in FASTA digest mode - disabled; to enable, generate an in silico predicted spectral library and analyse with this library
The following variable modifications will be scored: UniMod:1

10 files will be processed
[0:00] Loading FASTA /mnt/d/bmsProjects/diaTesting/uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta
[0:05] Processing FASTA
[0:13] Assembling elution groups
[0:18] 2784234 precursors generated
[0:18] Gene names missing for some isoforms
[0:18] Library contains 20387 proteins, and 20185 genes
[0:23] Encoding peptides for spectra and RTs prediction
[0:30] Predicting spectra and IMs
[6:08] Predicting RTs
[7:05] Decoding predicted spectra and IMs
[9:06] Decoding RTs
[9:08] Saving the library to /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.predicted.speclib
[10:12] Initialising library

[10:14] File #1/10
[10:14] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf1.mzML
[11:54] 420613 library precursors are potentially detectable
[11:54] Processing...
[12:02] RT window set to 3.43531
[12:02] Peak width: 4.436
[12:02] Scan window radius set to 9
[12:03] Recommended MS1 mass accuracy setting: 8.2986 ppm
[12:12] Optimised mass accuracy: 4.10346 ppm
[12:25] Removing low confidence identifications
[12:25] Searching PTM decoys
[12:25] Removing interfering precursors
[12:30] Training neural networks: 80325 targets, 139385 decoys
[12:40] Number of IDs at 0.01 FDR: 20500
[12:40] Calculating protein q-values
[12:40] Number of genes identified at 1% FDR: 6299 (precursor-level), 5951 (protein-level) (inference performed using proteotypic peptides only)
[12:40] Quantification
[12:40] Precursors with monitored PTMs at 1% FDR: 125 out of 126
[12:40] Unmodified precursors with monitored PTM sites at 1% FDR: 71 out of 76
[12:42] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf1.mzML.quant.

[12:42] File #2/10
[12:42] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf2.mzML
[14:24] 393493 library precursors are potentially detectable
[14:24] Processing...
[14:31] RT window set to 3.79747
[14:31] Peak width: 4.436
[14:31] Scan window radius set to 9
[14:31] Recommended MS1 mass accuracy setting: 9.48197 ppm
[14:41] Optimised mass accuracy: 6.19007 ppm
[14:54] Removing low confidence identifications
[14:54] Searching PTM decoys
[14:54] Removing interfering precursors
[14:58] Training neural networks: 91938 targets, 141162 decoys
[15:09] Number of IDs at 0.01 FDR: 22770
[15:09] Calculating protein q-values
[15:09] Number of genes identified at 1% FDR: 6666 (precursor-level), 6482 (protein-level) (inference performed using proteotypic peptides only)
[15:09] Quantification
[15:10] Precursors with monitored PTMs at 1% FDR: 167 out of 170
[15:10] Unmodified precursors with monitored PTM sites at 1% FDR: 79 out of 86
[15:11] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf2.mzML.quant.

[15:11] File #3/10
[15:11] Loading run //mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf3.mzML
[16:36] 362882 library precursors are potentially detectable
[16:36] Processing...
[16:43] RT window set to 3.72173
[16:43] Peak width: 4.436
[16:43] Scan window radius set to 9
[16:43] Recommended MS1 mass accuracy setting: 11.2177 ppm
[16:51] Optimised mass accuracy: 5.32639 ppm
[17:02] Removing low confidence identifications
[17:02] Searching PTM decoys
[17:02] Removing interfering precursors
[17:05] Training neural networks: 76217 targets, 93909 decoys
[17:13] Number of IDs at 0.01 FDR: 21703
[17:13] Calculating protein q-values
[17:13] Number of genes identified at 1% FDR: 6598 (precursor-level), 6390 (protein-level) (inference performed using proteotypic peptides only)
[17:13] Quantification
[17:13] Precursors with monitored PTMs at 1% FDR: 173 out of 173
[17:13] Unmodified precursors with monitored PTM sites at 1% FDR: 72 out of 72
[17:14] Quantification information saved to //mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf3.mzML.quant.

[17:15] File #4/10
[17:15] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf4.mzML
[18:41] 329016 library precursors are potentially detectable
[18:41] Processing...
[18:48] RT window set to 3.75438
[18:48] Peak width: 4.548
[18:48] Scan window radius set to 10
[18:48] Recommended MS1 mass accuracy setting: 11.0278 ppm
[18:56] Optimised mass accuracy: 6.48894 ppm
[19:05] Removing low confidence identifications
[19:05] Searching PTM decoys
[19:05] Removing interfering precursors
[19:07] Training neural networks: 66107 targets, 75745 decoys
[19:13] Number of IDs at 0.01 FDR: 19173
[19:13] Calculating protein q-values
[19:13] Number of genes identified at 1% FDR: 6281 (precursor-level), 6062 (protein-level) (inference performed using proteotypic peptides only)
[19:13] Quantification
[19:14] Precursors with monitored PTMs at 1% FDR: 206 out of 216
[19:14] Unmodified precursors with monitored PTM sites at 1% FDR: 64 out of 67
[19:15] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf4.mzML.quant.

[19:15] File #5/10
[19:15] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf5.mzML
[20:44] 298487 library precursors are potentially detectable
[20:44] Processing...
[20:52] RT window set to 3.71111
[20:52] Peak width: 4.724
[20:52] Scan window radius set to 10
[20:52] Recommended MS1 mass accuracy setting: 11.9263 ppm
[21:00] Optimised mass accuracy: 8.30965 ppm
[21:09] Removing low confidence identifications
[21:09] Searching PTM decoys
[21:09] Removing interfering precursors
[21:11] Training neural networks: 68941 targets, 78850 decoys
[21:18] Number of IDs at 0.01 FDR: 17326
[21:18] Calculating protein q-values
[21:18] Number of genes identified at 1% FDR: 6136 (precursor-level), 6012 (protein-level) (inference performed using proteotypic peptides only)
[21:18] Quantification
[21:18] Precursors with monitored PTMs at 1% FDR: 179 out of 180
[21:18] Unmodified precursors with monitored PTM sites at 1% FDR: 70 out of 70
[21:19] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf5.mzML.quant.

[21:19] File #6/10
[21:19] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf6.mzML
[22:43] 276801 library precursors are potentially detectable
[22:43] Processing...
[22:52] RT window set to 4.1164
[22:52] Peak width: 4.832
[22:52] Scan window radius set to 10
[22:52] Recommended MS1 mass accuracy setting: 11.6854 ppm
[23:02] Optimised mass accuracy: 8.0698 ppm
[23:11] Removing low confidence identifications
[23:11] Searching PTM decoys
[23:11] Removing interfering precursors
[23:13] Training neural networks: 60516 targets, 65661 decoys
[23:18] Number of IDs at 0.01 FDR: 15223
[23:18] Calculating protein q-values
[23:18] Number of genes identified at 1% FDR: 5838 (precursor-level), 5726 (protein-level) (inference performed using proteotypic peptides only)
[23:18] Quantification
[23:18] Precursors with monitored PTMs at 1% FDR: 192 out of 201
[23:18] Unmodified precursors with monitored PTM sites at 1% FDR: 41 out of 42
[23:19] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf6.mzML.quant.

[23:19] File #7/10
[23:19] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf7.mzML
[24:44] 250425 library precursors are potentially detectable
[24:44] Processing...
[24:52] RT window set to 4.3693
[24:52] Peak width: 4.88
[24:52] Scan window radius set to 10
[24:53] Recommended MS1 mass accuracy setting: 12.6321 ppm
[25:02] Optimised mass accuracy: 9.72709 ppm
[25:09] Removing low confidence identifications
[25:09] Searching PTM decoys
[25:09] Removing interfering precursors
[25:11] Training neural networks: 52387 targets, 58249 decoys
[25:16] Number of IDs at 0.01 FDR: 12270
[25:16] Calculating protein q-values
[25:16] Number of genes identified at 1% FDR: 5173 (precursor-level), 5084 (protein-level) (inference performed using proteotypic peptides only)
[25:16] Quantification
[25:16] Precursors with monitored PTMs at 1% FDR: 142 out of 170
[25:16] Unmodified precursors with monitored PTM sites at 1% FDR: 33 out of 42
[25:17] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf7.mzML.quant.

[25:17] File #8/10
[25:17] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf8.mzML
[26:41] 222345 library precursors are potentially detectable
[26:41] Processing...
[26:50] RT window set to 4.44313
[26:50] Peak width: 4.932
[26:50] Scan window radius set to 10
[26:50] Recommended MS1 mass accuracy setting: 11.7335 ppm
[27:02] Optimised mass accuracy: 16.4287 ppm
[27:09] Removing low confidence identifications
[27:09] Searching PTM decoys
[27:09] Removing interfering precursors
[27:10] Training neural networks: 55460 targets, 71528 decoys
[27:16] Number of IDs at 0.01 FDR: 9951
[27:16] Calculating protein q-values
[27:16] Number of genes identified at 1% FDR: 4717 (precursor-level), 4617 (protein-level) (inference performed using proteotypic peptides only)
[27:16] Quantification
[27:16] Precursors with monitored PTMs at 1% FDR: 134 out of 147
[27:16] Unmodified precursors with monitored PTM sites at 1% FDR: 23 out of 28
[27:17] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf8.mzML.quant.

[27:17] File #9/10
[27:17] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf9.mzML
[28:41] 184341 library precursors are potentially detectable
[28:41] Processing...
[28:49] RT window set to 4.24107
[28:49] Peak width: 4.932
[28:49] Scan window radius set to 10
[28:49] Recommended MS1 mass accuracy setting: 11.7887 ppm
[28:59] Optimised mass accuracy: 14.0403 ppm
[29:05] Removing low confidence identifications
[29:05] Searching PTM decoys
[29:05] Removing interfering precursors
[29:06] Training neural networks: 39946 targets, 44787 decoys
[29:10] Number of IDs at 0.01 FDR: 7947
[29:10] Calculating protein q-values
[29:10] Number of genes identified at 1% FDR: 4100 (precursor-level), 4069 (protein-level) (inference performed using proteotypic peptides only)
[29:10] Quantification
[29:10] Precursors with monitored PTMs at 1% FDR: 132 out of 150
[29:10] Unmodified precursors with monitored PTM sites at 1% FDR: 14 out of 18
[29:11] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf9.mzML.quant.

[29:11] File #10/10
[29:11] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf10.mzML
[30:39] 147356 library precursors are potentially detectable
[30:39] Processing...
[30:46] RT window set to 4.51369
[30:46] Peak width: 5.016
[30:46] Scan window radius set to 11
[30:46] Recommended MS1 mass accuracy setting: 12.0928 ppm
[30:58] Optimised mass accuracy: 17.3431 ppm
[31:03] Removing low confidence identifications
[31:03] Searching PTM decoys
[31:03] Removing interfering precursors
[31:04] Training neural networks: 36920 targets, 42884 decoys
[31:08] Number of IDs at 0.01 FDR: 6386
[31:08] Calculating protein q-values
[31:08] Number of genes identified at 1% FDR: 3643 (precursor-level), 3606 (protein-level) (inference performed using proteotypic peptides only)
[31:08] Quantification
[31:08] Precursors with monitored PTMs at 1% FDR: 111 out of 114
[31:08] Unmodified precursors with monitored PTM sites at 1% FDR: 23 out of 23
[31:08] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_gpf10.mzML.quant.

[31:08] Cross-run analysis
[31:08] Reading quantification information: 10 files
[31:11] Averaged recommended settings for this experiment: Mass accuracy = 6ppm, MS1 accuracy = 11ppm, Scan window = 9
[31:11] Quantifying peptides
[31:13] Assembling protein groups
[31:15] Quantifying proteins
[31:15] Calculating q-values for protein and gene groups
[31:16] Calculating global q-values for protein and gene groups
[31:16] Writing report
[31:35] Report saved to /mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.tsv.
[31:35] Saving precursor levels matrix
[31:37] Precursor levels matrix (1% precursor and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.pr_matrix.tsv.
[31:37] Saving protein group levels matrix
[31:37] Protein group levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.pg_matrix.tsv.
[31:37] Saving gene group levels matrix
[31:37] Gene groups levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.gg_matrix.tsv.
[31:37] Saving unique genes levels matrix
[31:37] Unique genes levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.unique_genes_matrix.tsv.
[31:37] Stats report saved to /mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.stats.tsv
[31:37] Generating spectral library:
[31:37] Saving spectral library to /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv
[32:52] 148344 precursors saved
[32:52] Loading the generated library and saving it in the .speclib format
[32:52] Loading spectral library /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv
[33:17] Spectral library loaded: 10944 protein isoforms, 11802 protein groups and 148344 precursors in 128848 elution groups.
[33:17] Loading protein annotations from FASTA /mnt/d/bmsProjects/diaTesting/uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta
[33:18] Gene names missing for some isoforms
[33:18] Library contains 10944 proteins, and 10921 genes
[33:18] Saving the library to /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv.speclib
[33:21] Log saved to /mnt/d/bmsProjects/diaTesting/libGeneration/libGenerate.log.txt
Finished
```

Search the split-DIA runs against the library we just constructed and see how the output data looks. I think it is a good idea to search this as one mass range at a time that we can combine into a single set later on in the analysis portion handled in R. Use a shell script to process through the individual files.

```shell
#!/bin/bash

##define the location of tools you will need
diannLocation="/mnt/e/softwareTools/fragpipe-jre-211/tools/diann/1.8.2_beta_8/linux/diann-1.8.1.8"
baseProjectLocation="/mnt/d/bmsProjects/diaTesting/"

##run diann
for i in 231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia{1..3}
do
    printf "processing diann analysis for mass fraction ${i}.mzML."
    echo
    eval mkdir ${baseProjectLocation}${i}
    eval $diannLocation --f "${baseProjectLocation}${i}.mzML" --lib "${baseProjectLocation}libGeneration/gpfMb231HumanSpecLib.tsv" --threads 28 --verbose 1 --out "${baseProjectLocation}${i}/report.tsv" --qvalue 0.01  --matrices  --reannotate --fasta "${baseProjectLocation}uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta" --met-excision --cut K*,R* --missed-cleavages 1 --min-pep-len 7 --max-pep-len 30 --min-pr-mz 430 --max-pr-mz 930 --min-pr-charge 2 --max-pr-charge 4 --unimod4 --var-mods 1 --var-mod UniMod:35,15.994915,M --var-mod UniMod:1,42.010565,*n --monitor-mod UniMod:1 --smart-profiling --peak-center --no-ifs-removal
##filter the output file
    echo
    printf "filtering the output file for Lib.PG.Q.Value <= 0.01"
    echo
    eval "awk 'BEGIN {FS="\t"}; NR==1; NR > 1{ if($41 <= 0.01) { print }}' ${baseProjectLocation}${i}/report.tsv > ${baseProjectLocation}${i}/reportFiltered.tsv"
done
```

In hindsight, we probably should have included our experimental files in the library generation as well, but at least in this case, they are all the exact same sample, so it should make a big difference, if any. In the future, include the experimental files in the library generation and then go back against this refined library. Output of the command above is below.

```shell
bms@DESKTOP-AN2IGM9:/mnt/d/bmsProjects/diaTesting$ ./diannFractionProcessing.sh |& tee diannFractionProcessingLog.txt
processing diann analysis for mass fraction 231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1.mzML.
mkdir: cannot create directory ‘/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1’: File exists
DIA-NN 1.8.2 beta 8 (Data-Independent Acquisition by Neural Networks)
Compiled on Dec  1 2022 14:47:06
Current date and time: Tue Jan 16 12:33:38 2024
Logical CPU cores: 56
Thread number set to 28
Output will be filtered at 0.01 FDR
Precursor/protein x samples expression level matrices will be saved along with the main report
Library precursors will be reannotated using the FASTA database
N-terminal methionine excision enabled
In silico digest will involve cuts at K*,R*
Maximum number of missed cleavages set to 1
Min peptide length set to 7
Max peptide length set to 30
Min precursor m/z set to 430
Max precursor m/z set to 930
Min precursor charge set to 2
Max precursor charge set to 4
Cysteine carbamidomethylation enabled as a fixed modification
Maximum number of variable modifications set to 1
Modification UniMod:35 with mass delta 15.9949 at M will be considered as variable
Modification UniMod:1 with mass delta 42.0106 at *n will be considered as variable
When generating a spectral library, in silico predicted spectra will be retained if deemed more reliable than experimental ones
Fixed-width center of each elution peak will be used for quantification
Interference removal from fragment elution curves disabled
DIA-NN will optimise the mass accuracy automatically using the first run in the experiment. This is useful primarily for quick initial analyses, when it is not yet known which mass accuracy setting works best for a particular acquisition scheme.
The following variable modifications will be scored: UniMod:1

1 files will be processed
[0:00] Loading spectral library /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv
[0:27] Spectral library loaded: 10944 protein isoforms, 11802 protein groups and 148344 precursors in 128848 elution groups.
[0:27] Loading FASTA /mnt/d/bmsProjects/diaTesting/uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta
[0:53] Reannotating library precursors with information from the FASTA database
[0:54] 148344 precursors generated
[0:54] Gene names missing for some isoforms
[0:54] Library contains 10944 proteins, and 10921 genes
[0:54] Initialising library
[0:55] Saving the library to /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv.speclib

[0:58] File #1/1
[0:58] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1.mzML
[2:24] 51637 library precursors are potentially detectable
[2:24] Processing...
[2:25] RT window set to 1.472
[2:25] Peak width: 3.964
[2:25] Scan window radius set to 8
[2:25] Recommended MS1 mass accuracy setting: 7.29749 ppm
[2:26] Optimised mass accuracy: 10.7296 ppm
[2:28] Removing low confidence identifications
[2:28] Searching PTM decoys
[2:28] Removing interfering precursors
[2:29] Training neural networks: 49666 targets, 31006 decoys
[2:33] Number of IDs at 0.01 FDR: 47696
[2:34] Calculating protein q-values
[2:34] Number of genes identified at 1% FDR: 7814 (precursor-level), 7313 (protein-level) (inference performed using proteotypic peptides only)
[2:34] Quantification
[2:34] Precursors with monitored PTMs at 1% FDR: 336 out of 336
[2:34] Unmodified precursors with monitored PTM sites at 1% FDR: 158 out of 158
[2:37] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1.mzML.quant.

[2:38] Cross-run analysis
[2:38] Reading quantification information: 1 files
[2:39] Quantifying peptides
[2:39] Assembling protein groups
[2:39] Quantifying proteins
[2:39] Calculating q-values for protein and gene groups
[2:39] Calculating global q-values for protein and gene groups
[2:39] Writing report
[2:45] Report saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.tsv.
[2:45] Saving precursor levels matrix
[2:46] Precursor levels matrix (1% precursor and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.pr_matrix.tsv.
[2:46] Saving protein group levels matrix
[2:46] Protein group levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.pg_matrix.tsv.
[2:46] Saving gene group levels matrix
[2:46] Gene groups levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.gg_matrix.tsv.
[2:46] Saving unique genes levels matrix
[2:46] Unique genes levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.unique_genes_matrix.tsv.
[2:46] Stats report saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.stats.tsv
[2:46] Log saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.log.txt
Finished


filtering the output file for Lib.PG.Q.Value <= 0.01
processing diann analysis for mass fraction 231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2.mzML.
mkdir: cannot create directory ‘/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2’: File exists
DIA-NN 1.8.2 beta 8 (Data-Independent Acquisition by Neural Networks)
Compiled on Dec  1 2022 14:47:06
Current date and time: Tue Jan 16 12:36:47 2024
Logical CPU cores: 56
Thread number set to 28
Output will be filtered at 0.01 FDR
Precursor/protein x samples expression level matrices will be saved along with the main report
Library precursors will be reannotated using the FASTA database
N-terminal methionine excision enabled
In silico digest will involve cuts at K*,R*
Maximum number of missed cleavages set to 1
Min peptide length set to 7
Max peptide length set to 30
Min precursor m/z set to 430
Max precursor m/z set to 930
Min precursor charge set to 2
Max precursor charge set to 4
Cysteine carbamidomethylation enabled as a fixed modification
Maximum number of variable modifications set to 1
Modification UniMod:35 with mass delta 15.9949 at M will be considered as variable
Modification UniMod:1 with mass delta 42.0106 at *n will be considered as variable
When generating a spectral library, in silico predicted spectra will be retained if deemed more reliable than experimental ones
Fixed-width center of each elution peak will be used for quantification
Interference removal from fragment elution curves disabled
DIA-NN will optimise the mass accuracy automatically using the first run in the experiment. This is useful primarily for quick initial analyses, when it is not yet known which mass accuracy setting works best for a particular acquisition scheme.
The following variable modifications will be scored: UniMod:1

1 files will be processed
[0:00] Loading spectral library /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv
[0:29] Spectral library loaded: 10944 protein isoforms, 11802 protein groups and 148344 precursors in 128848 elution groups.
[0:29] Loading FASTA /mnt/d/bmsProjects/diaTesting/uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta
[0:55] Reannotating library precursors with information from the FASTA database
[0:56] 148344 precursors generated
[0:56] Gene names missing for some isoforms
[0:56] Library contains 10944 proteins, and 10921 genes
[0:56] Initialising library
[0:57] Saving the library to /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv.speclib

[1:01] File #1/1
[1:01] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2.mzML
[2:29] 52188 library precursors are potentially detectable
[2:29] Processing...
[2:30] RT window set to 1.66561
[2:30] Peak width: 3.756
[2:30] Scan window radius set to 8
[2:30] Recommended MS1 mass accuracy setting: 9.26026 ppm
[2:31] Optimised mass accuracy: 8.0321 ppm
[2:33] Removing low confidence identifications
[2:33] Searching PTM decoys
[2:33] Removing interfering precursors
[2:34] Training neural networks: 49843 targets, 18747 decoys
[2:37] Number of IDs at 0.01 FDR: 48250
[2:38] Calculating protein q-values
[2:38] Number of genes identified at 1% FDR: 7901 (precursor-level), 7556 (protein-level) (inference performed using proteotypic peptides only)
[2:38] Quantification
[2:38] Precursors with monitored PTMs at 1% FDR: 489 out of 489
[2:38] Unmodified precursors with monitored PTM sites at 1% FDR: 166 out of 166
[2:40] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2.mzML.quant.

[2:40] Cross-run analysis
[2:40] Reading quantification information: 1 files
[2:41] Quantifying peptides
[2:41] Assembling protein groups
[2:42] Quantifying proteins
[2:42] Calculating q-values for protein and gene groups
[2:42] Calculating global q-values for protein and gene groups
[2:42] Writing report
[2:49] Report saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.tsv.
[2:49] Saving precursor levels matrix
[2:50] Precursor levels matrix (1% precursor and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.pr_matrix.tsv.
[2:50] Saving protein group levels matrix
[2:50] Protein group levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.pg_matrix.tsv.
[2:50] Saving gene group levels matrix
[2:50] Gene groups levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.gg_matrix.tsv.
[2:50] Saving unique genes levels matrix
[2:50] Unique genes levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.unique_genes_matrix.tsv.
[2:50] Stats report saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.stats.tsv
[2:50] Log saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.log.txt
Finished


filtering the output file for Lib.PG.Q.Value <= 0.01
processing diann analysis for mass fraction 231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3.mzML.
mkdir: cannot create directory ‘/mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3’: File exists
DIA-NN 1.8.2 beta 8 (Data-Independent Acquisition by Neural Networks)
Compiled on Dec  1 2022 14:47:06
Current date and time: Tue Jan 16 12:39:54 2024
Logical CPU cores: 56
Thread number set to 28
Output will be filtered at 0.01 FDR
Precursor/protein x samples expression level matrices will be saved along with the main report
Library precursors will be reannotated using the FASTA database
N-terminal methionine excision enabled
In silico digest will involve cuts at K*,R*
Maximum number of missed cleavages set to 1
Min peptide length set to 7
Max peptide length set to 30
Min precursor m/z set to 430
Max precursor m/z set to 930
Min precursor charge set to 2
Max precursor charge set to 4
Cysteine carbamidomethylation enabled as a fixed modification
Maximum number of variable modifications set to 1
Modification UniMod:35 with mass delta 15.9949 at M will be considered as variable
Modification UniMod:1 with mass delta 42.0106 at *n will be considered as variable
When generating a spectral library, in silico predicted spectra will be retained if deemed more reliable than experimental ones
Fixed-width center of each elution peak will be used for quantification
Interference removal from fragment elution curves disabled
DIA-NN will optimise the mass accuracy automatically using the first run in the experiment. This is useful primarily for quick initial analyses, when it is not yet known which mass accuracy setting works best for a particular acquisition scheme.
The following variable modifications will be scored: UniMod:1

1 files will be processed
[0:00] Loading spectral library /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv
[0:30] Spectral library loaded: 10944 protein isoforms, 11802 protein groups and 148344 precursors in 128848 elution groups.
[0:30] Loading FASTA /mnt/d/bmsProjects/diaTesting/uniprotkb_proteome_UP000005640_AND_revi_2024_01_15.fasta
[0:56] Reannotating library precursors with information from the FASTA database
[0:57] 148344 precursors generated
[0:57] Gene names missing for some isoforms
[0:57] Library contains 10944 proteins, and 10921 genes
[0:57] Initialising library
[0:58] Saving the library to /mnt/d/bmsProjects/diaTesting/libGeneration/gpfMb231HumanSpecLib.tsv.speclib

[1:01] File #1/1
[1:01] Loading run /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3.mzML
[2:30] 47477 library precursors are potentially detectable
[2:30] Processing...
[2:31] RT window set to 1.78525
[2:31] Peak width: 4.268
[2:31] Scan window radius set to 9
[2:31] Recommended MS1 mass accuracy setting: 9.29387 ppm
[2:32] Optimised mass accuracy: 9.76162 ppm
[2:33] Removing low confidence identifications
[2:33] Searching PTM decoys
[2:33] Removing interfering precursors
[2:35] Training neural networks: 44408 targets, 20531 decoys
[2:38] Number of IDs at 0.01 FDR: 41313
[2:38] Calculating protein q-values
[2:38] Number of genes identified at 1% FDR: 7509 (precursor-level), 7148 (protein-level) (inference performed using proteotypic peptides only)
[2:38] Quantification
[2:38] Precursors with monitored PTMs at 1% FDR: 570 out of 578
[2:38] Unmodified precursors with monitored PTM sites at 1% FDR: 114 out of 116
[2:41] Quantification information saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3.mzML.quant.

[2:41] Cross-run analysis
[2:41] Reading quantification information: 1 files
[2:41] Quantifying peptides
[2:41] Assembling protein groups
[2:42] Quantifying proteins
[2:42] Calculating q-values for protein and gene groups
[2:42] Calculating global q-values for protein and gene groups
[2:42] Writing report
[2:48] Report saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.tsv.
[2:48] Saving precursor levels matrix
[2:48] Precursor levels matrix (1% precursor and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.pr_matrix.tsv.
[2:48] Saving protein group levels matrix
[2:48] Protein group levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.pg_matrix.tsv.
[2:48] Saving gene group levels matrix
[2:48] Gene groups levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.gg_matrix.tsv.
[2:48] Saving unique genes levels matrix
[2:48] Unique genes levels matrix (1% precursor FDR and protein group FDR) saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.unique_genes_matrix.tsv.
[2:48] Stats report saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.stats.tsv
[2:48] Log saved to /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.log.txt
Finished


filtering the output file for Lib.PG.Q.Value <= 0.01
```

Combine the reports output by the tool so that we can use this file as an input for the iq R package. 

```shell
##combine the output files
head -n 1 /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.tsv > /mnt/d/bmsProjects/diaTesting/combinedFilteredReport.tsv; tail -n +2 -q /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia1/report.tsv /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia2/report.tsv /mnt/d/bmsProjects/diaTesting/231220_mb231Standard_1in20_2uLInject_75umBy25cmWith2umC18_dia3/report.tsv >> /mnt/d/bmsProjects/diaTesting/combinedFilteredReport.tsv


```