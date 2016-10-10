## FSig-SNV

### An R package detecting SNV coding and non-coding drivers in cancer

Installing FSig-SNV


You will need to install devtools for running FSigSNV package

```sh
install.packages("devtools")
library("devtools")
devtools::install_github("khuranalab/FSig-SNV")
```

FSig-SNV example

```sh

library(jsonlite)
library(fsigsnv)


config <- fromJSON("tmp/fsigSNV.json")

#####
# global parameters setup
#####

dataContextDir<-config$global$dataContextDir
outputDir<-config$global$outputDir
seedNum<-config$global$seedNum
reSampleIter<-config$global$reSampleIter
useCores<-config$global$useCores
debugMode<-config$global$debugMode

tumorTypeList<-names(config$tumorTypes)

for(tumorType in tumorTypeList)
{
#cancerType<-names(config$cancerTypes)[2]

#####
# setup netbox related parameters
#####

  #source(file.path("tmp","fsigsnvParameterSetup.R"))

#####

  funseq2OutputFile<-config$tumorTypes[[tumorType]]$funseq2OutputFile

  preProcessVCF(funseq2OutputFile,outputDir,tumorType,useCores)


#####

  # where to load preProcessed Rd
  inputDir<-outputDir

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
                                    useCores=6,
                                    debugMode=debugMode)

#####

}

```

