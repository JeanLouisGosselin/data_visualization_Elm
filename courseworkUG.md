---
id: litvis

narrative-schemas:
  - ../narrative-schemas/courseworkUG.yml

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/tidy: latest
---

@import "../css/datavis.less"

```elm {l=hidden}
import Tidy exposing (..)
import VegaLite exposing (..)
```

# Jean-Louis Gosselin:

# Undergraduate Coursework

##Table of contents

1. [Introduction: Research Questions / Data / Initial designs](#1-introduction:-research-questions-/-data-/-initial-designs)
2. [The visualization](#2-the-visualization)
3. [Insights](#3-insights)
4. [Design justification](#4-design-justification)
5. [References](#5-references)

### 1. Introduction

### (a) Research Questions

As outlined in the submitted coursework proposal, my initial intention was to investigate data from the Organisation of Economic Co-operation & Development (OECD) that reflects some of the eating habits (good and bad) all member states have been following over a dozen years in the new millenium, with the aim of providing answers to both following question:

#### _(1) Are healthy eating habits inversely proportional to bad eating habits?_

-> _In other words: could the visualization suggest something along the lines of:_

![unnamed](screenshots/answerChart.png)

#### _(2) Have these habits noticeably changed over time for every country?_

(_NOTE_: only _member countries_ of the OECD, officially totalling 36, have been considered for this coursework; data for accession candidates or key partners was not analysed).

Founded in 1961 to initially stimulate economic progress and world trade, the [OECD](http://www.oecd.org/general/Key-information-about-the-OECD.pdf) works on establishing international norms and finding evidence-based solutions to a range of social, economic and environmental challenges.

From improving economic performance and creating jobs to fostering strong education and fighting international tax evasion, the OECD aims to provide a unique forum and knowledge hub for data and analysis, exchange of experiences, best-practice sharing, and advice on public policies and global standard-setting. Their statistics [webpage](https://stats.oecd.org/) certainly bears testatment to this: from Monthly Economic Indicators to Agriculture & Fisheries, and from Demography & Population to Industry & Services, a wide choice of spreadsheets and csv files is freely accessible:

![unnamed](screenshots/all_themes.png)

### (b) Data

The starting idea was to consider using all 4 available files in the "_non-medical determinant of health_" section:

![unnamed](screenshots/health_section.png)

Although _TOBACCO_CONSUMPTION_ for instance contains data which looked promising at first and would have been of great interest to us (namely: the _Grammes per capita_ category):

```
VAR,Variable,UNIT,Measure,COU,Country,YEA,Year,Value,Flag Codes,Flags
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2000,2000,1353,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2001,2001,1308,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2002,2002,1393,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2003,2003,1316,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2004,2004,1277,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2005,2005,1219,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2006,2006,1180,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2007,2007,1201,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2008,2008,1130,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2009,2009,1064,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2010,2010,1009,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2011,2011,971.8,B,Break
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2012,2012,964.6,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2013,2013,916.1,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2014,2014,830.9,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2015,2015,789.9,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2016,2016,756.4,,
TOBATBCT,Tobacco consumption,GRPPERQT,Grammes per capita (15+),AUS,Australia,2017,2017,677.1,,
```

...on further inspection however, this dataset sadly proved to be either incomplete or seriously lacking for some member countries, which led me to discard the dataset altogether (as well as the _BODY WEIGHT_ dataset, which would have unjustifyingly complicated the data analysing process).

This left me with only 2 datasets to investigate:

- _ALCOHOL CONSUMPTION_
- _FOOD SUPPLY & CONSUMPTION_

The _ALCOHOL CONSUMPTION_ dataset is very straightforward and provides data that is of specific use to this coursework, namely: alcohol consumption per capita from 2000 to 2016:

```
VAR,Variable,UNIT,Measure,COU,Country,YEA,Year,Value,Flag Codes,Flags
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2000,2000,10.2,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2001,2001,10,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2002,2002,10.4,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2003,2003,10.3,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2004,2004,10.5,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2005,2005,10.5,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2006,2006,10.8,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2007,2007,10.8,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2008,2008,10.6,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2009,2009,10.5,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2010,2010,10.3,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2011,2011,10,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2012,2012,9.9,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2013,2013,9.8,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2014,2014,9.5,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2015,2015,9.7,,
ACOLALCT,Alcohol consumption,LIPPERNB,Litres per capita (15+),AUS,Australia,2016,2016,9.4,,
```

Strangely, the _FOOD SUPPLY & CONSUMPTION_ dataset is far more varied (in some cases even: patchy), containing a plethora of information and units which may be difficult to amalgamate (from kilocalories to grammes/litres to percentages) and consisting of many sub-categories, for instance _vegetebale consumtion_ (all set in population percentages) and _fat supply_:

```
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,TUR,Turkey,2008,2008,59.7,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,TUR,Turkey,2010,2010,67.7,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,TUR,Turkey,2012,2012,63.6,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,TUR,Turkey,2014,2014,57.7,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,TUR,Turkey,2016,2016,59,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,TUR,Turkey,2008,2008,61.3,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,TUR,Turkey,2010,2010,69.4,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,TUR,Turkey,2012,2012,64.8,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,TUR,Turkey,2014,2014,60.3,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,TUR,Turkey,2016,2016,60.9,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGFMTF,% of females aged 15 years old and over,GBR,United Kingdom,2014,2014,70,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,GBR,United Kingdom,2014,2014,60.3,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,GBR,United Kingdom,2014,2014,65.5,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGFMTF,% of females aged 15 years old and over,USA,United States,2007,2007,91.6,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGFMTF,% of females aged 15 years old and over,USA,United States,2009,2009,93.5,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGFMTF,% of females aged 15 years old and over,USA,United States,2011,2011,91.8,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGFMTF,% of females aged 15 years old and over,USA,United States,2014,2014,92.4,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGFMTF,% of females aged 15 years old and over,USA,United States,2015,2015,91.7,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,USA,United States,2007,2007,93.4,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,USA,United States,2009,2009,91,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,USA,United States,2011,2011,93,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,USA,United States,2014,2014,91.6,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGHOTH,% of males aged 15 years old and over,USA,United States,2015,2015,91.9,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,USA,United States,2007,2007,92.5,,
FOODVEGE,"Vegetables consumption, daily (survey)",FRVGTOTX,% of population aged 15 years old and over,USA,United States,2009,2009,92.3,,
```

```
VAR,Variable,UNIT,Measure,COU,Country,YEA,Year,Value,Flag Codes,Flags
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2000,2000,130,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2001,2001,134.3,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2002,2002,131.2,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2003,2003,131,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2004,2004,134,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2005,2005,137.3,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2006,2006,143.4,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2007,2007,143.9,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2008,2008,141,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2009,2009,143.6,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2010,2010,145.9,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2011,2011,148.6,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2012,2012,152.4,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUS,Australia,2013,2013,150.9,,
FOODTFAT,Total fat supply,PROPERQT,Grammes per capita per day,AUT,Austria,2000,2000,159,,
```

The presence of _SUPPLY_- and _CONSUMPTION_-based data proved particularly problematic. Since using percentage-based data (which, in this dataset, is labelled as _CONSUMPTION_) differed greatly in nature from the data found in the _ALCOHOL_CONSUMPTION_ dataset, the only way I found to justify the use of _SUPPLY_-based data is as follows:

- alcohol (and in some way: sugar) are non perishable products; therefore: supply equals consumption

- fruit and vegetables are perishable products; therefore, supply would not logically be equal to consumption. But for the purpose of this coursework, we assume that all fruit & vegetables are supplied in order to be consumed.

The other two hurdles I have had to overcome with these two datasets are as follows:

- the timeline factor: since some date ranges are missing in the _FRUIT & VEGETABLE_ dataset, the timeline has been levelled out to take in only years between 2005 and 2013;

- Latvia and Lithuania: although the data related to alcohol consumption is clearly present for these two countries, the data relating to sugar/fruit/vegetable supply is patchy and often missing. Therefore, the formal list of 36 OECD member countries has been reduced to 34.

Much manual effort was put into cleaning the data - the sets of which have been added for use and future reference at the very bottom of this coursework file.

### (c) Inital Designs

The long process of data cleansing mentioned above enabled me to put into good use the sound advice discussed in week 2 by New York Times science graphics editor Jonathan Corum who makes the case for sketching as part of the _design process_, suggesting it is a form of "visual problem solving" and that because sketches are quick to generate, they are not "commitments" to a particular design and allow us to explore different design possibilities easily.

The first idea I had in mind (which I briefly discussed in the coursework proposal) was to either use:

- a diverging stacked bar chart (commonly referred to as "_pyramid_"), where both "_bad habits_" would be represented on the left-hand side and "_good habits_" on the right-hand side,
  with flags of all 34 (36-2) countries placed in the middle:

![unnamed](screenshots/pyramid.png)

- a positive/negative bar chart discussed in week 3, with both "_good habits_" represented in the "positives" and "_bad habits_" in the "negatives" (placing names of countries would require the use of interactive tooltips):

![unnamed](screenshots/divergingColourScaleChart.png)

In the end, the ideal solution I imagined and tried implementing was the following (drawn using an online [sketchpad](https://sketch.io/sketchpad/)):

![unnamed](screenshots/fullSketch.png)

The idea here was to use 3 interactive techniques overlapping in a _single_ chart:

- an interactive legend consisting of radio buttons (for years)
- another interactive legend, this time consisting of coloured squares, one for each product
- finally: tooltips to display information on the chart itself about the country and its corresponding product quantity.

Therefore, by clicking on, say, _SUGAR_, my intention was to enable the user to see in one fell swoop the changes in sugar supply per country on a year-to-year basis, from 2005 up to 2013 and vice versa (_note_: a line, drawn in red on the sketch, representing the average supply of sugar for the year 2008 would be displayed, providing the user with a clearer sense of: (1) the number of countries that lie below/above this average (2) the change in countries occurred over the years).

As further explained in section 4 (_Design Justification_), the intention was to make maximum use of:

- Edward Tufte's concept of data-ink ratio

- Ben Schneiderman's visual information-seeking mantra of _"overview first, zoom and filter, then details-on-demand"_

Although the visualization I have implemented did not in the end quite achieve this level of minimalist efficiency (for reasons I shall endeavour to explain below), all three interactive techniques have been used, albeit splayed across a two-part visualization.

### 2. The Visualization

```elm {l=hidden highlight=[21-24,26-29]}
alcoholData =
    dataFromColumns []
        << dataColumn "country" (strColumn "country" alcohol |> strs)
        << dataColumn "year" (numColumn "year" alcohol |> nums)
        << dataColumn "quantity" (numColumn "quantity" alcohol |> nums)


sugarData =
    dataFromColumns []
        << dataColumn "country" (strColumn "country" sugar |> strs)
        << dataColumn "year" (numColumn "year" sugar |> nums)
        << dataColumn "quantity" (numColumn "quantity" sugar |> nums)


fruitData =
    dataFromColumns []
        << dataColumn "country" (strColumn "country" fruit |> strs)
        << dataColumn "year" (numColumn "year" fruit |> nums)
        << dataColumn "quantity" (numColumn "quantity" fruit |> nums)


vegetableData =
    dataFromColumns []
        << dataColumn "country" (strColumn "country" vegetables |> strs)
        << dataColumn "year" (numColumn "year" vegetables |> nums)
        << dataColumn "quantity" (numColumn "quantity" vegetables |> nums)


overallData =
    dataFromColumns []
        << dataColumn "year" (numColumn "year" yearlyProductAverages |> nums)
        << dataColumn "product type" (strColumn "product type" yearlyProductAverages |> strs)
        << dataColumn "annual average per capita" (numColumn "annual average per capita" yearlyProductAverages |> nums)


alcoholColours =
    categoricalDomainMap
        [ ( "2005", "rgb(255,153,163)" )
        , ( "2006", "rgb(255,153,153)" )
        , ( "2007", "rgb(255,102,102)" )
        , ( "2008", "rgb(255,51,51)" )
        , ( "2009", "rgb(255,0,0)" )
        , ( "2010", "rgb(204,0,0)" )
        , ( "2011", "rgb(153,0,0)" )
        , ( "2012", "rgb(102,0,0)" )
        , ( "2013", "rgb(26,0,0)" )
        ]


sugarColours =
    categoricalDomainMap
        [ ( "2005", "rgb(128,180,255)" )
        , ( "2006", "rgb(128,170,255)" )
        , ( "2007", "rgb(77,136,255)" )
        , ( "2008", "rgb(25,102,255)" )
        , ( "2009", "rgb(0,77,230)" )
        , ( "2010", "rgb(0,60,179)" )
        , ( "2011", "rgb(0,43,128)" )
        , ( "2012", "rgb(0,26,77)" )
        , ( "2013", "rgb(0,9,26)" )
        ]


fruitColours =
    categoricalDomainMap
        [ ( "2005", "rgb(255,195,77)" )
        , ( "2006", "rgb(255,195,77)" )
        , ( "2007", "rgb(255,179,25)" )
        , ( "2008", "rgb(230,153,0)" )
        , ( "2009", "rgb(179,119,0)" )
        , ( "2010", "rgb(128,85,0)" )
        , ( "2011", "rgb(77,51,0)" )
        , ( "2012", "rgb(26,17,0)" )
        , ( "2013", "rgb(0,0,0)" )
        ]


vegetableColours =
    categoricalDomainMap
        [ ( "2005", "rgb(128,255,138)" )
        , ( "2006", "rgb(128,255,128)" )
        , ( "2007", "rgb(77,255,77)" )
        , ( "2008", "rgb(25,255,25)" )
        , ( "2009", "rgb(0,230,0)" )
        , ( "2010", "rgb(0,179,0)" )
        , ( "2011", "rgb(0,128,0)" )
        , ( "2012", "rgb(0,77,0)" )
        , ( "2013", "rgb(0,26,0)" )
        ]


productColours =
    categoricalDomainMap
        [ ( "alcohol", "rgb(255,0,0)" )
        , ( "all sugar", "rgb(0,77,230)" )
        , ( "fruit", "rgb(255,179,25)" )
        , ( "vegetables", "rgb(0,230,0)" )
        ]


encHighlight =
    encoding
        << position X [ pName "country", pNominal, pTitle "" ]
        << position Y [ pName "quantity", pQuant ]
        << color
            [ mSelectionCondition (selectionName "mySelection")
                [ mName "product", mNominal, mScale alcoholColours ]
                [ mStr "black" ]
            ]
        << opacity
            [ mSelectionCondition (selectionName "mySelection")
                [ mNum 1 ]
                [ mNum 0.1 ]
            ]
```

```elm {l=hidden highlight=21}
alcoholInteractiveLegend : String -> String -> Spec
alcoholInteractiveLegend field selName =
    let
        enc =
            encoding
                << position Y
                    [ pName field
                    , pNominal
                    , pAxis [ axTitle "", axDomain False, axTicks False ]
                    ]
                << color
                    [ mSelectionCondition (selectionName selName)
                        [ mName field, mNominal, mScale alcoholColours, mLegend [] ]
                        [ mStr "lightgrey" ]
                    ]

        sel =
            selection
                << select selName seMulti [ seEncodings [ chColor ] ]
    in
    asSpec [ sel [], enc [], square [ maSize 120, maOpacity 1 ] ]
```

```elm {l=hidden highlight=21}
sugarInteractiveLegend : String -> String -> Spec
sugarInteractiveLegend field selName =
    let
        enc =
            encoding
                << position Y
                    [ pName field
                    , pNominal
                    , pAxis [ axTitle "", axDomain False, axTicks False ]
                    ]
                << color
                    [ mSelectionCondition (selectionName selName)
                        [ mName field, mNominal, mScale sugarColours, mLegend [] ]
                        [ mStr "lightgrey" ]
                    ]

        sel =
            selection
                << select selName seMulti [ seEncodings [ chColor ] ]
    in
    asSpec [ sel [], enc [], square [ maSize 120, maOpacity 1 ] ]
```

```elm {l=hidden highlight=21}
fruitInteractiveLegend : String -> String -> Spec
fruitInteractiveLegend field selName =
    let
        enc =
            encoding
                << position Y
                    [ pName field
                    , pNominal
                    , pAxis [ axTitle "", axDomain False, axTicks False ]
                    ]
                << color
                    [ mSelectionCondition (selectionName selName)
                        [ mName field, mNominal, mScale fruitColours, mLegend [] ]
                        [ mStr "lightgrey" ]
                    ]

        sel =
            selection
                << select selName seMulti [ seEncodings [ chColor ] ]
    in
    asSpec [ sel [], enc [], square [ maSize 120, maOpacity 1 ] ]
```

```elm {l=hidden highlight=21}
vegetableInteractiveLegend : String -> String -> Spec
vegetableInteractiveLegend field selName =
    let
        enc =
            encoding
                << position Y
                    [ pName field
                    , pNominal
                    , pAxis [ axTitle "", axDomain False, axTicks False ]
                    ]
                << color
                    [ mSelectionCondition (selectionName selName)
                        [ mName field, mNominal, mScale vegetableColours, mLegend [] ]
                        [ mStr "lightgrey" ]
                    ]

        sel =
            selection
                << select selName seMulti [ seEncodings [ chColor ] ]
    in
    asSpec [ sel [], enc [], square [ maSize 120, maOpacity 1 ] ]
```

```elm {v interactive l=hidden}
introAverages : Spec
introAverages =
    let
        enc =
            encoding
                << position X [ pName "year", pTemporal, pTitle "", pAxis [] ]
                << position Y
                    [ pName "annual average per capita"
                    , pQuant
                    , pTitle ""
                    ]
                << color [ mName "product type", mNominal, mScale productColours, mLegend [] ]
                << column
                    [ fName "product type"
                    , fNominal
                    , fHeader [ hdTitleFontSize 0, hdLabelAngle 0, hdLabelAlign haCenter, hdLabelFontSize 15 ]
                    ]
                << tooltips
                    [ [ tName "year", tTemporal, tFormat "%Y" ]
                    , [ tName "annual average per capita", tQuant ]
                    ]

        res =
            resolve
                << resolution (reScale [ ( chY, reIndependent ) ])
    in
    toVegaLite [ height 250, width 100, overallData [], area [], enc [], res [] ]
```

```elm {l=hidden v interactive highlight=[6,14,15,24]}
alcoholConsumed : Spec
alcoholConsumed =
    let
        trans =
            transform
                << filter (fiSelection "legendSel")

        enc =
            encoding
                << position X [ pName "country", pNominal, pTitle "" ]
                << position Y [ pName "quantity", pQuant, pTitle "" ]
                << color [ mName "year", mNominal, mScale alcoholColours, mLegend [] ]
                << tooltips
                    [ [ tName "country", tNominal ]
                    , [ tName "quantity", tQuant ]
                    ]

        chartSpec =
            asSpec [ width 500, height 150, trans [], enc [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis [ axcoTicks False, axcoDomain False ])
    in
    toVegaLite
        [ cfg []
        , alcoholData []
        , hConcat [ chartSpec, alcoholInteractiveLegend "year" "legendSel" ]
        ]
```

```elm {l=hidden v interactive highlight=[6,14,15,24]}
sugarSupplied : Spec
sugarSupplied =
    let
        trans =
            transform
                << filter (fiSelection "legendSel")

        enc =
            encoding
                << position X [ pName "country", pNominal, pTitle "" ]
                << position Y [ pName "quantity", pQuant, pTitle "" ]
                << color [ mName "year", mNominal, mScale sugarColours, mLegend [] ]
                << tooltips
                    [ [ tName "country", tNominal ]
                    , [ tName "quantity", tQuant ]
                    ]

        chartSpec =
            asSpec [ width 500, height 150, trans [], enc [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis [ axcoTicks False, axcoDomain False ])
    in
    toVegaLite
        [ cfg []
        , sugarData []
        , hConcat [ chartSpec, sugarInteractiveLegend "year" "legendSel" ]
        ]
```

```elm {l=hidden v interactive highlight=[6,14,15,24]}
fruitSupplied : Spec
fruitSupplied =
    let
        trans =
            transform
                << filter (fiSelection "legendSel")

        enc =
            encoding
                << position X [ pName "country", pNominal, pTitle "" ]
                << position Y [ pName "quantity", pQuant, pTitle "" ]
                << color [ mName "year", mNominal, mScale fruitColours, mLegend [] ]
                << tooltips
                    [ [ tName "country", tNominal ]
                    , [ tName "quantity", tQuant ]
                    ]

        chartSpec =
            asSpec [ width 500, height 150, trans [], enc [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis [ axcoTicks False, axcoDomain False ])
    in
    toVegaLite
        [ cfg []
        , fruitData []
        , hConcat [ chartSpec, fruitInteractiveLegend "year" "legendSel" ]
        ]
```

```elm {l=hidden v interactive highlight=[6,14,15,24]}
vegetablesSupplied : Spec
vegetablesSupplied =
    let
        trans =
            transform
                << filter (fiSelection "legendSel")

        enc =
            encoding
                << position X [ pName "country", pNominal, pTitle "" ]
                << position Y [ pName "quantity", pQuant, pTitle "" ]
                << color [ mName "year", mNominal, mScale vegetableColours, mLegend [] ]
                << tooltips
                    [ [ tName "country", tNominal ]
                    , [ tName "quantity", tQuant ]
                    ]

        chartSpec =
            asSpec [ width 500, height 150, trans [], enc [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])
                << configuration
                    (coAxis [ axcoTicks False, axcoDomain False ])
    in
    toVegaLite
        [ cfg []
        , vegetableData []
        , hConcat [ chartSpec, vegetableInteractiveLegend "year" "legendSel" ]
        ]
```

### 3. Insights

From glancing at the first top part of the visualization (consisting of 4 horizontally juxtaposed area charts -> horizontal juxtaposition being one of Tufte's cherished efficient design concepts), several observations come to mind:

- alcolhol consumption has dropped noticeably between 2005 and 2013 across 34 member countries (Latvia and Lithuania not being represented for reasons explained in Section 1 "_Data_")

- sugar supply has been slightly on the rise generally

- fruit and vegetable supply have shown somewhat similar trajectories, apart from the last 4 years, where after a noticeable fall (2008 financial crisis, perhaps...?), fruit supply is shown to be on the rise, while vegetable supply is seen to rise and decrease slightly.

With alcohol consumption showing a much sharper decline than the rise of sugar supply (thanks to the information displayed by tooltips), the visualization would suggest that "_good eating habits is somewhat inversely proportional to bad eating habits_".

As for the second part of the visualization which consists of 4 vertically juxtaposed sparklines, the change in colour saturation itself tells its own story: changes in alcohol consumption and sugar/fruit/vegetable supply are significant in some countries, and this would warrant further investigation into the possible socio-economic reasons behind such differences. Again, the visualization suggests that "_some habits have indeed noticeably changed over time in some countries_".

Interestingly, both parts of the visualization do not act independently from one another: the top part (besides being a colour code reference for the bottom part - see next section below), which displays averages of quantities consumed or supplied on a yearly basis, also acts as a general visual reference to the bottom part, since it is possible to visually match a country's average consumption of a certain product with the overall average for that same year (the "ideal" visualization mentioned in Section 1 "_Initial Designs"_ would of course have offered a far simpler and visually more appealling solution).

### 4. Design Justification

Section 1 briefly touched on the implementation imperatives I was faced with regarding the choice of data: not only did I have to consider the country-to-product/quantity rapport, but on top of that I also had to consider the quantity average-to-timeline rapport.

All in all, this turned the customary 2D design requirements to a more elaborate set of 3D design requirements - all this in order to answer as best and as simply as possible both research questions asked at the start of this coursework.

My understanding of all 9 lectures on data visualization is that efficient data vizualisation should follow 3 fundamental rules:

- graphical elegance and simplicity
- enabling user interaction
- a proper use of colour codes

In Section 1 (_"Initial designs"_), I mentioned how two key sets of visualization tenets guided me in my choice of design:

- Edward Tufte's concepts of data-ink ratio and chart junk

- Ben Schneiderman's visual information-seeking mantra of _"overview first, zoom and filter, then details-on-demand"_

In fact, it can honestly be said that alongside other online sources pertaining to data visualization (on the proper use of colours, for instance), Tufte's work _"The Visual Display of Quantitative Information"_ served as the main inspiration for my coursework.

Indeed, ever since I first laid eyes on Tufte's sparkline example from week 2's lecture:

![unnamed](screenshots/TufteSparkline.png)

...I felt that this technique should somehow be applied in my own work, since _"graphical elegance is often found in simplicity of design and complexity of data "_ (Tufte, 2001).

In addition to this, page 9 of Tufte's work served as a clear reminder of what the above visualization ought to display:

![unnamed](screenshots/TufteTenets.png)

...although despite my best efforts, the visualization clearly does not observe some of these tenets!

In order to justify my design as accurately as possible, I have divided this section into 3 parts, in the hope that they would further demonstrate the simplicity and the efficiency of the design:

- Structure & Disposition
- Colours
- Marks

#### Structure & Disposition

There is a simple explanation for my inability to produce the "ideal" chart mentioned in Section 1 ("_Initial Designs_"): I simply could not figure out code-wise how to amalgamate all 3 different interactive tools into one single chart:

- radio buttons to invoke yearly data
- an interactive legend to invoke product category data
- tooltips to display detailed information on yearly averages

Failure to implement these techniques in one fell swoop led me to break this design down to two parts (both _vertically_ juxtaposed):

(1) 4 _horizontally_ juxtaposed graphs, the purposes of which are:

- to provide a first, visually intuitive answer to the first research question (see section 3)
- to provide more detailed information (quantity averages per year) to inquisitive users (using tooltips)
- to serve as the main colour code reference for the entire visualization

_(NOTE: for some reason, the tooltips display the correct average values, but not the correct year, which is stuck on 1970!)_

(2) 4 _vertically_ juxtaposed sparkline graphs, the purposes of which are:

- to provide a first, visually intuitive answer to the second research question (see section 3)
- to provide more detailed information as to the changes of every country's eating habits over a specific time

The horizontal juxtaposition in the top part was entirely intentional: as Edward Tufte himself states:

_"Graphics should tend toward the horizontal, greater in length than height. Several lines of reasoning favor horizontal over vertical displays. First, analogy to the horizon. Our eye is naturally practised in detecting deviations from the horizon, and graphic design should take advantage of this fact. Horizontally stretched time-series are more accessible to the eye."_

The horizontal layout of the top part is validated by the fact that it is necessary for the user to visualize the variations in time of the consumption/supply of all 4 products. The horizontal display enables the time-based narrative to run its course.

The _vertical_ layout of the second part is also intentional: although it is always possible to compare product consumption by country _as well as by product category_, the point of each of these 4 sparklines is first and foremost to highlight the differences in _each_ country over a period of time - in other words: the vertical layout intends to _break_ any form of possible narrative between each of the 4 sparklines. As we shall discuss further in the following section, it is the colour _saturation_ this time that enables the time-based narrative to run its course.

#### Colours

We already know that a visualization specification in litvis / Vega-Lite will not need to specify colours individually. Instead, by default, Vega-Lite will allocate a colour palette according to the type of data you specify should be encoded with colour.

However primal, basic or automatically generated the choice of colours in my visualization may come across, it was certainly not random, and some thought process was actually involved in choosing these colours.

Much emphasis was put in week 3's lecture on colourblindness, and this led me to investigate this further, in order to ensure that the colours I would be using in this work would be appropriate.

To get an idea of how people with various colour-perceiving deficiencies actually perceive colours, I looked into [this website](http://www.colourblindawareness.org/colour-blindness/) which lays out all 4 possible perceptions:

![unnamed](screenshots/1_normalvision.png)

![unnamed](screenshots/2_othervision.png)

![unnamed](screenshots/3_othervision.png)

![unnamed](screenshots/4_othervision.png)

The important thing to look out for was to never mix or even juxtapose both red and green colours in the design. And this in fact suited the design well: with yellow and green being assigned to fruit and vegetables respectively (colours which -for people with no colour-perceiving deficiencies at least- can easily and naturally be associated with these categories), I therefore made sure that red and green colours would be at all times as further apart as possible.

As for the choice in saturation: if the horizontally-layed out top part of the design partly serves as a colour code reference for the bottom part of the design, the colours exposed in this top part would naturally have to be somewhat memorable and immediately identifiable to the user who would eventually come to view the bottom part. In this sense, the choice in colour saturation in the top part is perhaps more important than the colour hues themselves.

The choice in colour saturation in the bottom part however plays an even greater role: to provide the user with a 2-step identification process.

The first step relies on the user's immediate ability to tell which of the sparklines is most recent (the lightest saturation) and which is oldest (the strongest - hopefully, the choice I made will have paid off...), therefore providing the user with an immediate sense of data historicity.

The second step involves a form of interaction with the user (in line, again, with Schneiderman's _"overview first, zoom and filter, then details-on-demand"_ mantra previously mentioned): saturation is in fact required in order to enable the user to choose a year without losing track of the product they are currently concerned with.

Once again, as with the horizontal layout, the vertical layout of this second part ensures that both red and green colours are firmly separated.

#### Marks

Only two marks in this visualization were chosen: AREA and LINE (dotted).

Assigning the AREA mark to the top part should be evident by now: to further reinforce the colours, which are not only used to differentiate each of the 4 graphs, but also serve as colour reference. It is however noteworthy to highlight 2 important factors in this design:

- the HEIGHT value for each of these 4 graphs was intentional: with changes only occurring because of the values' decimals, a "taller" AREA was required in order to highlight these changes more clearly;

- if the tooltips were used to provide the user with year-by-year (though incorrect...) information on averages, value indicators on the Y axis (and which are designed to be product-specific) were intentionally kept (at the risk of imbalancing Tufte's data-ink ratio slightly...), since value ranges greately differ from product to product.

As for the (dotted) LINE mark used in all 4 subsequent charts, this not only offers a level of refinement as well as a sharp design contrast between both top and bottom parts (essential in its own visually-appealing right): it enhances readability and visual correspondance between dot and country.

Of course, the presence of country names (also vertically laid out: a sacrilege in Tufte's view!) makes the Tufte _"makeover"_ illustrated in week 2's lecture incomplete. But it must be noted that this would have been avoided altogether, had I been able to amalgamate both tooltips and interactive legends in the same chart...

### 5. References

**Tufte, Edward R.** () [The Visual Display of Quantitative Information (2nd edition)](http://www.econ.upf.edu/~michael/visualdata/tufte-aesthetics_and_technique.pdf), Graphics Press

_added at the bottom of this page: all code for data tables:_

```elm {l=hidden}
alcohol : Table
alcohol =
    """country,year,quantity
Australia,2005,10.5
Austria,2005,12.6
Belgium,2005,12.2
Canada,2005,8
Chile,2005,7.2
Czech Republic,2005,12
Denmark,2005,12.7
Estonia,2005,13
Finland,2005,10
France,2005,12.9
Germany,2005,12
Greece,2005,10
Hungary,2005,13
Iceland,2005,7.1
Ireland,2005,13.4
Israel,2005,2.4
Italy,2005,7.4
Japan,2005,8.5
Korea,2005,9
Luxembourg,2005,12.3
Mexico,2005,4.9
Netherlands,2005,9.6
New Zealand,2005,9.3
Norway,2005,6.4
Poland,2005,9.1
Portugal,2005,13.3
Slovak Republic,2005,11
Slovenia,2005,10.3
Spain,2005,11.9
Sweden,2005,6.5
Switzerland,2005,10.1
Turkey,2005,1.3
United Kingdom,2005,11.4
United States,2005,8.5
Australia,2006,10.8
Austria,2006,12.9
Belgium,2006,10.9
Canada,2006,8.2
Chile,2006,6.9
Czech Republic,2006,11.9
Denmark,2006,12.2
Estonia,2006,13.4
Finland,2006,10.1
France,2006,13.1
Germany,2006,12.1
Greece,2006,9.5
Hungary,2006,13.2
Iceland,2006,7.2
Ireland,2006,13.4
Israel,2006,2.2
Italy,2006,7.3
Japan,2006,7.9
Korea,2006,9.2
Luxembourg,2006,12.3
Mexico,2006,5.1
Netherlands,2006,9.6
New Zealand,2006,9.3
Norway,2006,6.5
Poland,2006,9.9
Portugal,2006,13.1
Slovak Republic,2006,10.6
Slovenia,2006,12.2
Spain,2006,11.9
Sweden,2006,6.8
Switzerland,2006,10.2
Turkey,2006,1.2
United Kingdom,2006,11
United States,2006,8.6
Australia,2007,10.8
Austria,2007,12.9
Belgium,2007,10.3
Canada,2007,8.4
Chile,2007,7.4
Czech Republic,2007,12.1
Denmark,2007,12.1
Estonia,2007,14.8
Finland,2007,10.5
France,2007,12.9
Germany,2007,11.9
Greece,2007,9.7
Hungary,2007,12.6
Iceland,2007,7.5
Ireland,2007,13.2
Israel,2007,2.3
Italy,2007,7.2
Japan,2007,7.7
Korea,2007,9.3
Luxembourg,2007,12.2
Mexico,2007,4.1
Netherlands,2007,9.6
New Zealand,2007,9.2
Norway,2007,6.6
Poland,2007,9.9
Portugal,2007,12.6
Slovak Republic,2007,10.7
Slovenia,2007,11
Spain,2007,11.1
Sweden,2007,7
Switzerland,2007,10.4
Turkey,2007,1.3
United Kingdom,2007,11.1
United States,2007,8.7
Australia,2008,10.6
Austria,2008,12.4
Belgium,2008,10.5
Canada,2008,8.4
Chile,2008,7.4
Czech Republic,2008,12.1
Denmark,2008,10.9
Estonia,2008,14.2
Finland,2008,10.3
France,2008,12.5
Germany,2008,11.8
Greece,2008,9.5
Hungary,2008,11.8
Iceland,2008,7.2
Ireland,2008,12.2
Israel,2008,2.5
Italy,2008,6.8
Japan,2008,7.5
Korea,2008,9.5
Luxembourg,2008,12
Mexico,2008,4
Netherlands,2008,9.7
New Zealand,2008,9.5
Norway,2008,6.8
Poland,2008,10.8
Portugal,2008,12.4
Slovak Republic,2008,11.2
Slovenia,2008,11
Spain,2008,10.2
Sweden,2008,7
Switzerland,2008,10.2
Turkey,2008,1.5
United Kingdom,2008,10.8
United States,2008,8.7
Australia,2009,10.5
Austria,2009,12.1
Belgium,2009,10.1
Canada,2009,8.4
Chile,2009,7.2
Czech Republic,2009,12.1
Denmark,2009,10.1
Estonia,2009,11.9
Finland,2009,10
France,2009,12.6
Germany,2009,11.6
Greece,2009,9.1
Hungary,2009,11.5
Iceland,2009,7.2
Ireland,2009,11
Israel,2009,2.6
Italy,2009,6.4,
Japan,2009,7.4
Korea,2009,8.9
Luxembourg,2009,12
Mexico,2009,4.3
Netherlands,2009,9.4
New Zealand,2009,9.3
Norway,2009,6.7
Poland,2009,10.2
Portugal,2009,12
Slovak Republic,2009,10.7
Slovenia,2009,10.5
Spain,2009,10
Sweden,2009,7.3
Switzerland,2009,10.1
Turkey,2009,1.5
United Kingdom,2009,10.1
United States,2009,8.7
Australia,2010,10.3
Austria,2010,12.5
Belgium,2010,10.3
Canada,2010,8.4
Chile,2010,7.6
Czech Republic,2010,11.4
Denmark,2010,10.3
Estonia,2010,11.4
Finland,2010,9.7
France,2010,12.3
Germany,2010,11.4
Greece,2010,9
Hungary,2010,10.8
Iceland,2010,6.8
Ireland,2010,11.6
Israel,2010,2.6
Italy,2010,7
Japan,2010,7.3
Korea,2010,9
Luxembourg,2010,11.8
Mexico,2010,4
Netherlands,2010,9.1
New Zealand,2010,9.6
Norway,2010,6.6
Poland,2010,10
Portugal,2010,12.2
Slovak Republic,2010,10.1
Slovenia,2010,10.3
Spain,2010,9.8
Sweden,2010,7.3
Switzerland,2010,10
Turkey,2010,1.5
United Kingdom,2010,10.1
United States,2010,8.6
Australia,2011,10
Austria,2011,12.2
Belgium,2011,10.1
Canada,2011,8.2
Chile,2011,7.1
Czech Republic,2011,11.5
Denmark,2011,10.5
Estonia,2011,12
Finland,2011,9.8
France,2011,12.4
Germany,2011,11.9
Greece,2011,8
Hungary,2011,11.4
Iceland,2011,6.8
Ireland,2011,11.7
Israel,2011,2.7
Italy,2011,7
Japan,2011,7.3
Korea,2011,8.9
Luxembourg,2011,12.1
Mexico,2011,4
Netherlands,2011,9
New Zealand,2011,9.5
Norway,2011,6.4
Poland,2011,10.3
Portugal,2011,11.9
Slovak Republic,2011,10.2
Slovenia,2011,10.6
Spain,2011,9
Sweden,2011,7.4
Switzerland,2011,10
Turkey,2011,1.5
United Kingdom,2011,9.9
United States,2011,8.7
Australia,2012,9.9
Austria,2012,12.4
Belgium,2012,10.1
Canada,2012,8.3
Chile,2012,8.3
Czech Republic,2012,11.6
Denmark,2012,9.3
Estonia,2012,12.1
Finland,2012,9.3
France,2012,12.2
Germany,2012,11.8
Greece,2012,7.6
Hungary,2012,11.1
Iceland,2012,6.8
Ireland,2012,11.5
Israel,2012,2.8
Italy,2012,7.5
Japan,2012,7.2
Korea,2012,9.1
Luxembourg,2012,12
Mexico,2012,3.9
Netherlands,2012,9.1
New Zealand,2012,9.2
Norway,2012,6.2
Poland,2012,10.2
Portugal,2012,10.9
Slovak Republic,2012,10.1
Slovenia,2012,11
Spain,2012,8.8
Sweden,2012,7.2
Switzerland,2012,9.9
Turkey,2012,1.6
United Kingdom,2012,9.6
United States,2012,8.9
Australia,2013,9.8
Austria,2013,12
Belgium,2013,10.3
Canada,2013,8.2
Chile,2013,7.2
Czech Republic,2013,11.5
Denmark,2013,9.4
Estonia,2013,11.9
Finland,2013,9.1
France,2013,11.6
Germany,2013,11.7
Greece,2013,7.5
Hungary,2013,10.6
Iceland,2013,6.8
Ireland,2013,10.6
Israel,2013,2.9
Italy,2013,7.4
Japan,2013,7.4
Korea,2013,8.7
Luxembourg,2013,11.7
Mexico,2013,3.9
Netherlands,2013,8.7
New Zealand,2013,9.2
Norway,2013,6.2
Poland,2013,10.8
Portugal,2013,10.5
Slovak Republic,2013,9.9
Slovenia,2013,9.5
Spain,2013,8.8
Sweden,2013,7.3
Switzerland,2013,9.8
Turkey,2013,1.4
United Kingdom,2013,9.4
United States,2013,8.8"""
        |> fromCSV
```

```elm {l=hidden}
sugar : Table
sugar =
    """country,year,quantity
Australia,2005,46.8
Austria,2005,49.6
Belgium,2005,53.2
Canada,2005,53.8
Chile,2005,43.6
Czech Republic,2005,37.6
Denmark,2005,50.8
Estonia,2005,52.6
Finland,2005,34
France,2005,41.7
Germany,2005,50.9
Greece,2005,34
Hungary,2005,36
Iceland,2005,47.7
Ireland,2005,38
Israel,2005,26.8
Italy,2005,31.9
Japan,2005,28.7
Korea,2005,38
Luxembourg,2005,50.7
Mexico,2005,46.4
Netherlands,2005,46.7
New Zealand,2005,61.5
Norway,2005,37.8
Poland,2005,43.6
Portugal,2005,30.7
Slovak Republic,2005,47.8
Slovenia,2005,23.8
Spain,2005,29.9
Sweden,2005,41.9
Switzerland,2005,58.9
Turkey,2005,29.3
United Kingdom,2005,37.6
United States,2005,70.4
Australia,2006,46.4
Austria,2006,48.8
Belgium,2006,51
Canada,2006,52.8
Chile,2006,43.5
Czech Republic,2006,32.1
Denmark,2006,54
Estonia,2006,47.8
Finland,2006,33.4
France,2006,37.9
Germany,2006,48.2
Greece,2006,28.6
Hungary,2006,37.1
Iceland,2006,47
Ireland,2006,39.6
Israel,2006,25.8
Italy,2006,29.9
Japan,2006,27.6
Korea,2006,37.8
Luxembourg,2006,51.5
Mexico,2006,48.4
Netherlands,2006,46.4
New Zealand,2006,59
Norway,2006,36.7
Poland,2006,43.3
Portugal,2006,29.8
Slovak Republic,2006,44.2
Slovenia,2006,23
Spain,2006,30.2
Sweden,2006,41.2
Switzerland,2006,60
Turkey,2006,32.1
United Kingdom,2006,33
United States,2006,68.4
Australia,2007,47.4
Austria,2007,46.9
Belgium,2007,50.4
Canada,2007,53.3
Chile,2007,47.2
Czech Republic,2007,34.7
Denmark,2007,62.7
Estonia,2007,47.7
Finland,2007,34.5
France,2007,37.3
Germany,2007,46.1
Greece,2007,31.4
Hungary,2007,35
Iceland,2007,46.5
Ireland,2007,42.4
Israel,2007,26.1
Italy,2007,29.7
Japan,2007,30.1
Korea,2007,36.5
Luxembourg,2007,51.9
Mexico,2007,48.5
Netherlands,2007,46.9
New Zealand,2007,60.1
Norway,2007,37.5
Poland,2007,43.6
Portugal,2007,28.8
Slovak Republic,2007,40.6
Slovenia,2007,24
Spain,2007,29.8
Sweden,2007,41.6
Switzerland,2007,59
Turkey,2007,28.5
United Kingdom,2007,36.1
United States,2007,63.8
Australia,2008,47
Austria,2008,48.8
Belgium,2008,52.6
Canada,2008,51.3
Chile,2008,47.6
Czech Republic,2008,35
Denmark,2008,53.5
Estonia,2008,43.5
Finland,2008,29.9
France,2008,36.3
Germany,2008,49.5
Greece,2008,31.1
Hungary,2008,35.9
Iceland,2008,46.7
Ireland,2008,43.7
Israel,2008,25.3
Italy,2008,29.7
Japan,2008,28.4
Korea,2008,36.5
Luxembourg,2008,51.6
Mexico,2008,48.9
Netherlands,2008,46
New Zealand,2008,62.4
Norway,2008,38.4
Poland,2008,42.1
Portugal,2008,30.2
Slovak Republic,2008,51.1
Slovenia,2008,24.1
Spain,2008,30.2
Sweden,2008,40.7
Switzerland,2008,59.4
Turkey,2008,30.7
United Kingdom,2008,36.5
United States,2008,61.9
Australia,2009,45.9
Austria,2009,46.8
Belgium,2009,54.4
Canada,2009,49.5
Chile,2009,47.6
Czech Republic,2009,38.9
Denmark,2009,52.4
Estonia,2009,46.5
Finland,2009,33.8
France,2009,40.4
Germany,2009,48.1
Greece,2009,27.6
Hungary,2009,34.3
Iceland,2009,48.4
Ireland,2009,39.2
Israel,2009,26.2
Italy,2009,29.6
Japan,2009,27.6
Korea,2009,34.7
Luxembourg,2009,52.6
Mexico,2009,49.8
Netherlands,2009,47.2
New Zealand,2009,60.3
Norway,2009,37.6
Poland,2009,43.5
Portugal,2009,29
Slovak Republic,2009,52
Slovenia,2009,24.5
Spain,2009,30.3
Sweden,2009,40.5
Switzerland,2009,58.1
Turkey,2009,32.3
United Kingdom,2009,38.8
United States,2009,60.2
Australia,2010,46.1
Austria,2010,48.3
Belgium,2010,57.1
Canada,2010,46.1
Chile,2010,48.6
Czech Republic,2010,40.4
Denmark,2010,53.6
Estonia,2010,46.2
Finland,2010,30.8
France,2010,38
Germany,2010,49.9
Greece,2010,28.2
Hungary,2010,34.3
Iceland,2010,48.8
Ireland,2010,38.3
Israel,2010,27.6
Italy,2010,30
Japan,2010,25.3
Korea,2010,38.1
Luxembourg,2010,50.1
Mexico,2010,49.7
Netherlands,2010,47.1
New Zealand,2010,56.8
Norway,2010,38.2
Poland,2010,43.5
Portugal,2010,29.4
Slovak Republic,2010,52.5
Slovenia,2010,26.1
Spain,2010,30.4
Sweden,2010,38.7
Switzerland,2010,58.7
Turkey,2010,33
United Kingdom,2010,36.6
United States,2010,60.8
Australia,2011,46.5
Austria,2011,47.7
Belgium,2011,50.5
Canada,2011,50
Chile,2011,48.7
Czech Republic,2011,41.3
Denmark,2011,53.3
Estonia,2011,43.7
Finland,2011,31.3
France,2011,37.7
Germany,2011,48.7
Greece,2011,27.4
Hungary,2011,35.3
Iceland,2011,47.3
Ireland,2011,49.2
Israel,2011,27.5
Italy,2011,30.8
Japan,2011,27.9
Korea,2011,36.5
Luxembourg,2011,46.6
Mexico,2011,49
Netherlands,2011,45.5
New Zealand,2011,58.1
Norway,2011,40.3
Poland,2011,43.7
Portugal,2011,28.4
Slovak Republic,2011,48.5
Slovenia,2011,25.7
Spain,2011,31.1
Sweden,2011,40.2
Switzerland,2011,58.2
Turkey,2011,31.2
United Kingdom,2011,40.1
United States,2011,62.7
Australia,2012,45.9
Austria,2012,47.3
Belgium,2012,55.7
Canada,2012,48.2
Chile,2012,48.4
Czech Republic,2012,43.6
Denmark,2012,55.5
Estonia,2012,32.7
Finland,2012,31.7
France,2012,38
Germany,2012,47.4
Greece,2012,29.7
Hungary,2012,34.4
Iceland,2012,48.2
Ireland,2012,48.6
Israel,2012,28.1
Italy,2012,31.5
Japan,2012,27.5
Korea,2012,37.7
Luxembourg,2012,50.8
Mexico,2012,50.7
Netherlands,2012,45.4
New Zealand,2012,57.6
Norway,2012,41.7
Poland,2012,44.5
Portugal,2012,28.3
Slovak Republic,2012,44.5
Slovenia,2012,27
Spain,2012,31.2
Sweden,2012,40.2
Switzerland,2012,60.4
Turkey,2012,31.5
United Kingdom,2012,38.6
United States,2012,63.5
Australia,2013,46.1
Austria,2013,45.3
Belgium,2013,52.4
Canada,2013,48.3
Chile,2013,47.8
Czech Republic,2013,42.9
Denmark,2013,55.5
Estonia,2013,31.6
Finland,2013,31.7
France,2013,39.2
Germany,2013,48.5
Greece,2013,29.3
Hungary,2013,34.7
Iceland,2013,48.2
Ireland,2013,46
Israel,2013,27.8
Italy,2013,32.1
Japan,2013,27.1
Korea,2013,38.4
Luxembourg,2013,51.4
Mexico,2013,48.5
Netherlands,2013,44.5
New Zealand,2013,59.2
Norway,2013,43.9
Poland,2013,44.4
Portugal,2013,28.5
Slovak Republic,2013,49.7
Slovenia,2013,27.4
Spain,2013,31.2
Sweden,2013,42.1
Switzerland,2013,60.4
Turkey,2013,31.9
United Kingdom,2013,41.3
United States,2013,63.8"""
        |> fromCSV
```

```elm {l=hidden}
fruit : Table
fruit =
    """country,year,quantity
Australia,2005,108.1
Austria,2005,143.7
Belgium,2005,69.9
Canada,2005,129.3
Chile,2005,57.7
Czech Republic,2005,69.7
Denmark,2005,136.4
Estonia,2005,70.9
Finland,2005,94.3
France,2005,112.1
Germany,2005,87.1
Greece,2005,171
Hungary,2005,80.9
Iceland,2005,114.2
Ireland,2005,111.8
Israel,2005,188.6
Italy,2005,153.6
Japan,2005,60.3
Korea,2005,76.1
Luxembourg,2005,189.2
Mexico,2005,108.7
Netherlands,2005,127
New Zealand,2005,119.1
Norway,2005,131.8
Poland,2005,51.2
Portugal,2005,113.6
Slovak Republic,2005,61
Slovenia,2005,152
Spain,2005,100.2
Sweden,2005,109.7
Switzerland,2005,71.9
Turkey,2005,120.5
United Kingdom,2005,127
United States,2005,109.8
Australia,2006,100.7
Austria,2006,144.1
Belgium,2006,87.2
Canada,2006,131.9
Chile,2006,56.2
Czech Republic,2006,68.9
Denmark,2006,127
Estonia,2006,65.4
Finland,2006,89.8
France,2006,110.9
Germany,2006,84.3
Greece,2006,151.9
Hungary,2006,93.7
Iceland,2006,136.9
Ireland,2006,111.9
Israel,2006,147.2
Italy,2006,161.5
Japan,2006,55.2
Korea,2006,70.8
Luxembourg,2006,197.7
Mexico,2006,112.4
Netherlands,2006,129.6
New Zealand,2006,97
Norway,2006,134.5
Poland,2006,51.7
Portugal,2006,118.6
Slovak Republic,2006,57.7
Slovenia,2006,130.7
Spain,2006,107.6
Sweden,2006,111.6
Switzerland,2006,74.3
Turkey,2006,113.5
United Kingdom,2006,139.1
United States,2006,108.5
Australia,2007,105.2
Austria,2007,149.7
Belgium,2007,84.1
Canada,2007,142.7
Chile,2007,58.8
Czech Republic,2007,70.2
Denmark,2007,111.1
Estonia,2007,79.6
Finland,2007,93.6
France,2007,116.2
Germany,2007,83.1
Greece,2007,158.1
Hungary,2007,73.6
Iceland,2007,149.1
Ireland,2007,139.1
Israel,2007,136.4
Italy,2007,161.6
Japan,2007,57.8
Korea,2007,79.9
Luxembourg,2007,187.9
Mexico,2007,109.6
Netherlands,2007,135
New Zealand,2007,114.2
Norway,2007,142.5
Poland,2007,45.3
Portugal,2007,115.6
Slovak Republic,2007,62.9
Slovenia,2007,126
Spain,2007,86.4
Sweden,2007,117.4
Switzerland,2007,79.4
Turkey,2007,116.8
United Kingdom,2007,126.9
United States,2007,109.9
Australia,2008,101.6
Austria,2008,156.7
Belgium,2008,90.4
Canada,2008,132.8
Chile,2008,60.9
Czech Republic,2008,76.8
Denmark,2008,111
Estonia,2008,83
Finland,2008,87.7
France,2008,115.3
Germany,2008,81.1
Greece,2008,140.3
Hungary,2008,62
Iceland,2008,143.5
Ireland,2008,149.6
Israel,2008,119
Italy,2008,155.7
Japan,2008,55.1
Korea,2008,74.1
Luxembourg,2008,206.2
Mexico,2008,111.2
Netherlands,2008,125.2
New Zealand,2008,120.3
Norway,2008,148
Poland,2008,55.1
Portugal,2008,115
Slovak Republic,2008,65.8
Slovenia,2008,121.9
Spain,2008,97.4
Sweden,2008,134.1
Switzerland,2008,86.5
Turkey,2008,123.6
United Kingdom,2008,134.2
United States,2008,115.1
Australia,2009,98.7
Austria,2009,130.5
Belgium,2009,78.1
Canada,2009,134
Chile,2009,63
Czech Republic,2009,75.8
Denmark,2009,122
Estonia,2009,82.6
Finland,2009,89.8
France,2009,116
Germany,2009,81.6
Greece,2009,145.8
Hungary,2009,82.2
Iceland,2009,128
Ireland,2009,142.9
Israel,2009,138.3
Italy,2009,174.4
Japan,2009,52.5
Korea,2009,72.9
Luxembourg,2009,177.5
Mexico,2009,107.4
Netherlands,2009,111.4
New Zealand,2009,110.3
Norway,2009,129
Poland,2009,54.7
Portugal,2009,117.8
Slovak Republic,2009,67.8
Slovenia,2009,132.1
Spain,2009,76.6
Sweden,2009,117.7
Switzerland,2009,95.7
Turkey,2009,127.3
United Kingdom,2009,125.1
United States,2009,109.3
Australia,2010,100
Austria,2010,130.7
Belgium,2010,70.7
Canada,2010,133.3
Chile,2010,65.5
Czech Republic,2010,72
Denmark,2010,100.5
Estonia,2010,74.4
Finland,2010,89.5
France,2010,113
Germany,2010,78.7
Greece,2010,123.7
Hungary,2010,65.5
Iceland,2010,123.9
Ireland,2010,129.8
Israel,2010,120.5
Italy,2010,152.6
Japan,2010,49.1
Korea,2010,67.6
Luxembourg,2010,176.2
Mexico,2010,97.5
Netherlands,2010,116.2
New Zealand,2010,109
Norway,2010,127.4
Poland,2010,52.4
Portugal,2010,107.1
Slovak Republic,2010,63.7
Slovenia,2010,122.2
Spain,2010,79.7
Sweden,2010,119.3
Switzerland,2010,103
Turkey,2010,120.8
United Kingdom,2010,123.3
United States,2010,103.2
Australia,2011,94.9
Austria,2011,155.8
Belgium,2011,59.8
Canada,2011,128.6
Chile,2011,54.2
Czech Republic,2011,68.6
Denmark,2011,98.7
Estonia,2011,71.9
Finland,2011,94.6
France,2011,113.9
Germany,2011,88.2
Greece,2011,126.5
Hungary,2011,51.8
Iceland,2011,124.3
Ireland,2011,123.5
Israel,2011,108.2
Italy,2011,144
Japan,2011,51.2
Korea,2011,67.1
Luxembourg,2011,202.2
Mexico,2011,100.7
Netherlands,2011,155.1
New Zealand,2011,91.5
Norway,2011,137.8
Poland,2011,53.9
Portugal,2011,111
Slovak Republic,2011,61.7
Slovenia,2011,121.4
Spain,2011,74.9
Sweden,2011,116.9
Switzerland,2011,105.4
Turkey,2011,122.2
United Kingdom,2011,125.7
United States,2011,97.7
Australia,2012,91.5
Austria,2012,156.4
Belgium,2012,61.7
Canada,2012,125.8
Chile,2012,57.4
Czech Republic,2012,58.7
Denmark,2012,125.2
Estonia,2012,80.8
Finland,2012,100.9
France,2012,107.9
Germany,2012,87.9
Greece,2012,117.4
Hungary,2012,71.2
Iceland,2012,136.9
Ireland,2012,150.4
Israel,2012,127.5
Italy,2012,129.9
Japan,2012,54.3
Korea,2012,67.1
Luxembourg,2012,202.4
Mexico,2012,98.1
Netherlands,2012,157.1
New Zealand,2012,94
Norway,2012,140.6
Poland,2012,57.6
Portugal,2012,106.7
Slovak Republic,2012,52.5
Slovenia,2012,110.6
Spain,2012,66.2
Sweden,2012,117.7
Switzerland,2012,109.3
Turkey,2012,127.1
United Kingdom,2012,126.1
United States,2012,103.4
Australia,2013,88.2
Austria,2013,154.1
Belgium,2013,71.4
Canada,2013,135.7
Chile,2013,63.9
Czech Republic,2013,65.6
Denmark,2013,121.6
Estonia,2013,81.1
Finland,2013,95.2
France,2013,114.3
Germany,2013,88.5
Greece,2013,121.5
Hungary,2013,58.4
Iceland,2013,133.1
Ireland,2013,158.6
Israel,2013,118.8
Italy,2013,139.8
Japan,2013,52.9
Korea,2013,66.9
Luxembourg,2013,200.8
Mexico,2013,107.4
Netherlands,2013,176.1
New Zealand,2013,99
Norway,2013,140.9
Poland,2013,60.2
Portugal,2013,125.3
Slovak Republic,2013,58.1
Slovenia,2013,117.4
Spain,2013,72.2
Sweden,2013,125
Switzerland,2013,103.6
Turkey,2013,127.2
United Kingdom,2013,127.4
United States,2013,104.5"""
        |> fromCSV
```

```elm {l=hidden}
vegetables : Table
vegetables =
    """country,year,quantity
Australia,2005,97.8
Austria,2005,93.1
Belgium,2005,124.6
Canada,2005,119.2
Chile,2005,101.8
Czech Republic,2005,79
Denmark,2005,97.7
Estonia,2005,83.2
Finland,2005,78.7
France,2005,104.2
Germany,2005,86.2
Greece,2005,262.8
Hungary,2005,108
Iceland,2005,65.2
Ireland,2005,71.6
Israel,2005,199.2
Italy,2005,183.4
Japan,2005,107.8
Korea,2005,215.8
Luxembourg,2005,87.6
Mexico,2005,63.4
Netherlands,2005,86.1
New Zealand,2005,122.1
Norway,2005,74.1
Poland,2005,114.9
Portugal,2005,187.1
Slovak Republic,2005,74.9
Slovenia,2005,87.3
Spain,2005,155.8
Sweden,2005,84.2
Switzerland,2005,88
Turkey,2005,249
United Kingdom,2005,95.9
United States,2005,123
Australia,2006,97.5
Austria,2006,94.8
Belgium,2006,122.6
Canada,2006,121.1
Chile,2006,99.9
Czech Republic,2006,77.6
Denmark,2006,99.6
Estonia,2006,82.2
Finland,2006,74.6
France,2006,97.6
Germany,2006,88
Greece,2006,237.9
Hungary,2006,128.9
Iceland,2006,72.3
Ireland,2006,80
Israel,2006,181.7
Italy,2006,160.2
Japan,2006,106.1
Korea,2006,224.1
Luxembourg,2006,89.9
Mexico,2006,61.7
Netherlands,2006,94.6
New Zealand,2006,151.7
Norway,2006,75.7
Poland,2006,112.9
Portugal,2006,151.7
Slovak Republic,2006,85.8
Slovenia,2006,87.6
Spain,2006,147
Sweden,2006,88.8
Switzerland,2006,88.3
Turkey,2006,239.8
United Kingdom,2006,93.4
United States,2006,125.9
Australia,2007,99.7
Austria,2007,95.9
Belgium,2007,129.1
Canada,2007,122.6
Chile,2007,76
Czech Republic,2007,75.4
Denmark,2007,97.2
Estonia,2007,99.5
Finland,2007,79.1
France,2007,98.2
Germany,2007,86.6
Greece,2007,241.6
Hungary,2007,113.9
Iceland,2007,76.5
Ireland,2007,82.2
Israel,2007,177.3
Italy,2007,157.7
Japan,2007,104.8
Korea,2007,215.1
Luxembourg,2007,86.9
Mexico,2007,64.6
Netherlands,2007,105.7
New Zealand,2007,129.5
Norway,2007,77.6
Poland,2007,127.2
Portugal,2007,177.1
Slovak Republic,2007,88.9
Slovenia,2007,76.9
Spain,2007,154.7
Sweden,2007,86.1
Switzerland,2007,90.5
Turkey,2007,234.6
United Kingdom,2007,91.6
United States,2007,128.8
Australia,2008,102.3
Austria,2008,101.8
Belgium,2008,129.2
Canada,2008,114.1
Chile,2008,79.8
Czech Republic,2008,77.6
Denmark,2008,100.1
Estonia,2008,96.4
Finland,2008,78
France,2008,100.8
Germany,2008,87.9
Greece,2008,230.5
Hungary,2008,115
Iceland,2008,72.7
Ireland,2008,88.1
Israel,2008,168
Italy,2008,153.3
Japan,2008,102.9
Korea,2008,222.8
Luxembourg,2008,89
Mexico,2008,62
Netherlands,2008,103.5
New Zealand,2008,131.7
Norway,2008,81.7
Poland,2008,113.1
Portugal,2008,164.4
Slovak Republic,2008,97.8
Slovenia,2008,84.9
Spain,2008,145.4
Sweden,2008,88.8
Switzerland,2008,95.4
Turkey,2008,244.2
United Kingdom,2008,93
United States,2008,116.4
Australia,2009,97
Austria,2009,101.2
Belgium,2009,128.9
Canada,2009,113.1
Chile,2009,76.9
Czech Republic,2009,72
Denmark,2009,116.9
Estonia,2009,107.2
Finland,2009,84.7
France,2009,105.6
Germany,2009,91.7
Greece,2009,246.3
Hungary,2009,105
Iceland,2009,69.5
Ireland,2009,87.2
Israel,2009,175.3
Italy,2009,174.8
Japan,2009,100.6
Korea,2009,217.8
Luxembourg,2009,79
Mexico,2009,55
Netherlands,2009,88.4
New Zealand,2009,140.9
Norway,2009,78.9
Poland,2009,126.1
Portugal,2009,191.6
Slovak Republic,2009,95.1
Slovenia,2009,87.7
Spain,2009,158.4
Sweden,2009,91.8
Switzerland,2009,99.8
Turkey,2009,235.3
United Kingdom,2009,88.7
United States,2009,121.8
Australia,2010,97.8
Austria,2010,105.7
Belgium,2010,117.1
Canada,2010,110.7
Chile,2010,65.6
Czech Republic,2010,70.5
Denmark,2010,120.8
Estonia,2010,109.6
Finland,2010,80.2
France,2010,103.1
Germany,2010,90
Greece,2010,219.8
Hungary,2010,70.5
Iceland,2010,68.5
Ireland,2010,91.5
Israel,2010,163
Italy,2010,148.1
Japan,2010,98.9
Korea,2010,196.5
Luxembourg,2010,110.1
Mexico,2010,56.2
Netherlands,2010,78.4
New Zealand,2010,123.4
Norway,2010,71.8
Poland,2010,113.6
Portugal,2010,201.4
Slovak Republic,2010,92.7
Slovenia,2010,75
Spain,2010,140.5
Sweden,2010,88.2
Switzerland,2010,104
Turkey,2010,227
United Kingdom,2010,92.8
United States,2010,120.1
Australia,2011,95.8
Austria,2011,109.3
Belgium,2011,129.2
Canada,2011,113
Chile,2011,68.7
Czech Republic,2011,74
Denmark,2011,121.9
Estonia,2011,122
Finland,2011,88
France,2011,99.5
Germany,2011,94.1
Greece,2011,237.9
Hungary,2011,94
Iceland,2011,69.1
Ireland,2011,96.1
Israel,2011,167.9
Italy,2011,144.4
Japan,2011,101.3
Korea,2011,221.7
Luxembourg,2011,109.3
Mexico,2011,51.6
Netherlands,2011,83.2
New Zealand,2011,121.2
Norway,2011,77.8
Poland,2011,129.4
Portugal,2011,164.8
Slovak Republic,2011,95.3
Slovenia,2011,79.9
Spain,2011,125.5
Sweden,2011,93.9
Switzerland,2011,113.4
Turkey,2011,241
United Kingdom,2011,96.9
United States,2011,116.4
Australia,2012,99.9
Austria,2012,103.8
Belgium,2012,130.3
Canada,2012,115.2
Chile,2012,72
Czech Republic,2012,68.2
Denmark,2012,109.3
Estonia,2012,104.3
Finland,2012,82.4
France,2012,98.3
Germany,2012,96.9
Greece,2012,227.8
Hungary,2012,80.9
Iceland,2012,72.3
Ireland,2012,97.1
Israel,2012,172.7
Italy,2012,133.8
Japan,2012,104.4
Korea,2012,197.8
Luxembourg,2012,107.8
Mexico,2012,59.8
Netherlands,2012,77.3
New Zealand,2012,134
Norway,2012,73.3
Poland,2012,119.8
Portugal,2012,176.3
Slovak Republic,2012,62.6
Slovenia,2012,74.8
Spain,2012,126.2
Sweden,2012,98.2
Switzerland,2012,108.3
Turkey,2012,239.1
United Kingdom,2012,92.9
United States,2012,118.2
Australia,2013,102.8
Austria,2013,103.7
Belgium,2013,136.4
Canada,2013,108.5
Chile,2013,71.7
Czech Republic,2013,72.2
Denmark,2013,105.3
Estonia,2013,115.3
Finland,2013,88.8
France,2013,97.3
Germany,2013,92.9
Greece,2013,224.4
Hungary,2013,78.9
Iceland,2013,70.1
Ireland,2013,93.9
Israel,2013,174.3
Italy,2013,128.9
Japan,2013,102.3
Korea,2013,205.9
Luxembourg,2013,107.4
Mexico,2013,55.9
Netherlands,2013,86.3
New Zealand,2013,129.5
Norway,2013,77.2
Poland,2013,107.7
Portugal,2013,152.1
Slovak Republic,2013,60.5
Slovenia,2013,80.3
Spain,2013,119.2
Sweden,2013,94.4
Switzerland,2013,108.7
Turkey,2013,241.3
United Kingdom,2013,97
United States,2013,114"""
        |> fromCSV
```

```elm {l=hidden}
yearlyProductAverages : Table
yearlyProductAverages =
    """year,product type,annual average per capita
2005,alcohol,9.7
2006,alcohol,9.726
2007,alcohol,9.679
2008,alcohol,9.52
2009,alcohol,9.22
2010,alcohol,9.14
2011,alcohol,9.12
2012,alcohol,9
2013,alcohol,8.8
2005,fruit,109.66
2006,fruit,108.23
2007,fruit,109.55
2008,fruit,110.47
2009,fruit,107.9
2010,fruit,102.41
2011,fruit,103
2012,fruit,105.24
2013,fruit,108
2005,all sugar,42.73
2006,all sugar,41.66
2007,all sugar,41.95
2008,all sugar,41.95
2009,all sugar,42
2010,all sugar,41.97
2011,all sugar,42
2012,all sugar,42.23
2013,all sugar,42.38
2005,vegetables,116.84
2006,vegetables,115.92
2007,vegetables,116.15
2008,vegetables,115.66
2009,vegetables,117.94
2010,vegetables,112.44
2011,vegetables,116.1
2012,vegetables,112.82
2013,vegetables,111.91"""
        |> fromCSV
```
