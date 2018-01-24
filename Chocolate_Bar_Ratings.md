Chocolate-Bar-Ratings
================
2018-01-24

This is a dataset of 1,700 individual chocolate bars. This dataset has information on their regional origin, percentage of cocoa, the variety of chocolate bean used and where the beans were grown. I think it would be fun to use some R code to take a look at what chocolate bar gets the highest rating over the years, what countries has the highest rated bars and what goes into getting a highly rated chocolate bar.

A look at the data:

``` r
kable(head(cocoa), format = "html")
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Company...Maker.if.known.
</th>
<th style="text-align:left;">
Specific.Bean.Origin.or.Bar.Name
</th>
<th style="text-align:right;">
REF
</th>
<th style="text-align:right;">
Review.Date
</th>
<th style="text-align:left;">
Cocoa.Percent
</th>
<th style="text-align:left;">
Company.Location
</th>
<th style="text-align:right;">
Rating
</th>
<th style="text-align:left;">
Bean.Type
</th>
<th style="text-align:left;">
Broad.Bean.Origin
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
A. Morin
</td>
<td style="text-align:left;">
Agua Grande
</td>
<td style="text-align:right;">
1876
</td>
<td style="text-align:right;">
2016
</td>
<td style="text-align:left;">
63%
</td>
<td style="text-align:left;">
France
</td>
<td style="text-align:right;">
3.75
</td>
<td style="text-align:left;">
</td>
<td style="text-align:left;">
Sao Tome
</td>
</tr>
<tr>
<td style="text-align:left;">
A. Morin
</td>
<td style="text-align:left;">
Kpime
</td>
<td style="text-align:right;">
1676
</td>
<td style="text-align:right;">
2015
</td>
<td style="text-align:left;">
70%
</td>
<td style="text-align:left;">
France
</td>
<td style="text-align:right;">
2.75
</td>
<td style="text-align:left;">
</td>
<td style="text-align:left;">
Togo
</td>
</tr>
<tr>
<td style="text-align:left;">
A. Morin
</td>
<td style="text-align:left;">
Atsane
</td>
<td style="text-align:right;">
1676
</td>
<td style="text-align:right;">
2015
</td>
<td style="text-align:left;">
70%
</td>
<td style="text-align:left;">
France
</td>
<td style="text-align:right;">
3.00
</td>
<td style="text-align:left;">
</td>
<td style="text-align:left;">
Togo
</td>
</tr>
<tr>
<td style="text-align:left;">
A. Morin
</td>
<td style="text-align:left;">
Akata
</td>
<td style="text-align:right;">
1680
</td>
<td style="text-align:right;">
2015
</td>
<td style="text-align:left;">
70%
</td>
<td style="text-align:left;">
France
</td>
<td style="text-align:right;">
3.50
</td>
<td style="text-align:left;">
</td>
<td style="text-align:left;">
Togo
</td>
</tr>
<tr>
<td style="text-align:left;">
A. Morin
</td>
<td style="text-align:left;">
Quilla
</td>
<td style="text-align:right;">
1704
</td>
<td style="text-align:right;">
2015
</td>
<td style="text-align:left;">
70%
</td>
<td style="text-align:left;">
France
</td>
<td style="text-align:right;">
3.50
</td>
<td style="text-align:left;">
</td>
<td style="text-align:left;">
Peru
</td>
</tr>
<tr>
<td style="text-align:left;">
A. Morin
</td>
<td style="text-align:left;">
Carenero
</td>
<td style="text-align:right;">
1315
</td>
<td style="text-align:right;">
2014
</td>
<td style="text-align:left;">
70%
</td>
<td style="text-align:left;">
France
</td>
<td style="text-align:right;">
2.75
</td>
<td style="text-align:left;">
Criollo
</td>
<td style="text-align:left;">
Venezuela
</td>
</tr>
</tbody>
</table>
I need to remove the % sign in Cocoa.Percent and make it an int.

``` r
cocoa$Cocoa.Percent<-gsub("%","",cocoa$Cocoa.Percent)
cocoa$Cocoa.Percent <- as.integer(cocoa$Cocoa.Percent)
str(cocoa)
```

    ## 'data.frame':    1795 obs. of  9 variables:
    ##  $ Company...Maker.if.known.       : Factor w/ 416 levels "A. Morin","Acalli",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ Specific.Bean.Origin.or.Bar.Name: Factor w/ 1039 levels "\"heirloom\", Arriba Nacional",..: 15 494 68 16 813 175 288 923 805 731 ...
    ##  $ REF                             : int  1876 1676 1676 1680 1704 1315 1315 1315 1319 1319 ...
    ##  $ Review.Date                     : int  2016 2015 2015 2015 2015 2014 2014 2014 2014 2014 ...
    ##  $ Cocoa.Percent                   : int  63 70 70 70 70 70 70 70 70 70 ...
    ##  $ Company.Location                : Factor w/ 60 levels "Amsterdam","Argentina",..: 19 19 19 19 19 19 19 19 19 19 ...
    ##  $ Rating                          : num  3.75 2.75 3 3.5 3.5 2.75 3.5 3.5 3.75 4 ...
    ##  $ Bean.Type                       : Factor w/ 41 levels " ","Amazon","Amazon mix",..: 1 1 1 1 1 9 1 9 9 1 ...
    ##  $ Broad.Bean.Origin               : Factor w/ 100 levels " ","Africa, Carribean, C. Am.",..: 69 79 79 79 56 92 17 92 92 56 ...

Ok now lets see a distribution of the Ratings and percent of cocoa.

``` r
ggplot(cocoa, aes(x=Rating)) +
      geom_histogram(color = "black", fill = "red", binwidth = .25) +
      scale_y_continuous(breaks = seq(0,400,25)) +
      labs(x = "Ratings", title = "Cocoa Ratings", y = "Count")
```

![](Chocolate_Bar_Ratings_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-5-1.png)

Not bad most in the 2.5 to 4 range. The mean is 3.186 and the median is 3.25
