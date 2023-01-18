---
layout: tutorial_hands_on

title: Obis marine indicators
zenodo_link: ''
questions:
objectives:
- Calculating and vizualizing biodiversity indicators
time_estimation: 1H
key_points:
- Marine data
- Diversity indicators
contributors:
- Marie59

---


# Introduction

OBIS is a global open-access data and information clearing-house on marine biodiversity for science, conservation and sustainable development. In order to visualize their marine data OBIS created the package obisindicators.

Obisindicators is an R library developed during the 2022 IOOS Code Sprint. The purpose was to create an ES50 diversity index within hexagonal grids following the diversity indicators notebook by Pieter Provoost linked above. The package includes several examples, limited to 1M occurrences, that demonstrate uses of the package.
This tutorial will guide you on getting obis marine data and processing them in order to calculate and visualize multiple indicators. 

This tool for obisindicators is composed of 5 indicators : Number of record, Shannon, Simpson, Es50 and Hill which will be explained in more details later on.

In this tutorial, we'll be working on obis data extracted from .  First those data will be prepared. 


> ### Agenda
>
> In this tutorial, we will cover:
>
> 1. TOC
> {:toc}
>
{: .agenda}

## Upload of the data

This first step consist of downloading and properly prepare the data to use it in Sentinel 2 toolsuite.

> <hands-on-title>Hands-on: Data upload</hands-on-title>
>
> 1. Create a new history for this tutorial and give it a name (example: “Obisindicators tutorial”) for you to find it again later if needed.
>
>    {% snippet faqs/galaxy/histories_create_new.md box_type="none" %}
>
>
> 2. Download the files from [Scihub]({{ https://scihub.copernicus.eu/dhus/#/home }}) 
>
>    You will have to to create an account for either of these platform. 
>    Select Sentinel 2 and choose the Product type "S2MSI2A".
>
>    ![Scihub portal](images/Scihub.png "Scihub portal")
>
>    This an example of the Copernicus portal, Scihub. You need to download a zip folder. Keep it that way. 
>
> 3. Upload the zip folder
>
>     > <tip-title>Tip: Importing data from your computer</tip-title>
>     >
>     > * Open the Galaxy Upload Manager {% icon galaxy-upload %}
>     > * Select **Choose local files**
>     > * Browse in your computer and get the downloaded zip folder
>     >
>     > * Press **Start**
>     {: .tip}
>
> 4. Rename the datasets “sentinel_2_data.zip" for example and preview your dataset
>
>
{: .hands_on}

# obisinticators tool
>
>   Data ready
> <hand-on-title>Hands-on: Obis</hand-on-title>H
>
> 1. {% tool [Ocean biodiversity indicators](toolshed.g2.bx.psu.edu/repos/ecology/obisindicators/obisindicators/0.0.1) %} with the following parameters:
>    - {% icon param-file %} *"Input data"*: `sentinel_2_data.zip` (Input dataset)
>    - {% icon param-select %} *"Where does your data come from ?"*: 'From Scihub or Peps'
>
> 2. Click on **Execute** 
> 3. You will see 5 outputs appear on the history pannel. one for each of the indicators
>
{: .hands_on}

## Shannon

The Shannon index expresses the uncertainty associated with the prediction of the species the next sampled individual belongs to. It assumes that individuals are randomly sampled from an infinitely large community, and that all species are represented in the sample.

Warning: OBIS uses records as a proxy for individuals and sampling is generally not random, the community is not infinitely large and not all species are represented in the sample.

The Shannon diversity index, also known as the Shannon-Wiener diversity index, is defined in OBIS as the sum over all species of -fi*log(fi) with fi defined as n/ni with n as the total number of records in the raster cell and ni as the total number of records for the ith-species in the raster cell.
![Preprocessing outputs](images/preproc.png "Preprocessing outputs")


> <question-title>Question</question-title>
>
> 1. What are the files you are interested in for the folowing tools ?
>
> > <solution-title>Solution</solution-title>
> >
> > 1. The 2 files in the **Refelectance** folder that finish by "_Refl" and "_Refl.hdr"
> >
> {: .solution}
>
{: .question}

## Simpson
The measure equals the probability that two entities taken at random from the dataset of interest represent the same type. It equals:

where is richness (the total number of types in the dataset) and is the proportional abundances of the types of interest.

Simpson’s index expresses the probability that any two individuals drawn at random from an infinitely large community belong to the same species. Note that small values are obtained in cells of high diversity and large values in cells of low diversity. This counterintuitive behavior is adressed with the Hill 2 number, which is the inverse of the Simpson index.

The Simpson biodiversity index is defined in OBIS as the sum over all species of (ni/n)^2 with n as the total number of records in the cell and ni the total number of records for the ith species.

Warning: The Simpson index has the same assumptions as the Shannon index.

## ES50

The expected number of marine species in a random sample of 50 individuals (records) is an indicator on marine biodiversity richness.

The ES50 is defined in OBIS as the sum(esi) over all species of the following per species calculation:

    when n - ni >= 50 (with n as the total number of records in the cell and ni the total number of records for the ith-species)
        esi = 1 - exp(lngamma(n-ni+1) + lngamma(n-50+1) - lngamma(n-ni-50+1) - lngamma(n+1))
    when n >= 50
        esi = 1
    else
        esi = NULL

Warning: ES50 assumes that individuals are randomly distributed, the sample size is sufficiently large, the samples are taxonomically similar, and that all of the samples have been taken in the same manner.

## Maxp

## Hill

**Hill 1**

The Hill biodiversity index accounts for species’ relative abundance (number of records in OBIS) and Hill1 can be roughly interpreted as the number of species with “typical” abundances, and is a commonly used indicator for marine biodiversity richness. It is defined as:

Warning: The Simpson index has the same assumptions as the Shannon index.

**Hill 2**

The Hill biodiversity index accounts for species’ relative abundance (number of records in OBIS) and discounts rare species, so Hill2 can be interpreted as the equivalent to the number of more dominant species and so is less sensitive to sample size than Hill1. The Hill index is a commonly used indicator for marine biodiversity richness. It is defined as:

Warning: The Simpson index has the same assumptions as the Shannon index.


# Conclusion

You are now all set to use your obis data in order to do a diversity analysis.

