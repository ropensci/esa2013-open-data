
# Acquiring data from the web 

For this tutorial we will combine data from three separate data repositories. 
* Open Fisheries - contains landings data.
* Taxize - 
* Global Biodiversity Information Facility (gbif)


First install some packages


```r
install.packages("rgbif")
install.packages("taxize_")
install.packages("rfisheries")
```



```r
library(rgbif)
library(taxize)
```



```r
library(rfisheries)
# first we acquire a list of species in the database
species <- species_codes()
```



```
## 
```




```r
head(species)
```

```
##          scientific_name   taxocode a3_code isscaap
## 1     Petromyzon marinus 1020100101     LAU      25
## 2   Lampetra fluviatilis 1020100201     LAR      25
## 3    Lampetra tridentata 1020100202     LAO      25
## 4 Ichthyomyzon unicuspis 1020100401     LAY      25
## 5    Eudontomyzon mariae 1020100501     LAF      25
## 6      Geotria australis 1020100701     LAE      25
##              english_name
## 1             Sea lamprey
## 2           River lamprey
## 3         Pacific lamprey
## 4          Silver lamprey
## 5 Ukrainian brook lamprey
## 6         Pouched lamprey
```

```r
# Rather than look up data for every single one in this dataset, we'll
# pick a random sample of 10
species <- species[sample(nrow(species), 10), ]
```


# Taxize



```r
# look up code
taxon_identifiers <- get_tsn(species[, 1])
```

```
## 
## Retrieving data for species ' Ctenocidaris nutrix '
## 
## Retrieving data for species ' Palaemonetes kadiakensis '
## 
## Retrieving data for species ' Trachycardium consors '
## 
## Retrieving data for species ' Encrasicholina devisi '
## 
## Retrieving data for species ' Halieutichthys aculeatus '
## 
## Retrieving data for species ' Micropogonias altipinnis '
## 
## Retrieving data for species ' Palatogobius paradoxus '
## 
## Retrieving data for species ' Aulohalaelurus kanakorum '
## 
## Retrieving data for species ' Barbucca diabolica '
## 
## Retrieving data for species ' Grahamichthys radiata '
```

```
## Warning: the condition has length > 1 and only the first element will be
## used
```

```r
# then taxonomic info
classification_data <- classification(taxon_identifiers)
```

```
## http://www.itis.gov/ITISWebService/services/ITISService/getFullHierarchyFromTSN?tsn=96396
## http://www.itis.gov/ITISWebService/services/ITISService/getFullHierarchyFromTSN?tsn=551382
## http://www.itis.gov/ITISWebService/services/ITISService/getFullHierarchyFromTSN?tsn=164594
## http://www.itis.gov/ITISWebService/services/ITISService/getFullHierarchyFromTSN?tsn=646637
## http://www.itis.gov/ITISWebService/services/ITISService/getFullHierarchyFromTSN?tsn=171969
## http://www.itis.gov/ITISWebService/services/ITISService/getFullHierarchyFromTSN?tsn=687916
## http://www.itis.gov/ITISWebService/services/ITISService/getFullHierarchyFromTSN?tsn=637467
```

# gbif

```r
# then locations
locations <- llply(species[, 1], occurrencelist_many, .progress = "text")
```

```
## 
```
