# Rnmr1D

Rnmr1D is the main module in the NMRProcFlow web application (http://nmrprocflow.org) concerning the NMR spectra processing.

* Inside NMRProcFlow, Rnmr1D allows users to process their NMR spectra within a GUI application and thus the macro-command sequence coming from this process can be saved. 

* Outside NMRProcFlow Rnmr1D become an R package allowing users to replay  the macro-command sequence generated within NMRProcFlow. Moreover, without using NMRProcFlow, this package can also be used to replace any 'home-made script'  by a macro-command sequence.

* See the [Macro-command Reference Guide](https://nmrprocflow.org/themes/pdf/Macrocommand.pdf) to have more details about macro-commands.

## Installation of some dependencies

* You may need to install a C++ compiler if not the case yet (see https://teuder.github.io/rcpp4everyone_en/020_install.html)

* Some R packages:

```R
source('http://bioconductor.org/biocLite.R');
biocLite('MassSpecWavelet'); biocLite('impute');
install.packages(c('doParallel', 'ptw', 'signal', 'speaq', 'base64enc', 'XML', 'igraph'), repos='http://cran.rstudio.com')
```

## Installation of the R package 

* Note for Windows 7/10: Before performing the installation within R GUI it may require to specify the Compiler binaries path in the PATH environment variable so that the C++ code compilation will be correctly done ( check with Sys.getenv("PATH") )

```R
require(devtools)
install_github("djacob65/Rnmr1D", dependencies = TRUE)
```

## Example of use


```R
library(Rnmr1D)

# Test with the provided example data
data_dir <- system.file("extra", package = "Rnmr1D")
RAWDIR <- file.path(data_dir, "MMBBI_14P05")
CMDFILE <- file.path(data_dir, "NP_macro_cmd.txt")
SAMPLEFILE <- file.path(data_dir, "Samples.txt")

# Detect the number of Cores
detectCores()

# Launch the pre-processing then the processing defined in the macro-command file
out <- Rnmr1D(RAWDIR, cmdfile=CMDFILE, samplefile=SAMPLEFILE, ncpu=detectCores())

# Have a look on returned data structure
ls(out)
ls(out$specMat)

# Stacked Plot with a perspective effect
plot.SpecMat(out$specMat, ppm_lim=c(0.5,5))

# Overlaid Plot
plot.SpecMat(out$specMat, ppm_lim=c(0.5,5), K=0)

# Get the data matrix 
outMat <- get_Buckets_dataset(out, norm_meth='CSN')

# Get the Signal/Noise Ratio (SNR) matrix 
outSNR <- get_SNR_dataset(out, c(10.2,10.5), ratio=TRUE)

# Get the bucket table
outBucket <- get_Buckets_table(out)

# Get the spectra data
spectra <- get_Spectra_Data(out)

```

* See a quick presentation online with output examples (https://nmrprocflow.org/themes/pdf/NMRProcFlow_Rstudio.pdf)

