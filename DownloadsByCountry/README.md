
[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="888" alt="Visit QuantNet">](http://quantlet.de/)

## [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **DownloadsByCountry** [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/)

```yaml

Name of Quantlet : DownloadsByCountry

Published in : GitHub API based QuantNet Mining infrastructure in R, Section "Google Analytics"

Description : 'Extracts recent download statistics from Google Analytics starting November 2013 for
each Quantlet. Top ten countries are taken and the final results are presented as an R data frame.
The API query specifies the parameters dimensions, metrics and filters conditioning the desired
Event Tracking criteria.'

Keywords : 'Google Analytics, API, RGoogleAnalytics, Web Metrics, data mining, QuantNet, download
statistics, statistics'

See also : TopDownloads, QTopSearch

Author : Lukas Borke

Submitted : 18.02.2017 by Lukas Borke

Example : DownloadsByCountry.md

```


|Country        | Downloads|
|:--------------|---------:|
|Germany        |     37042|
|United States  |      9735|
|China          |      9266|
|Bulgaria       |      2502|
|Russia         |      2356|
|United Kingdom |      2265|
|India          |      1653|
|Italy          |      1643|
|Japan          |      1609|
|France         |      1335|


### R Code:
```r

# Clear all variables
rm(list = ls(all = TRUE))
graphics.off()

# Install and load packages
libraries = c("RGoogleAnalytics", "httpuv", "xtable", "knitr")
lapply(libraries, function(x) if (!(x %in% installed.packages())) {
  install.packages(x)
})
lapply(libraries, library, quietly = TRUE, character.only = TRUE)

# setwd("C:/R/GoA/QNet")

oauth_token = Auth(client.id = "XXX", client.secret = "YYY")

GetProfiles(oauth_token)

s_date = "2013-11-16"
e_date = "2016-11-22"

###########################################################################
# Quantlet downloads by country
###########################################################################
query.list.Country = Init(	start.date = s_date,
							end.date = e_date,
							dimensions = "ga:country",
							metrics = "ga:totalEvents",
							filters = "ga:eventCategory==QNetShow",
							sort = "-ga:totalEvents",
							max.results = 1000,
							table.id = "ga:78690351")

ga.query = QueryBuilder(query.list.Country)
ga.df	 = GetReportData(ga.query, oauth_token)

# Parameters for the length and the column names of the output
samplesize  = 10
columnnames = c("Country", "Downloads")
colnames(ga.df) = columnnames
qn_top_frm = ga.df[1:samplesize,]

# output as data frame, top countries as defined by samplesize
qn_top_frm

# convert to Latex output
print(xtable(qn_top_frm, align = "|r|l|r|", digits = 0), include.rownames = F)

# convert to Markdown format
( k_t = kable(qn_top_frm, row.names = F) )
writeLines(k_t, con = "DownloadsByCountry.md")

```
