---
layout: default
title: rgbif
---



Here's the `rgbif` screencast!

Follow along below in your favorite R client.  I'll be using the RStudio IDE.

<iframe src="//player.vimeo.com/video/127119010" width="600" height="381" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>


rgbif screencast
==========

## Intro

> Hi, I'm Scott Chamberlain

> In this screencast, I'll do a brief intoduction to the `rgbif` package - an `R` client for the Global Biodiversity Information Facility (GBIF)

> rgbif allows you to get species occurrence data from GBIF

![gbif_site](../public/images/gbif.png)

## Installation and load


```r
install.packages("rgbif")
```


```r
library("rgbif")
```

## Get number of occurrences

> Search by type of record, all observational in this case


```r
occ_count(basisOfRecord = 'OBSERVATION')
#> [1] 114413835
```

> Records for **Puma concolor** with lat/long data (georeferenced) only. Note that `hasCoordinate` in `occ_search()` is the same as `georeferenced` in `occ_count()`.


```r
occ_count(taxonKey = 2435099, georeferenced = TRUE)
#> [1] 2847
```

> All georeferenced records in GBIF


```r
occ_count(georeferenced = TRUE)
#> [1] 465184716
```

> Records from Denmark only


```r
(denmark_code <- isocodes[grep("Denmark", isocodes$name), "code"])
#> [1] "DK"
occ_count(country = denmark_code)
#> [1] 9614891
```

## Search for taxonomic names

> Four taxononomic names functions

* `name_backbone()`
* `name_lookup()`
* `name_usage()`
* `name_suggest()`

> `name_backbone()` searches against the GBIF backbone taxonomy


```r
name_backbone(name = 'Helianthus', rank = 'genus', kingdom = 'plants')
#> $usageKey
#> [1] 3119134
#> 
#> $scientificName
#> [1] "Helianthus L."
#> 
#> $canonicalName
#> [1] "Helianthus"
#> 
#> $rank
#> [1] "GENUS"
#> 
#> $status
#> [1] "ACCEPTED"
#> 
#> $confidence
#> [1] 97
#> 
#> $matchType
#> [1] "EXACT"
#> 
#> $kingdom
#> [1] "Plantae"
#> 
#> $phylum
#> [1] "Magnoliophyta"
#> 
#> $order
#> [1] "Asterales"
#> 
#> $family
#> [1] "Asteraceae"
#> 
#> $genus
#> [1] "Helianthus"
#> 
#> $kingdomKey
#> [1] 6
#> 
#> $phylumKey
#> [1] 49
#> 
#> $classKey
#> [1] 220
#> 
#> $orderKey
#> [1] 414
#> 
#> $familyKey
#> [1] 3065
#> 
#> $genusKey
#> [1] 3119134
#> 
#> $synonym
#> [1] FALSE
#> 
#> $class
#> [1] "Magnoliopsida"
```

> `name_lookup()` does full text search of name usages covering the scientific and vernacular name, the species description, distribution and the entire classification across all name usages of all or some checklists


```r
out <- name_lookup(query = 'mammalia')
```


```r
names(out)
#> [1] "meta"        "data"        "facets"      "hierarchies" "names"
```


```r
out$meta
#>   offset limit endOfRecords  count
#> 1      0   100        FALSE 119432
```


```r
head(out$data, n = 2)
#>         key          scientificName                           datasetKey
#> 1 125798198                Mammalia 16c3f9cb-4b19-4553-ac8e-ebb90003aa02
#> 2 116665331 Mammalia Linnaeus, 1758 cbb6498e-8927-405a-916b-576d00a6289b
#>   nubKey parentKey   parent   phylum phylumKey  classKey canonicalName
#> 1    359 137006861 Chordata Chordata 137006861 125798198      Mammalia
#> 2    359 143035196 Chordata Chordata 143035196 116665331      Mammalia
#>       authorship   nameType  rank numDescendants numOccurrences taxonID
#> 1                WELLFORMED CLASS              2              0 2621711
#> 2 Linnaeus, 1758 WELLFORMED CLASS           1193              0   18838
#>   extinct habitats nomenclaturalStatus threatStatuses synonym    class
#> 1    TRUE     <NA>                  NA             NA   FALSE Mammalia
#> 2      NA     <NA>                  NA             NA   FALSE Mammalia
#>    kingdom kingdomKey publishedIn accordingTo taxonomicStatus order
#> 1     <NA>         NA        <NA>        <NA>            <NA>  <NA>
#> 2 Animalia  116630539        <NA>        <NA>            <NA>  <NA>
#>   orderKey species speciesKey acceptedKey accepted family familyKey genus
#> 1       NA    <NA>         NA          NA     <NA>   <NA>        NA  <NA>
#> 2       NA    <NA>         NA          NA     <NA>   <NA>        NA  <NA>
#>   genusKey
#> 1       NA
#> 2       NA
```

## Search for occurrences

> By default `occ_search()` returns a `dplyr` like output summary in which the data printed expands based on how much data is returned, and the size of your window. You can search in a variety of ways, including by scientific name:


```r
occ_search(scientificName = "Ursus americanus", limit = 20)
#> Records found [7202] 
#> Records returned [20] 
#> No. unique hierarchies [1] 
#> No. media records [19] 
#> Args [scientificName=Ursus americanus, limit=20, offset=0, fields=all] 
#> First 10 rows of data
#> 
#>                name        key decimalLatitude decimalLongitude
#> 1  Ursus americanus 1065588899        35.73304        -82.42028
#> 2  Ursus americanus 1065590124        38.36662        -79.68283
#> 3  Ursus americanus 1065611122        43.94883        -72.77432
#> 4  Ursus americanus  786016809              NA               NA
#> 5  Ursus americanus  891034709        29.23322       -103.29468
#> 6  Ursus americanus  891041363        29.28284       -103.28908
#> 7  Ursus americanus 1024328693        34.20990       -118.14681
#> 8  Ursus americanus  891045574        43.73511        -72.52534
#> 9  Ursus americanus 1050834838        33.11070       -107.70675
#> 10 Ursus americanus  891056344        29.27444       -103.31536
#> ..              ...        ...             ...              ...
#> Variables not shown: issues (chr), datasetKey (chr), publishingOrgKey
#>      (chr), publishingCountry (chr), protocol (chr), lastCrawled (chr),
#>      lastParsed (chr), extensions (chr), basisOfRecord (chr), taxonKey
#>      (int), kingdomKey (int), phylumKey (int), classKey (int), orderKey
#>      (int), familyKey (int), genusKey (int), speciesKey (int),
#>      scientificName (chr), kingdom (chr), phylum (chr), order (chr),
#>      family (chr), genus (chr), species (chr), genericName (chr),
#>      specificEpithet (chr), taxonRank (chr), dateIdentified (chr), year
#>      (int), month (int), day (int), eventDate (chr), modified (chr),
#>      lastInterpreted (chr), references (chr), identifiers (chr), facts
#>      (chr), relations (chr), geodeticDatum (chr), class (chr), countryCode
#>      (chr), country (chr), verbatimEventDate (chr), verbatimLocality
#>      (chr), http...unknown.org.occurrenceDetails (chr), rights (chr),
#>      rightsHolder (chr), occurrenceID (chr), collectionCode (chr), taxonID
#>      (chr), gbifID (chr), institutionCode (chr), catalogNumber (chr),
#>      datasetName (chr), recordedBy (chr), eventTime (chr), identifier
#>      (chr), identificationID (chr), occurrenceRemarks (chr), sex (chr),
#>      establishmentMeans (chr), preparations (chr), recordNumber (chr),
#>      nomenclaturalCode (chr), higherClassification (chr), type (chr),
#>      vernacularName (chr), accessRights (chr), occurrenceStatus (chr),
#>      language (chr), disposition (chr), infraspecificEpithet (chr),
#>      lifeStage (chr), elevation (dbl), elevationAccuracy (dbl), continent
#>      (chr), stateProvince (chr), georeferencedDate (chr), institutionID
#>      (chr), higherGeography (chr), identifiedBy (chr), georeferenceSources
#>      (chr), identificationVerificationStatus (chr), samplingProtocol
#>      (chr), endDayOfYear (chr), otherCatalogNumbers (chr),
#>      georeferenceVerificationStatus (chr), individualID (chr),
#>      locationAccordingTo (chr), previousIdentifications (chr),
#>      verbatimCoordinateSystem (chr), georeferenceProtocol (chr),
#>      identificationQualifier (chr), dynamicProperties (chr), county (chr),
#>      locality (chr), georeferencedBy (chr)
```

> Or to be more precise, you can search for names first, make sure you have the right name, then pass the GBIF key to the `occ_search()` function:


```r
key <- name_suggest(q = 'Helianthus annuus', rank = 'species')$key[1]
occ_search(taxonKey = key, limit = 20)
#> Records found [20363] 
#> Records returned [20] 
#> No. unique hierarchies [1] 
#> No. media records [9] 
#> Args [taxonKey=3119195, limit=20, offset=0, fields=all] 
#> First 10 rows of data
#> 
#>                 name        key decimalLatitude decimalLongitude
#> 1  Helianthus annuus  922042404        -3.28140         37.52415
#> 2  Helianthus annuus  899948224         1.27890        103.79930
#> 3  Helianthus annuus  891052261        24.82589        -99.58411
#> 4  Helianthus annuus 1038317691       -43.52777        172.62544
#> 5  Helianthus annuus  922039507        50.31402          8.52341
#> 6  Helianthus annuus  922044332        21.27114         40.41424
#> 7  Helianthus annuus  899970378        32.54041       -117.08731
#> 8  Helianthus annuus 1038322459       -43.07327        172.68473
#> 9  Helianthus annuus  998785009        44.10879          4.66839
#> 10 Helianthus annuus  899969160        24.82901        -99.58257
#> ..               ...        ...             ...              ...
#> Variables not shown: issues (chr), datasetKey (chr), publishingOrgKey
#>      (chr), publishingCountry (chr), protocol (chr), lastCrawled (chr),
#>      lastParsed (chr), extensions (chr), basisOfRecord (chr), taxonKey
#>      (int), kingdomKey (int), phylumKey (int), classKey (int), orderKey
#>      (int), familyKey (int), genusKey (int), speciesKey (int),
#>      scientificName (chr), kingdom (chr), phylum (chr), order (chr),
#>      family (chr), genus (chr), species (chr), genericName (chr),
#>      specificEpithet (chr), taxonRank (chr), year (int), month (int), day
#>      (int), eventDate (chr), lastInterpreted (chr), identifiers (chr),
#>      facts (chr), relations (chr), geodeticDatum (chr), class (chr),
#>      countryCode (chr), country (chr), gbifID (chr), institutionCode
#>      (chr), catalogNumber (chr), recordedBy (chr), locality (chr),
#>      collectionCode (chr), dateIdentified (chr), modified (chr),
#>      references (chr), verbatimEventDate (chr), verbatimLocality (chr),
#>      http...unknown.org.occurrenceDetails (chr), rights (chr),
#>      rightsHolder (chr), occurrenceID (chr), taxonID (chr),
#>      occurrenceRemarks (chr), datasetName (chr), eventTime (chr),
#>      identifier (chr), identificationID (chr), county (chr), identifiedBy
#>      (chr), stateProvince (chr), recordNumber (chr), verbatimElevation
#>      (chr), georeferenceSources (chr), coordinateAccuracy (dbl), elevation
#>      (dbl), elevationAccuracy (dbl), depth (dbl), depthAccuracy (dbl),
#>      habitat (chr), fieldNotes (chr), municipality (chr)
```

> You can choose what fields to return. This isn't passed on to the API query to GBIF as they don't allow that, but we filter out the columns before we give the data back to you.


```r
occ_search(scientificName = "Ursus americanus", fields = 'name', limit = 20)
#> Records found [7202] 
#> Records returned [20] 
#> No. unique hierarchies [1] 
#> No. media records [19] 
#> Args [scientificName=Ursus americanus, limit=20, offset=0, fields=name] 
#> First 10 rows of data
#> 
#>                name
#> 1  Ursus americanus
#> 2  Ursus americanus
#> 3  Ursus americanus
#> 4  Ursus americanus
#> 5  Ursus americanus
#> 6  Ursus americanus
#> 7  Ursus americanus
#> 8  Ursus americanus
#> 9  Ursus americanus
#> 10 Ursus americanus
#> ..              ...
```

> Most parameters are vectorized, so you can pass in more than one value:


```r
splist <- c('Cyanocitta stelleri', 'Junco hyemalis', 'Aix sponsa')
keys <- sapply(splist, function(x) name_suggest(x)$key[1], USE.NAMES = FALSE)
occ_search(taxonKey = keys, limit = 5)
#> Occ. found [2482598 (355649), 2492010 (1942197), 2498387 (592079)] 
#> Occ. returned [2482598 (5), 2492010 (5), 2498387 (5)] 
#> No. unique hierarchies [2482598 (1), 2492010 (1), 2498387 (1)] 
#> No. media records [2482598 (5), 2492010 (5), 2498387 (2)] 
#> Args [taxonKey=2482598,2492010,2498387, limit=5, offset=0, fields=all] 
#> First 10 rows of data from 2482598
#> 
#>                  name        key decimalLatitude decimalLongitude
#> 1 Cyanocitta stelleri 1052604494        37.76975        -122.4715
#> 2 Cyanocitta stelleri 1065590449        37.50713        -122.4818
#> 3 Cyanocitta stelleri 1065588252        36.54670        -105.1335
#> 4 Cyanocitta stelleri 1065588311        37.26200        -122.3271
#> 5 Cyanocitta stelleri 1065588175        39.11113        -121.3083
#> Variables not shown: issues (chr), datasetKey (chr), publishingOrgKey
#>      (chr), publishingCountry (chr), protocol (chr), lastCrawled (chr),
#>      lastParsed (chr), extensions (chr), basisOfRecord (chr), taxonKey
#>      (int), kingdomKey (int), phylumKey (int), classKey (int), orderKey
#>      (int), familyKey (int), genusKey (int), speciesKey (int),
#>      scientificName (chr), kingdom (chr), phylum (chr), order (chr),
#>      family (chr), genus (chr), species (chr), genericName (chr),
#>      specificEpithet (chr), taxonRank (chr), dateIdentified (chr), year
#>      (int), month (int), day (int), eventDate (chr), modified (chr),
#>      lastInterpreted (chr), references (chr), identifiers (chr), facts
#>      (chr), relations (chr), geodeticDatum (chr), class (chr), countryCode
#>      (chr), country (chr), verbatimEventDate (chr), verbatimLocality
#>      (chr), http...unknown.org.occurrenceDetails (chr), rights (chr),
#>      rightsHolder (chr), occurrenceID (chr), collectionCode (chr), taxonID
#>      (chr), occurrenceRemarks (chr), gbifID (chr), institutionCode (chr),
#>      catalogNumber (chr), datasetName (chr), recordedBy (chr), eventTime
#>      (chr), identifier (chr), identificationID (chr)
```

## Single occurrence records

> You can also get data for specific occurrences identified by occurrence key. Data is returned as a list, with slots for metadata and data

> Just data


```r
occ_get(key = 766766824, return = 'data')
#>              name       key decimalLatitude decimalLongitude        issues
#> 1 Corvus monedula 766766824         59.4568          17.9054 depunl,gass84
```

> Just taxonomic hierarchy


```r
occ_get(key = 766766824, return = 'hier')
#>              name     key    rank
#> 1        Animalia       1 kingdom
#> 2        Chordata      44  phylum
#> 3            Aves     212   class
#> 4   Passeriformes     729   order
#> 5        Corvidae    5235  family
#> 6          Corvus 2482468   genus
#> 7 Corvus monedula 2482473 species
```

> All data, or leave return parameter blank


```r
occ_get(key = 766766824, return = 'all')
#> $hierarchy
#>              name     key    rank
#> 1        Animalia       1 kingdom
#> 2        Chordata      44  phylum
#> 3            Aves     212   class
#> 4   Passeriformes     729   order
#> 5        Corvidae    5235  family
#> 6          Corvus 2482468   genus
#> 7 Corvus monedula 2482473 species
#> 
#> $media
#> list()
#> 
#> $data
#>              name       key decimalLatitude decimalLongitude        issues
#> 1 Corvus monedula 766766824         59.4568          17.9054 depunl,gass84
```

> Get many occurrences. `occ_get` is vectorized


```r
occ_get(key = c(766766824, 101010, 240713150, 855998194), return = 'data')
#>                  name       key decimalLatitude decimalLongitude
#> 1     Corvus monedula 766766824         59.4568          17.9054
#> 2 Platydoras costatus    101010         -4.3500         -70.0667
#> 3            Pelosina 240713150        -77.5667         163.5830
#> 4    Sciurus vulgaris 855998194         58.4068          12.0438
#>           issues
#> 1  depunl,gass84
#> 2 cucdmis,gass84
#> 3 cdround,gass84
#> 4  depunl,gass84
```

## Cleaning data

> what do issues mean, can print whole table, or search for matches


```r
head(gbif_issues())
#>    code                              issue
#> 1   bri            BASIS_OF_RECORD_INVALID
#> 2   ccm         CONTINENT_COUNTRY_MISMATCH
#> 3   cdc CONTINENT_DERIVED_FROM_COORDINATES
#> 4 conti                  CONTINENT_INVALID
#> 5  cdiv                 COORDINATE_INVALID
#> 6 cdout            COORDINATE_OUT_OF_RANGE
#>                                                                                                    description
#> 1 The given basis of record is impossible to interpret or seriously different from the recommended vocabulary.
#> 2                                                       The interpreted continent and country do not match up.
#> 3                  The interpreted continent is based on the coordinates, not the verbatim string information.
#> 4                                                                      Uninterpretable continent values found.
#> 5                                      Coordinate value given in some form but GBIF is unable to interpret it.
#> 6                                        Coordinate has invalid lat/lon values out of their decimal max range.
```

> compare out data to after occ_issues use


```r
(res <- occ_search(geometry = 'POLYGON((30.1 10.1, 10 20, 20 40, 40 40, 30.1 10.1))', limit = 50))
#> Records found [972494] 
#> Records returned [50] 
#> No. unique hierarchies [45] 
#> No. media records [16] 
#> Args [geometry=POLYGON((30.1 10.1, 10 20, 20 40, 40 40, 30.1 10.1)),
#>      limit=50, offset=0, fields=all] 
#> First 10 rows of data
#> 
#>                       name        key decimalLatitude decimalLongitude
#> 1          Hirundo rustica 1052606227        30.19364         31.10182
#> 2      Porphyrio porphyrio 1052606578        30.19364         31.10182
#> 3      Phalacrocorax carbo 1052606608        30.19364         31.10182
#> 4          Motacilla flava 1052606594        30.19364         31.10182
#> 5     Plegadis falcinellus 1052606235        30.19364         31.10182
#> 6         Luscinia svecica 1052606247        30.19364         31.10182
#> 7        Saxicola rubicola 1052606528        35.01809         25.85227
#> 8  Rostratula benghalensis 1052606564        30.19364         31.10182
#> 9             Ceryle rudis 1052606237        30.19364         31.10182
#> 10      Halcyon smyrnensis 1052606212        30.19364         31.10182
#> ..                     ...        ...             ...              ...
#> Variables not shown: issues (chr), datasetKey (chr), publishingOrgKey
#>      (chr), publishingCountry (chr), protocol (chr), lastCrawled (chr),
#>      lastParsed (chr), extensions (chr), basisOfRecord (chr), taxonKey
#>      (int), kingdomKey (int), phylumKey (int), classKey (int), orderKey
#>      (int), familyKey (int), genusKey (int), speciesKey (int),
#>      scientificName (chr), kingdom (chr), phylum (chr), order (chr),
#>      family (chr), genus (chr), species (chr), genericName (chr),
#>      specificEpithet (chr), taxonRank (chr), dateIdentified (chr), year
#>      (int), month (int), day (int), eventDate (chr), modified (chr),
#>      lastInterpreted (chr), references (chr), identifiers (chr), facts
#>      (chr), relations (chr), geodeticDatum (chr), class (chr), countryCode
#>      (chr), country (chr), verbatimEventDate (chr),
#>      http...unknown.org.occurrenceDetails (chr), rights (chr),
#>      rightsHolder (chr), occurrenceID (chr), collectionCode (chr), taxonID
#>      (chr), gbifID (chr), institutionCode (chr), catalogNumber (chr),
#>      datasetName (chr), recordedBy (chr), identifier (chr),
#>      identificationID (chr), verbatimLocality (chr), occurrenceRemarks
#>      (chr), locality (chr), identifiedBy (chr), infraspecificEpithet (chr)
```

> Keep data only with particular issues


```r
res %>% occ_issues(cdround)
#> Records found [972494] 
#> Records returned [15] 
#> No. unique hierarchies [45] 
#> No. media records [16] 
#> Args [geometry=POLYGON((30.1 10.1, 10 20, 20 40, 40 40, 30.1 10.1)),
#>      limit=50, offset=0, fields=all] 
#> First 10 rows of data
#> 
#>                       name        key decimalLatitude decimalLongitude
#> 1          Hirundo rustica 1052606227        30.19364         31.10182
#> 2      Porphyrio porphyrio 1052606578        30.19364         31.10182
#> 3      Phalacrocorax carbo 1052606608        30.19364         31.10182
#> 4          Motacilla flava 1052606594        30.19364         31.10182
#> 5     Plegadis falcinellus 1052606235        30.19364         31.10182
#> 6         Luscinia svecica 1052606247        30.19364         31.10182
#> 7        Saxicola rubicola 1052606528        35.01809         25.85227
#> 8  Rostratula benghalensis 1052606564        30.19364         31.10182
#> 9             Ceryle rudis 1052606237        30.19364         31.10182
#> 10      Halcyon smyrnensis 1052606212        30.19364         31.10182
#> ..                     ...        ...             ...              ...
#> Variables not shown: issues (chr), datasetKey (chr), publishingOrgKey
#>      (chr), publishingCountry (chr), protocol (chr), lastCrawled (chr),
#>      lastParsed (chr), extensions (chr), basisOfRecord (chr), taxonKey
#>      (int), kingdomKey (int), phylumKey (int), classKey (int), orderKey
#>      (int), familyKey (int), genusKey (int), speciesKey (int),
#>      scientificName (chr), kingdom (chr), phylum (chr), order (chr),
#>      family (chr), genus (chr), species (chr), genericName (chr),
#>      specificEpithet (chr), taxonRank (chr), dateIdentified (chr), year
#>      (int), month (int), day (int), eventDate (chr), modified (chr),
#>      lastInterpreted (chr), references (chr), identifiers (chr), facts
#>      (chr), relations (chr), geodeticDatum (chr), class (chr), countryCode
#>      (chr), country (chr), verbatimEventDate (chr),
#>      http...unknown.org.occurrenceDetails (chr), rights (chr),
#>      rightsHolder (chr), occurrenceID (chr), collectionCode (chr), taxonID
#>      (chr), gbifID (chr), institutionCode (chr), catalogNumber (chr),
#>      datasetName (chr), recordedBy (chr), identifier (chr),
#>      identificationID (chr), verbatimLocality (chr), occurrenceRemarks
#>      (chr), locality (chr), identifiedBy (chr), infraspecificEpithet (chr)
```

> Remove data only with certain issues


```r
res %>% occ_issues(-mdatunl)
#> Records found [972494] 
#> Records returned [38] 
#> No. unique hierarchies [45] 
#> No. media records [16] 
#> Args [geometry=POLYGON((30.1 10.1, 10 20, 20 40, 40 40, 30.1 10.1)),
#>      limit=50, offset=0, fields=all] 
#> First 10 rows of data
#> 
#>                      name        key decimalLatitude decimalLongitude
#> 13 Sympetrum fonscolombii 1065585732        35.01809         25.85227
#> 14        Charadriiformes 1065590820        30.19364         31.10182
#> 15      Passer domesticus 1065590106        35.01809         25.85227
#> 16           Alcea setosa 1076770356        33.86317         35.54867
#> 17      Arbutus andrachne 1076771248        33.86050         35.57142
#> 18  Aristida caerulescens 1076771339        33.86378         35.56557
#> 19         Cistus incanus 1076774204        33.89840         35.73811
#> 20      Clematis cirrhosa 1076774251        33.86050         35.57142
#> 21      Clematis flammula 1076774254        33.86050         35.57142
#> 22     Asplenium ceterach 1076773752        33.89840         35.73811
#> ..                    ...        ...             ...              ...
#> Variables not shown: issues (chr), datasetKey (chr), publishingOrgKey
#>      (chr), publishingCountry (chr), protocol (chr), lastCrawled (chr),
#>      lastParsed (chr), extensions (chr), basisOfRecord (chr), taxonKey
#>      (int), kingdomKey (int), phylumKey (int), classKey (int), orderKey
#>      (int), familyKey (int), genusKey (int), speciesKey (int),
#>      scientificName (chr), kingdom (chr), phylum (chr), order (chr),
#>      family (chr), genus (chr), species (chr), genericName (chr),
#>      specificEpithet (chr), taxonRank (chr), dateIdentified (chr), year
#>      (int), month (int), day (int), eventDate (chr), modified (chr),
#>      lastInterpreted (chr), references (chr), identifiers (chr), facts
#>      (chr), relations (chr), geodeticDatum (chr), class (chr), countryCode
#>      (chr), country (chr), verbatimEventDate (chr),
#>      http...unknown.org.occurrenceDetails (chr), rights (chr),
#>      rightsHolder (chr), occurrenceID (chr), collectionCode (chr), taxonID
#>      (chr), gbifID (chr), institutionCode (chr), catalogNumber (chr),
#>      datasetName (chr), recordedBy (chr), identifier (chr),
#>      identificationID (chr), verbatimLocality (chr), occurrenceRemarks
#>      (chr), locality (chr), identifiedBy (chr), infraspecificEpithet (chr)
```

## Other things to explore

* Maps! We have some basic mapping functionality to help you visualize data
* Registry data - there's a suite of functions to explore data providers to GBIF
* `spocc` Explore the R package `spocc`, where we integrate biodiversity data
from many places, including from GBIF via this package [spocc](https://github.com/ropensci/spocc)

## end

> That's it! Thanks for watching...
