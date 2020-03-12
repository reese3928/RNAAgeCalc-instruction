Hi Daniel @dvantwisk ,

Thanks very much for your review. We have addressed all of your comments and the package is ready for second round review. Here is our point-by-point response to your comments.

> R/count2FPKM.R
> 
> * [X]  [REQUIRED] In the `DESeq2` package there is a function called `fpkm()`
>   that converts raw counts to fpkm. Since it's best to reuse existing
>   functionality, is there a reason not use `fpkm()` here and to create a new
>   function? If not, it would be better to use `fpkm()`.

We understand that reusing existing functionality is preferable than writing own functions. To reuse existing function when calculating fpkm, we are now using `getRPKM()` function from the [recount](https://bioconductor.org/packages/release/bioc/html/recount.html) package. The reason that we use `getRPKM()` from recount rather than `fpkm()` from DESeq2 is because our internal gene length database was also retrieved from recount. Thus using the functionality from recount is more consistent than using the functionality in DESeq2.

In addition, it is difficult to completely remove our function `count2FPKM()` because the function supports the calculation of fpkm for different gene id types ("SYMBOL", "ENSEMBL", "ENTREZID", "REFSEQ") and there are some preprocess that needs to be done before calling `getRPKM()` function. So we kept this function in the package for now.

> R/exampledata.R
> 
> * [X]  [REQUIRED] Please be more explicit about the source and shape of the data
>   in the documentation.

The shape, data source, reference, and how we process the example data is now included in the 
documentation files fpkm.Rd and rawcount.Rd.

> R/predict_age.R
> 
> * [X]  [CONSIDER] Using `match.arg()`. In the function `predict_age()`, the
>   argument `exprtype` can be written as `epxrtype=c("FPKM","counts")`. The
>   default value is "FPKM". This makes it explicit to the user which arguments are
>   accepted here. The user's selected value can be obtained with
>   `exprtype <- match.arg(exprtype)`. An error will be thrown if any unacceptable
>   values are given. This can be done in other arguments throughout the package.

Thank you for the suggestion. We are now using `exprtype = c("FPKM", "counts")` and then `exprtype <- match.arg(exprtype)` for this type of arguments throughout our package.

> 
> R/sysdata.rda
> 
> * [X]  [REQUIRED] Is `system.rda` needed? If not, please remove it.

The `system.rda` is essential for our package. It contains our research results discussed in our [manuscript](https://www.biorxiv.org/content/10.1101/2020.02.14.950188v1). The `system.rda` contains the gene signatures required to make the RNAAge calculation. This internal data is used in our `predict_age()` function line 249 and 251. The R script to generate this internal data was saved in: data-raw/internalData.R. 



