# BioCmirrors

## Introduction

The `BioCmirrors` package provides functions to check the health status
of Bioconductor mirrors. The package is useful for users who want to
ensure that the mirror they are using is up-to-date and has all the
packages available. The package also provides a list of all available
mirrors and their statistics. The capital `C` in `BioCmirrors` is
intentional to be consistent with the `chooseBioCmirror` function in the
`utils` package.

## Installation

Currently, the package can only be found on GitHub. To install it, use
the following:

    if (!requireNamespace("BiocManager", quietly=TRUE))
        install.packages("BiocManager")

    BiocManager::install("Bioconductor/BioCmirrors")

## Quick Start

    library(BioCmirrors)

## Mirror Health Checks

The `checkBioCmirror` function checks the health status of a
Bioconductor mirror including the main Bioconductor URL:

    url <- "https://bioconductor.org"
    checkBioCmirror(mirror = url)
    #> Warning in rep(as.integer(len), length = length(str)): partial argument match of 'length' to 'length.out'
    #> https://bioconductor.org/packages/3.21/bioc/src/contrib/PACKAGES 
    #>                                                             TRUE

The function can also be run interactively, where it will display a list
of mirrors to choose from:

    checkBioCmirror()
    #' Secure BioC mirrors 
    #' 
    #'  1: 0-Bioconductor (World-wide) [https]   2: USA (Boston) [https]               
    #'  3: Germany (Gottingen) [https]           4: Japan (Wako) [https]               
    #'  5: Taiwan (Hsinchu) [https]              6: China (Peking) [https]             
    #'  7: China (Nanjing) [https]               8: China (Hefei Anhui) [https]        
    #'  9: Norway (Bergen) [https]              10: Denmark (Aalborg) [https]          
    #' 11: Sweden (Umea) [https]                12: (other mirrors)                    
    #' 
    #' 
    #' Selection: 3
    #' https://ftp.gwdg.de/pub/misc/bioconductor/packages/bioc/src/contrib/PACKAGES 
    #'                                                                         TRUE 

## List of Mirrors

The `listBioCmirrors` function returns a list of all available
Bioconductor mirrors:

    mirrors <- listBioCmirrors()
    mirrors
    #> # A tibble: 34 × 10
    #>    Name                                  Country City  URL   Host  Maintainer    OK CountryCode Comment Protocol
    #>    <chr>                                 <chr>   <chr> <chr> <chr> <chr>      <int> <chr>       <chr>   <chr>   
    #>  1 0-Bioconductor (World-wide) [https]   0-Bioc… Worl… http… Bioc… Bioconduc…     1 us          "secur… https   
    #>  2 0-Bioconductor (World-wide) [unsecur… 0-Bioc… Worl… http… Bioc… Bioconduc…     0 us          "secur… http    
    #>  3 USA (Boston) [https]                  USA     Bost… http… Posi… Joshua Sp…     1 us          "secur… https   
    #>  4 USA (Boston) [unsecure]               USA     Bost… http… Posi… Joshua Sp…     0 us          "secur… http    
    #>  5 Germany (Dortmund) [https]            Germany Dort… http… Depa… Uwe Ligge…     1 de          ""      https   
    #>  6 Germany (Dortmund) [unsecure]         Germany Dort… http… Depa… Uwe Ligge…     1 de          ""      http    
    #>  7 Germany (Gottingen) [https]           Germany Gott… http… GWDG  Tim Ehler…     1 de          "secur… https   
    #>  8 Germany (Gottingen) [unsecure]        Germany Gott… http… GWDG  Tim Ehler…     1 de          "secur… http    
    #>  9 Japan (Wako) [https]                  Japan   Wako  http… RIKE… Itoshi NI…     1 jp          "secur… https   
    #> 10 Japan (Wako) [unsecure]               Japan   Wako  http… RIKE… Itoshi NI…     1 jp          "secur… http    
    #> # ℹ 24 more rows

The `tibble` output can be used to filter the mirrors by country:

    mirrors |>
        dplyr::filter(.data[["Country"]] == "USA")
    #> # A tibble: 2 × 10
    #>   Name                    Country City   URL                 Host  Maintainer    OK CountryCode Comment Protocol
    #>   <chr>                   <chr>   <chr>  <chr>               <chr> <chr>      <int> <chr>       <chr>   <chr>   
    #> 1 USA (Boston) [https]    USA     Boston https://bioconduct… Posi… Joshua Sp…     1 us          secure… https   
    #> 2 USA (Boston) [unsecure] USA     Boston http://bioconducto… Posi… Joshua Sp…     0 us          secure… http

## Mirror Statistics

The `BioCmirrorStats` function returns details about the `PACKAGES` file
time stamp, the number of packages available, and the number of packages
mirrored. The function can be used to check the status of a specific
mirror. In this example, we reproduce the output of
`utils::chooseBioCmirror()` by creating a named string with the URL of
the mirror in Germany:

    url <- "https://ftp.gwdg.de/pub/misc/bioconductor"
    de_mirror <- c("Germany (Gottingen) [https]" = url)
    stats <- BioCmirrorStats(mirror = de_mirror)
    stats
    #> Bioconductor version: 3.21
    #> Bioconductor mirror:
    #>    https://ftp.gwdg.de/pub/misc/bioconductor
    #> PACKAGES timestamp: 
    #> Query timestamp: 2025-02-26 22:57 UTC
    #> Bioconductor software packages: 2258
    #> Mirror packages: 2258
    #> Mirror software packages: 2258
    #> Missing mirror software packages: 0
    #> 
    #> Out-of-date mirror software packages: 0

As shown, the packages on the mirror are up-to-date, and there are no
missing packages.

# Session Info

<details>
<summary>
Click to expand
</summary>

    sessionInfo()
    #> R Under development (unstable) (2025-02-06 r87702)
    #> Platform: x86_64-pc-linux-gnu
    #> Running under: Ubuntu 24.04.2 LTS
    #> 
    #> Matrix products: default
    #> BLAS/LAPACK: /usr/lib/x86_64-linux-gnu/openblas-pthread/libopenblasp-r0.3.26.so;  LAPACK version 3.12.0
    #> 
    #> locale:
    #>  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C               LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
    #>  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8    LC_PAPER=en_US.UTF-8       LC_NAME=C                 
    #>  [9] LC_ADDRESS=C               LC_TELEPHONE=C             LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
    #> 
    #> time zone: America/New_York
    #> tzcode source: system (glibc)
    #> 
    #> attached base packages:
    #> [1] stats     graphics  grDevices utils     datasets  methods   base     
    #> 
    #> other attached packages:
    #> [1] BioCmirrors_0.0.1
    #> 
    #> loaded via a namespace (and not attached):
    #>  [1] vctrs_0.6.5         httr_1.4.7          cli_3.6.4           knitr_1.49          rlang_1.1.5        
    #>  [6] xfun_0.51           processx_3.8.6      generics_0.1.3      glue_1.8.0          RCurl_1.98-1.16    
    #> [11] htmltools_0.5.8.1   pkgbuild_1.4.6      ps_1.9.0            rsconnect_1.3.4     rmarkdown_2.29     
    #> [16] evaluate_1.0.3      tibble_3.2.1        bitops_1.0-9        fastmap_1.2.0       yaml_2.3.10        
    #> [21] lifecycle_1.0.4     BiocManager_1.30.25 compiler_4.5.0      dplyr_1.1.4         codetools_0.2-20   
    #> [26] pkgconfig_2.0.3     rstudioapi_0.17.1   digest_0.6.37       R6_2.6.1            tidyselect_1.2.1   
    #> [31] utf8_1.2.4          curl_6.2.1          pillar_1.10.1       callr_3.7.6         magrittr_2.0.3     
    #> [36] tools_4.5.0         remotes_2.5.0       desc_1.4.3

</details>
