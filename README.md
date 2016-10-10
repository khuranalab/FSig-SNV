## FSig-SNV

### An R package detecting SNV coding and non-coding drivers in cancer

Installing FSig-SNV


You will need to install devtools for running FSigSNV package

```sh
install.packages("devtools")
library("devtools")
devtools::install_github("khuranalab/FSig-SNV")
```

### FSig-SNV example
User will need to 
* (1) download drm.gene.bed file and put it in the "/path/to/dataContext"
* (2) assign "/path/to/Output.vcf" path for FunSeq2 annotated vcf file
* (3) assign "/path/to/output" path for saving FSig-SNV results
* (4) tumorType: name of tumor type
* (5) useCores: number of cores for parellel computation 
* (6) seedNum:  random number seed number (default is 42)  
* (7) reSampleIter: sampling iterations (suggesting number is 1000000 iterations) 

```sh

library(FsigSNV)

#####
# global parameters setup
#####

  dataContextDir<-"/path/to/dataContext"
  funseq2OutputFile<-"/path/to/Output.vcf"
  outputDir<-"/path/to/output"
  tumorType<-"Prostate"
  seedNum<-42
  reSampleIter<-1000
  useCores<-6
  debugMode<-FALSE

#####
  
  preProcessVCF(funseq2OutputFile,outputDir,tumorType,useCores)


#####

  mutationType<-"CDS"
  cdsOutputDf<-getCDSpvalue(inputDir,tumorType,mutationType,
                            reSampleIter=reSampleIter,
                            seedNum=seedNum,debugMode=debugMode)

#####

  mutationType<-"promoter"
  promoterOutputDf<-getPromoterPvalue(inputDir,tumorType,mutationType,
                            reSampleIter=reSampleIter,
                            seedNum=seedNum,debugMode=debugMode)

#####

  mutationType<-"lincRNA"
  lincRNAOutputDf<-getLincRNAPvalue(inputDir,tumorType,mutationType,
                                      reSampleIter=reSampleIter,
                                      seedNum=seedNum,debugMode=debugMode)
#####

  fileName<-"drm.gene.bed"
  fileName<-file.path(dataContextDir,fileName)
  enhancerGeneInteractionFileName<-fileName

  mutationType<-"enhancerUnit"
  enhancerUnitOutputDf<-getEnhancerUnitPvalue(inputDir,tumorType,mutationType,
                                    reSampleIter=reSampleIter,
                                    seedNum=seedNum,
                                    enhancerGeneInteractionFileName,
                                    useCores=useCores,
                                    debugMode=debugMode)

#####

```

