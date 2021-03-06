### Model-free analysis and permutation tests for allelic associations

04/01/2000

1.  [Introduction](#anchor171318)  
    [Model-free analysis](#anchor175938)  
    [Testing marker-marker association](#anchor176235)  
    [Permutation tests](#anchor176601)
2.  [Format of input files](#anchor177243)  
    [EHPLUS and PMPLUS](#anchor177658)
3.  [How to run the programs](#anchor177948)
4.  [The output](#anchor178242)  
    [Screen output](#anchor178473)  
    [Model-free case-control analysis](#anchor178842)  
    [Marker-marker association analysis](#anchor179076)
5.  [Examples](#anchor179349)  
    [Standard EH and EHPLUS analyses](#anchor179593)  
    [PM analyses](#anchor180163)  
    [Marker-marker analysis](#anchor180651)  
    [Disease-marker analysis](#anchor181458)
6.  [Problems](#anchor182061)
7.  [Compilation](#anchor182295)  
    [C source](#anchor182542)  
    [Program constants](#anchor182885)
8.  [References](#anchor183284)
9.  [Acknowledgements](#anchor183683)
10.  [Literature](#anchor184045)
11.  [Addresses](#anchor184384)  

This documentation describes several programs that perform model-free analysis and permutation tests for allelic associations between a disease locus and markers or between groups of markers. These include a utility program called PM (Permutation and Model-free analysis) which prepares data files for the EH (Estimating Haplotypes) program and which reports various test statistics, as well as PMPLUS and EHPLUS, which are their variants designed for analyses involving very large numbers of possible haplotypes.

### <a name="anchor171318"></a>1\. Introduction

The original EH program was developed by Xie and Ott (Xie and Ott, 1993) to test for allelic association between loci by comparing the likelihoods of the data under the assumptions of no association and of association. In the former case it is assumed that alleles at different loci occur independently so haplotype frequencies are formed as the product of constituent allele frequencies, while if association is allowed for haplotype frequencies are estimated from the data. EH can be used to test for association between genetic markers or between a disease locus and one or more markers. In order to test for association with a disease locus it is necessary to provide values for the penetrance parameters and disease allele frequency so that the probability of the observed disease phenotype can be incorporated into the likelihood calculations.

EH outputs up to three log likelihoods. The first, l0, assumes no marker-marker association. The second log likelihood, l1, assumes marker-marker association but that the disease locus (if present) is not associated with any marker haplotype. The third log likelihood, l2, assumes that there may be association between the disease and marker loci as well as marker-marker association. 2(l1-l0) provides a test for association between markers and 2(l2-l1) a test for association of disease with the marker(s). Each likelihood is maximised over a different number of parameters. l0 is maximised over N0 parameters, equal to the sum of the number of alleles minus 1 for each locus. l1 is maximised over N1 parameters, equal to the number of possible marker haplotypes minus 1\. l2 is maximised over N2 parameters, where N2=2*N1.  

<a name="anchor175938"></a>**Model-free analysis**

The conventional analysis using EH requires that the disease model be fully specified using a penetrance vector (f0, f1, f2), and q, the frequency of the disease allele. The penetrance parameter fi, i=0,1,2, defines the probability of being affected given there are i copy(s) of disease allele at the disease locus. For a non-Mendelian trait these values will not be known with any degree of accuracy. Therefore PM provides a number of methods for testing for association between marker(s) and disease, avoiding the need to accurately specify the transmission model.

To achieve a model-free analysis, PM outputs five different statistics, conveniently called T1-T5\. The first of these is the conventional likelihood ratio test based on the user-specified model, consisting of 2(l2-l1) which is taken to be a chi-squared statistic with N1 degrees of freedom. This model might represent the best guess of what the true transmission model might be. PM also uses this model to calculate a population prevalence for this disease (Kp=f0*[1-q]*[1-q]+2*f1*q*[1-q]+f2*q*q) and uses this population prevalence for all the other models it tests. In addition to outputting the likelihood ratio test statistic based on the user-specified model, PM also outputs similar statistics based on a Mendelian dominant model (f0=0, f1=f2=1) and a Mendelian recessive model (f0=f1=0, f2=1). Both these models are fixed to have appropriate values of q in order to produce the correct population prevalence and in each case the test statistic can be compared to a chi-squared statistic with N1 degrees of freedom. PM next outputs a statistic based on the difference between the log likelihood assuming no association, l1, maximised over transmission model, and the log likelihood allowing association, l2, also maximised over disease model. The maximum likelihood disease models will not necessarily be the same under the hypotheses of association and no association. The log likelihoods l1 and l2 are maximised over a range of disease models varying from Mendelian recessive to null effect and from null effect to Mendelian dominant. This method is analogous to that used in our model-free linkage program MFLINK (Curtis and Sham, 1995). Theoretically, the resulting test statistic could be referred to a chi-squared distribution with N1+1 degrees of freedom, although in practice this may be somewhat conservative, and it may be preferable to obtain a p value using Monte Carlo methods (see below). The fifth and final statistic output by PM to test disease-marker association is based on a heterogeneity test for whether the marker allele or haplotype frequencies are the same in case and control samples. The likelihood of the dataset assuming the same frequencies in both is compared with the likelihood when the frequencies are estimated separately in both samples. This yields a likelihood ratio test that can be compared with a chi-squared distribution having N1 degrees of freedom.

<a name="anchor176235"></a>**Testing marker-marker association**

Conventionally, EH tests for marker-marker association by comparing log likelihoods under the hypothesis that no markers are associated, l0, with the log likelihood assuming that some or all markers may be associated with each other, l1\. 2(l1-l0) can then be compared to a chi-squared statistic with N1-N0 degrees of freedom, where N0 and N1 are the degrees of freedom associated with the two log likelihoods. PM also allows one to compare the hypothesis that all markers are associated with each other with the hypothesis that there are two groups or "blocks" of markers, the markers within each block being associated with each other but there being no association between the blocks. Typically this would be useful if one had a block of markers known to be in linkage disequilibrium and one wished to test whether a new marker was associated with them, in which case the second "block" would consist of just this single marker. However if desired both blocks may contain more than one marker.

The log likelihoods assuming no association and association for the first block are written l0' and l1', and for the second block as l0'' and l1''. If markers from the two blocks can form h1 and h2 haplotypes respectively, the statistic 2(l1-l1'-l1'') would then be a test of association between the blocks with (h1h2-1)-(h1-1)-(h2-1) degrees of freedom. The statistic 2(l1'+l1''-l0) with (h1-1)+(h2-1)-N0 degrees of freedom tests whether there is association within blocks against no association.

<a name="anchor176601"></a>**Permutation tests**

Because p values based on asymptotic chi-squared distributions may be inaccurate, especially when samples sizes are relatively small and/or the number of possible haplotypes is relatively large, PM also implements a permutation procedure to carry out Monte Carlo test of statistical significance. Multiple datasets are generated at random from the real dataset assuming the null hypothesis of no association and the proportion of times the permuted dataset yielding a test statistic more extreme than that produced by the real dataset provides an empirical estimate of statistical significance. For case-control samples, the observed genotypes are used but affection status is assigned randomly, with the constraint that the number of subjects who are cases be kept constant. Each of the five test statistics listed above is calculated for the real dataset and the permuted datasets. The proportion of times each statistic from the real dataset is reached or exceeded by that from a permuted dataset yields the empirical significance of that statistic.

In the case of marker-marker associations, PM performs permutation test for association between two blocks of markers by permuting the genotypes for the second block of markers between subjects, while keeping the genotypes within each block intact. The proportion of occasions on which randomly permuted datasets produce test statistics exceeding that produced from the actual dataset provides the significance of the test for marker-marker association.

When Monte Carlo procedures are used to produce an estimate of the statistical significance, p, then the approximate standard error of this estimate is sqrt(p(1-p)/n), where n is the number of simulations performed.

### <a name="anchor177243"></a>2\. Format of input files

The input files used by EH for both marker-marker and disease-marker analyses have the same format. For marker-marker analysis a single input file is needed and called eh.dat, which contains the observed genotypes. For disease-marker analysis two separate files are required and called control.dat and case.dat which contain the marker genotypes for cases and controls, respectively. The first line of the input files contains the number of alleles at each locus and the following lines contain a table consisting of counts of the observed genotypes. If there are n loci then the rows of this table represent all the possible multilocus genotypes of the first n-1 loci and the columns represent the genotypes of the last locus. Consider an analysis of three loci with 3, 4, and 2 alleles: there are [3(3+1)/2][4(4+1)/2][2(2+1)/2]=180 possible multilocus genotypes. The genotype table has 60 rows for the genotypes of the first two loci and 3 columns for the genotypes of the last one. Although it is not too difficult to produce the required information when there is a very small number of markers each with small number of alleles the process can rapidly become very complicated, so PM automatically produces input files for EH based on a list of observed genotypes.

PM itself requires two input files, a datafile consisting of the observed genotypes and affection status and a parameter file describing the datafile and the type of analysis required.

The parameter file contains at most six lines:

Line 1 consists of four integer variables. The first variable indicates the number of marker loci contained in the data file. The second variable takes value of 0 for a marker-marker analysis and of 1 for case-control analysis. If a value of 0 is specified for a case-control datafile then the program uses all the cases and controls to do a marker-marker analysis. The third variable tells the program if the affection status of subjects is to be permuted in a case-control analysis. The last variable specifies the number of permutations to be performed.

Line 2 lists the number of alleles for all marker loci in the data file.

Line 3 consists of two variables. The first one indicates whether the genotype in the datafile are provided as pairs of alleles or numbered genotypes. If pairs of alleles are used then the variable takes the value of 0\. If instead it is set to 1 then this means that numbered genotypes are presented instead so that for a 3-allele markers genotypes 1/1, 1/2, 1/3, 2/2, 2/3, 3/3 would be coded as numbers 1-6\. The second variable is 0 or 1 according to whether screen output of the genotypes is suppressed or allowed, respectively.

Line 4 consists of a list of 0-1 indicator variables corresponding to each marker, taking value of 1 if the marker is to be used and taking a value of 0 if that marker is present in the datafile but is not to be used in the analysis.

Line 5 consists of a list of 0-1 indicator variables for each marker determining which block a marker belongs to for permutation analysis of marker-marker data. All markers with a 1 in line 5 and a 1 in line 4 form one block, while the other block is formed by markers having a 0 in line 5 and a 1 in line 4\. (Markers with a 0 in line 4 are not used in the analysis.)

Line 6 is an extra line for case-control analysis which contains the disease allele frequency followed by the three penetrance values for subjects having 0, 1 or 2 copies of the allele (in that order). These values are used directly for the "user-specified" analysis but for other analyses they are just used to calculate a prevalence for the trait.

The format of the PM datafile is as follows.

[ID] [label] [1a] [1b] [2a] [2b] ...  
or  
[ID] [label] [1] [2] ...

where [ID] and [label] are the individual's ID and case-control status respectively. [1a], [1b], [2a], [2b] are pairs of numbered alleles at each marker separated by spaces. If the marker genotype numbers have been calculated according to the scheme given in the original EH documentation then these can be listed instead of the alleles as single genotype numbers for each marker: [1] [2] etc.

<a name="anchor177658"></a>**EHPLUS and PMPLUS**

The datafile format used by EH is very inefficient if there is a large number of possible multilocus genotypes. This is because most of these genotypes will never actually be observed among the subjects which means that the input file for EH will contain a large number of counts, almost all of which will be zero. Because of this problem we have devised a modified version of EH called EHPLUS which only uses counts of genotypes that are actually observed. Each row of the data file consists of an identifying number for the genotype in question together with counts of the number of times it occurs in cases and controls. The genotype number is calculated in a similar way as for the original EH, and this number is produced automatically by PMPLUS, a program which accepts input files in the same format as PM but which outputs data in the format to be used by EHPLUS. EHPLUS also has a special method for handling memory requirement, which means that it can successfully deal with far more possible haplotypes than EH.

Whether marker-marker or disease-marker analysis is carried out, EHPLUS only requires a single datafile called ehplus.dat. The format for this datafile (which is produced automatically by PMPLUS) is as follows:

The first line contains the number of alleles at each locus and is the same as in eh.dat. Subsequent lines consist of the genotype number, total number of subjects having that genotype, number of cases with the genotype and number of controls with the genotype. For a marker-marker analysis only the first and second columns are needed by EHPLUS.

### <a name="anchor177948"></a>3\. How to run the programs

As usual, the executables EH.EXE and EHPLUS.EXE (under Unix eh and ehplus) need to be on the system path so that they can be run by PM and PMPLUS. PM and PMPLUS take as arguments the names of the parameter file, data file and output file. Optionally a random number seed can also be given. Different random number seeds generate different pseudorandom sequences for permutation analysis. The format for the command line is as follows:

pm parameterfile datafile outputfile [seed]  
or  
pmplus parameterfile datafile outputfile [seed]

If the parameter file and data file are called myfile.par and myfile.dat, respectively, and the random number seed is to be set to be 43, then the command is as follows:

pm myfile.par myfile.dat myfile.out 43

The output will be stored in a file called myfile.out.

The program also outputs to the screen appropriate information from the input files and also outputs the genotype numbers. If desired this information can be redirected to a log file.

### <a name="anchor178242"></a>4\. The output

Running PM or PMPLUS produces a main output file as defined on the command line, a subsidiary output file called mfeh.out (for case-control analysis) and output to the screen. If case-control analysis with permutation is carried out then the file mfeh.out will contain information from the last permuted dataset, not from the original dataset. Thus output in this file is only of interest when a case-control analysis is carried out without permutation.

<a name="anchor178473"></a>**Screen output**

The control information retrieved from the parameter file is output so that any errors can be identified. Subjects with missing genotypes are discarded and the program reports how many subjects remain in the subsequent analysis.

If the second variable at line 3 of the parameter file is 1, then the numerical genotype codes derived from pairs of marker alleles are displayed.

The screen output can be redirected into a log file by specifying "> logfile" as the last command line argument.

<a name="anchor178842"></a>**Model-free case-control analysis**

Five test statistics and their degrees of freedom based on the original sample are provided. They correspond to the user-specified model, Mendelian recessive model, Mendelian dominant model, model-free test and heterogeneity test. If permutation testing is specified then the output also contains empirical p values with their standard errors for each statistic.

For a single case-control analysis, detailed information about the likelihoods for different models can be obtained from file mfeh.out.

<a name="anchor179076"></a>**Marker-marker association analysis**

The test statistics based on the selected markers (as defined in line 4 of parameter file) in the original sample are output first. If there is only one block of markers (the list of indicator variables at line 5 are all 0) then only one statistic is provided, which is the test of association among all selected markers. If two blocks are to be differentiated then two test statistics are provided: 2(l1-l1'-l1'') is a test of "one versus two block association"; and 2(l1'+l1''-l0) is a test of "two block association versus independence".

If permutation testing is specified then the output also contains empirical p-values and their standard errors for these statistics. The standard error is suppressed if the empirical p-value is zero.

### <a name="anchor179349"></a>5\. Examples

<a name="anchor179593"></a>**Standard EH and EHPLUS analyses**

A simple example is provided in the format of EH. Datafiles called eh.dat, case.dat and control.dat are shown as follows:

eh.dat

2 2  
0 4 0  
3 2 1  
0 1 1

control.dat

2 2  
0 2 0  
3 1 1  
0 1 0

case.dat

2 2  
0 2 0  
0 1 0  
0 0 1

To carry out a test for association between markers, with these files in the current directory invoke EH by typing eh at the system prompt, and then answer program queries as follows:

Do you wish to use the case-control sampling option? [N] <ENTER> Enter name of data file [EH.DAT] <ENTER>  
Enter name of output file. [EH.OUT] <ENTER>

In this case we simply type <ENTER> three times to use the default options. The output file eh.out should be produced containing the following information.

<font face="Courier New">                          #param Ln(L) Chi-square  
-------------------------------------------------  
H0: No Association               2 -23.84 0.00  
H1: Allelic Associations Allowed 3 -23.75 0.18</font>

Twice the difference between log likelihoods under H1 and H0, 0.18, provides a test for allelic association. In the present case the difference is negligible, which is hardly surprising given such small dataset.

To carry out a case-control analysis EH will use the datafiles case.dat and control.dat rather than eh.dat. Run EH and answer the queries as follows:

Do you wish to use the case-control sampling option? [N] y <ENTER>  
Enter name of control data file [CONTROL.DAT] <ENTER>  
Enter name of case data file [CASE.DAT] <ENTER>  
Enter name of output file [EH.OUT] <ENTER>  
File eh.out exists. Overwrite it? [y/n]  
y <ENTER>

The program continues to ask for disease gene frequencies and penetrances, and we enter 0.001 and 0.05 0.2 0.8\. Output file eh.out is as follows:

<font face="Courier New">                           #param Ln(L) Chi-square  
--------------------------------------------------  
H0: No Association                   2 -23.84 0.00  
H1: Markers Asso., Indep. of Disease 3 -23.75 0.18  
H2: Markers and Disease Associated   6 -23.74 0.20</font>

Again there is negligible evidence for marker-marker association or for disease-marker association, with chi-squared statistics of 0.18 and 0.20 respectively.

In fact information contained in eh.dat, case.dat and control.dat can be combined into a single file. At the first line we still keep number of alleles of all loci while in the following lines we create a column of genotype identifier attached with number of subjects associated with this identifier in the following columns. Cells of zero counts in eh.dat would imply zero counts in case.dat and control.dat so that they would all be omitted. Here we supply this file called ehplus.dat, which contains the following lines.

2 2  
2 4 2 2  
4 3 0 3  
5 2 1 1  
6 1 0 1  
8 1 0 1  
9 1 1 0

The genotype identifiers now correspond to the 2-locus genotype actually observed in the sample, and lines with identifiers 1, 3, 7 do not appear since no subject has any of these combinations. For marker-marker analysis the first two columns is sufficient. File ehplus.dat is used by EHPLUS; its usage is about the same as EH.

The example files eh.dat, case.dat and control.dat above can actually be generated by PM from data file example.dat and parameter file example.par, and avoids the need to manually prepare them. So is the file ehplus.dat, which is generated from PMPLUS.

<a name="anchor180163"></a>**PM analyses**

File example.dat holds individual ID, affection status 2 marker phenotypes for 12 subjects as shown below.

1003.0 1 1 2 1 2  
1003.1 0 1 2 1 2  
1003.2 0 1 2 1 1  
1005.0 1 1 1 1 2  
1005.1 0 1 1 1 2  
1005.2 0 1 2 1 1  
1006.0 1 2 2 2 2  
1006.1 0 1 2 2 2  
1006.2 0 2 2 1 2  
1007.0 1 1 1 1 2  
1007.1 0 1 1 1 2  
1007.2 0 1 2 1 1

The first column is individual ID, followed by in the second column specifying individual's affection status (0=control, 1=case), then 2 columns of marker genotypes are written as pairs of alleles.

File example.par contains the following lines:

2 1 0 0 << nloci, case/control, label permutation, # permutations  
2 2 << a list of alleles  
0 0 << genotype/allele, screen output  
1 1 << marker selection status  
0 0 << marker permutation status  
0.001 0.05 0.2 0.8 << disease model for case-control design

This describes the fact that example.dat contains information on two biallelic markers and that a case-control analysis be performed.

To perform the actual analysis we issue the command:

pm example.par example.dat example.out

The screen output is as follows:

Permutation & Model-free analysis PM 1.0 15-JUN-1998

Maximum number of loci = 30  
Maximum number of individuals = 800

Number of loci in this analysis = 2  
Permutation procedure will not be invoked  
Number of alleles at these loci and their  
selection/permutation statuses [1=yes,0=no]:  
locus 1: alleles= 2 selection= 1 permutation= 0  
locus 2: alleles= 2 selection= 1 permutation= 0  
The disease model(q,f0,f1,f2) specified: 0.0010 0.0500 0.2000 0.8000 amounts to a population disease prevalence of 0.0503  
There are 4 cases out of 12 individuals  
Random number seed = 3000

Analysing observed data...done

Output has been written to example.out

PM has repeatedly run EH using different disease model specifications and generated the output file example.out with the following lines:

Chi-squared statistic for user-specified model = 0.02, df=3, p=0.9992  
Chi-squared statistic for recessive model = 3.92, df=3, p=0.2703  
Chi-squared statistic for dominant model = 3.16, df=3, p=0.3676  
Chi-squared statistic for model-free analysis = 3.92, df=4, p=0.4169  
Chi-squared statistic for heterogeneity model = 3.74, df=3, p=0.2910

Now file mfeh.out is also produced which contains information about the models used to produce the model-free statistic:

  #            q         f0         f1         f2         K Chi-square DF        P  
  0 0.22428 0.0000 0.0000 1.0000 0.0503            3.92 4 0.4169  
  1 0.22428 0.0101 0.0101 0.8101 0.0503            3.36 4 0.4995  
  2 0.22428 0.0201 0.0201 0.6201 0.0503            2.86 4 0.5815  
  3 0.22428 0.0302 0.0302 0.4302 0.0503            2.60 4 0.6268  
  4 0.22428 0.0402 0.0402 0.2402 0.0503            2.20 4 0.6990  
  5 0.50000 0.0503 0.0503 0.0503 0.0503            0.00 4 1.0000  
  6 0.02547 0.0000 1.0000 1.0000 0.0503            3.16 4 0.5314  
  7 0.02547 0.0101 0.8101 0.8101 0.0503            2.54 4 0.6375  
  8 0.02547 0.0201 0.6201 0.6201 0.0503            1.88 4 0.7578  
  9 0.02547 0.0302 0.4302 0.4302 0.0503            1.24 4 0.8715  
10 0.02547 0.0402 0.2402 0.2402 0.0503            0.62 4 0.9608

An additional degree of freedom reflects the fact that the chi-squared is maximised over penetrance.

<a name="anchor180651"></a>**Marker-marker analysis**

File mm.dat contains data from a sample of 771 individuals with full or partial genotypes of 6 markers (Dr Tao Li, personal  
communications), and part of which is shown below.

001.0 0 0 0 0 0 0 0 0 0 0 0 0 0  
002.1 0 1 2 1 2 0 0 0 0 0 0 0 0  
003.2 0 1 2 1 1 0 0 0 0 0 0 0 0  
004.0 0 1 1 0 0 2 2 1 2 1 2 6 8  
005.1 0 1 1 0 0 2 2 1 2 1 2 8 8  
006.2 0 1 2 1 1 1 2 1 2 1 2 6 6  
... ...

769.0 0 0 0 0 0 0 0 0 0 0 0 0 0  
770.1 0 0 0 0 0 0 0 0 0 0 0 0 0  
771.2 0 0 0 0 0 0 0 0 0 0 0 0 0

Contained again are subject ID at column 1, control indicators at column 2, and columns 3-14 pairs of alleles for each of the six markers.

A parameter file called mm.par contains the following lines:

6 0 0 5 << nloci, case/control, label permutation, # permutations  
2 2 2 2 2 11 << a list of alleles  
0 0 << allele/genotype screen output  
1 1 0 1 1 0 << marker selection status  
0 0 0 1 1 0 << marker permutation status  
0.001 0.05 0.2 0.8 << disease model for case-control design

Line 1 says there are 6 markers in the data file. This is not a case-control analysis but a marker-marker one, and therefore no affection status is to be permuted. Nevertheless we will perform permutations of the markers for just 5 permutations.

Line 2 indicates these six markers have 2, 2, 2, 2, 2, and 11 alleles, respectively.

Line 3 specifies that the genotypes in the datafile provided as pairs of alleles and that the converted genotypes will not be shown on the screen.

Line 4 selects the first, the second, the fourth and the fifth markers for analysis.

Line 5 differentiates the four selected markers as a block of the first and second and a block of the fourth and fifth. We intend to examine association of these two groups of markers relative to each other.

Line 6 is only utilised in disease-marker analysis and thus ignored.

Now PM can be invoked with command:

pm mm.par mm.dat mm.out

which generates the following screen output:

Permutation & Model-free analysis PM+ 1.0 15-JUN-1998

Maximum number of loci = 30  
Maximum number of individuals = 800

Number of loci in this analysis = 6  
Permutation procedure will be invoked 5 times  
Number of alleles at these loci and their  
selection/permutation statuses [1=yes,0=no]:  
locus 1: alleles= 2 selection= 1 permutation= 0  
locus 2: alleles= 2 selection= 1 permutation= 0  
locus 3: alleles= 2 selection= 0 permutation= 0  
locus 4: alleles= 2 selection= 1 permutation= 1  
locus 5: alleles= 2 selection= 1 permutation= 1  
locus 6: alleles=11 selection= 0 permutation= 0  
blocks 1 and 2 have 2, 2 loci  
There are 0 cases out of 459 individuals  
312 records with partial information have been left out  
Random number seed = 3000

Analysing observed data...done  
Running permutation 5 out of 5 ... done

Output has been written to mm.out

The output file mm.out is shown as follows

Chi-squared statistic for one block association = 299.06, df= 11, p= 0.0000  
Chi-squared statistic for one versus two block association = 245.82, df= 9, p= 0.0000  
Chi-squared statistic for two block versus no association = 53.24, df= 2, p= 0.0000

The random number seed is 3000  
The number of replicates is 5

One block association chi-squared statistic (299.06) was reached 0 times One block versus two block association chi-squared statistic (245.82) was reached 0 times Two block versus no association chi-squared statistic (53.24) was reached 0 times

The empirical p-values for these statistics are as follows:  
One block association: P-value = 0.0000  
One versus two block association: P-value = 0.0000  
Two block versus no association: P-value = 0.0000

The first statistic tests some or all markers being associated with each other with degrees of freedom (16-1)-4*(2-1)=11\. The second statistic tests two blocks being associated with each other versus these with association only occurring within each block and with degrees of freedom (16-1)-(4-1)-(4-1)=9\. The last statistic tests no association against association within blocks with degrees of freedom (4-1)+(4-1)-4*(2-1)=2\. All results are highly significant according to the chi-squared distribution and of the five random permutations performed none produces the chi-squared statistic as large or larger than that from the real data, and the "empirical p value" for each is 0\. However to gain any sort of realistic assessment of the true p value in this case one would probably need to perform at least a thousand permutations (to estimate a maximum p value of 0.003), and many thousand if greater accuracy were required.

<a name="anchor181458"></a>**Disease-marker analysis**

The final example dataset comes from a study of schizophrenia and the HLA markers DRB, DQA, and DQB on chromosome 6 (Dr Padraig Wright, personal communications). Parameter file hla.par contains the following lines,

3 1 1 5 << nloci, case/control, label permutation, # permutations  
13 6 5 << a list of alleles  
0 0 << allele/genotype, screen output  
1 0 0 << marker selection status  
0 0 0 << marker permutation status  
0.001 0.05 0.2 0.8 << disease model for case-control design

It should be relatively easier to catch information by line:

Line 1 reveals that there should be three markers presented in the data file of cases and controls, and the affection status will be permuted 5 times.  

Line 2 lists the numbers of alleles of these markers to be 13, 6, and 5, respectively.

Line 3 says the marker genotypes should be given as pairs of alleles. The converted genotype numbers will not be shown on the screen.  
Line 4 indicates that the first marker only is to be used in this particular analysis.

Line 5 is just a list of zeros since we are not permuting markers in a case-control study.

Line 6 contains a disease model. The disease allele frequency is 0.001, and the penetrance vector is (0.05, 0.2, 0.8) resulting in a population disease prevalence (Kp) of 0.05.

The head and tail of hla.dat are shown as follows:

    1 1  2  10 5 1 1 5  
    2 1  7    7 1 4 4 4  
    3 1  1    4 1 2 4 1  
    4 1  2  12 5 1 1 5  
    5 1 12 12 1 3 5 2  
... ...  
270 0  2    4 2 5 2 1  
271 0 12 12 1 1 5 5

Column 1 shows individual IDs numbered consecutively from 1 to 271.

Column 2 gives affection status of the subjects, 1 for case and 0 for control so there are 94 cases and 177 controls.

Columns 3-8 list the observed alleles for the three markers.

Now we can perform the analysis with command:

pmplus hla.par hla.dat hla.out

The screen output is obtained as follows.

Permutation & Model-free analysis PM+ 1.0 15-JUN-1998

Maximum number of loci = 30  
Maximum number of individuals = 800

Number of loci in this analysis = 3  
Permutation procedure will be invoked 5 times  
Number of alleles at these loci and their  
selection/permutation statuses [1=yes,0=no]:  
locus 1: alleles=13 selection= 1 permutation= 0  
locus 2: alleles= 6 selection= 0 permutation= 0  
locus 3: alleles= 5 selection= 0 permutation= 0  
The disease model(q,f0,f1,f2) specified: 0.0010 0.0500 0.2000 0.8000 amounts to a population disease prevalence of 0.0503  
There are 93 cases out of 270 individuals  
1 records with partial information have been left out  
Random number seed = 3000

Analysing observed data...done

Output has been written to hla.out

It reiterates we are doing a case-control analysis with the first marker, including a permutation analysis with 5 permutations. The output file hla.out contains the following information:

Chi-squared statistic for user-specified model = 1.74, df=12, p=0.9997  
Chi-squared statistic for recessive model = 18.34, df=12, p=0.1058  
Chi-squared statistic for dominant model = 15.46, df=12, p=0.2172  
Chi-squared statistic for model-free analysis = 21.72, df=13, p=0.0599  
Chi-squared statistic for heterogeneity model = 19.42, df=12, p=0.0789

The random number seed is 3000  
The number of replicates is 5

User-specified model chi-squared statistic (1.74) was reached 0 times Recessive model chi-squared statistic (18.34) was reached 2 times Dominant model chi-squared statistic (15.46) was reached 3 times Model-free chi-squared statistic (21.72) was reached 2 times Heterogeneity model chi-squared statistic (19.42) was reached 2 times

Empirical p-values for these statistics are as follows:

T1 - User specified model: P-value = 0.0000  
T2 - Mendelian recessive model: P-value = 0.4000  
T3 - Mendelian dominant model: P-value = 0.6000  
T4 - Model-free analysis: P-value = 0.4000  
T5 - Heterogeneity model: P-value = 0.4000

In this analysis the asymptotic p values approach significance for the model-free and heterogeneity analyses. Theoretically, one can get more reliable estimates of the significance by looking at the empirical p values which consist of the proportions of random permutations which produce the same or higher chi-squared statistics than those obtained from the real data. However as only five permutations have been performed the estimation of the empirical p value is wildly inaccurate. In this situation one would begin by performing perhaps 100 permutations to see if any tests appeared significant and then one would go on to perform a larger number if greater accuracy in estimating the p values were required.

### <a name="anchor182061"></a>6\. Problems

From our experience problems are most likely due to EH, when the number of loci, the number of alleles or the number of possible haplotypes exceeds the limit of the program. To see if this might be so, check the data files generated from PM (eh.dat, case.dat, control.dat) and run a standard EH analysis using these files. For reference those from real data are kept in  
eh.sav/case.sav/control.sav, eh1.sav/case1.sav/control1.sav, and eh2.sav/case2.sav/control2.sav, for block 1 markers and block 2 markers, if any. With PMPLUS and EHPLUS, ehplus.sav, ehplus1.sav and ehplus2.sav, would be sufficient.

When the number of loci are large (say over 10-15 loci), EH or EHPLUS could be very slow. Therefore in version 1.2 PMPLUS call to EHPLUS is replaced by FEHP, a program which uses faster algorithm described in Zhao and Sham (2002). This change is seamless for most users, but its behaviour could be checked analogously to EHPLUS.

### <a name="anchor182295"></a>7\. Compilation

<a name="anchor182542"></a>**C source**

The source code is contained in program eh.c, pm.c, ehplus.c and pmplus.c. Each program can be compiled separately, linking with the standard maths library when necessary, e.g.:

gcc pm.c -lm -o pm (GNU C compiler under Unix)

sc -mn pm.c (Symantec C compiler under MSDOS)

Simple makefiles are provided for Sun and DEC Alpha. To use them type make -f Makefile.sun or make -f Makefile.osf when appropriate to produce all the executables.

<a name="anchor182885"></a>**Program constants**

If desired these constants can be changed.

The following constants in PM define respectively the maximum number of loci and maximum number of individuals in an analysis:

#define MAX_LOC  30  
#define MAX_IND  800

Constants for eh.c and ehplus.c are also listed as follows.

#define maxalleles          15 /* max # of alleles at a locus */  
#define maxloci                5 /* max # of loci in a analysis */  
#define maxhap             800 /* max # of possible haplotypes */  
#define maxposcom 85995L /* max # of possible genotype combinations */

### <a name="anchor183284"></a>8\. References

Please use the following references when reporting results from PM.

Xie, X. and J. Ott (1993): Testing linkage disequilibrium between a disease gene and marker loci. Am J Hum Genet 53:1107.

Zhao, J. H., Curtis, D. and Sham, P. C. (2000). Model-free analysis and permutation test for allelic associations. Hum Hered 50:133-139.

The original EH is written in Pascal and available from  
http://linkage.rockefeller.edu.

### <a name="anchor183683"></a>9\. Acknowledgements

We would like to thank authors of the original EH for their work, Dr Padraig Wright for providing the HLA data, Drs Tao Li and Maria Arranz and other colleagues at the Institute of Psychiatry for their continuous interest in case-control association analyses. This work is supported by Wellcome Trust grant No 043279.

### <a name="anchor184045"></a>10\. Literature

Curtis, D. and P. C. Sham (1995). Model-free linkage analysis using likelihoods. Am J Hum Genet 57:703-716.

Zhao, J. H. and Sham, P. C. (2002) Faster allelic association analysis using unrelated subjects. Human Heredity, 53:36-41.

### <a name="anchor184384"></a>11\. Addresses

Please feel free to contact us for any problems and suggestions at the following addresses.

Jing Hua Zhao, Pak Chung Sham  
Section of Genetic Epidemiology and Biostatistics  
Department of Psychiatry  
KCL Institute of Psychiatry  
De Crespigny Park  
London SE5 8AF  
j.zhao@iop.kcl.ac.uk, p.sham@iop.kcl.ac.uk

Dave Curtis  
Depart of Adult Psychiatry  
3rd Floor, Outpatient Building  
Royal London Hospital  
Whitechapel  
London E1 1BB  
dcurtis@hgmp.mrc.ac.uk

<center>Last modified: 14/1/2000 by: [Jing Hua Zhao](mailto:j.zhao@iop.kcl.ac.uk)</center>
