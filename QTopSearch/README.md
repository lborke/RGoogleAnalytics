
[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="888" alt="Visit QuantNet">](http://quantlet.de/)

## [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **QTopSearch** [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/)

```yaml

Name of Quantlet : QTopSearch

Published in : GitHub API based QuantNet Mining infrastructure in R, Section "Google Analytics"

Description : 'Retrieves the most frequent search queries entered into the search field of the
QuantNet visualization, called QuantNetXploRer. After every keystroke the search queries are
tracked via Google Analytics. For better analysis, search queries with less than 3 characters are
omitted. The API query specifies the parameters dimensions, metrics and filters conditioning the
desired Event Tracking criteria.'

Keywords : 'Google Analytics, API, RGoogleAnalytics, Web Metrics, data mining, QuantNet,
QuantNetXploRer, search queries, analysis, statistics'

See also : DownloadsByCountry, TopDownloads

Author : Lukas Borke

Submitted : 18.02.2017 by Lukas Borke

Example : QTopSearch_trimmed.md

```

|SearchQuery  | frequency|
|:------------|---------:|
|(not set)    |       780|
|sfe          |       111|
|MVA          |        94|
|SMS          |        53|
|bor          |        45|
|borke        |        45|
|SFE          |        42|
|ming         |        42|
|mmstat       |        41|
|mms          |        37|
|min          |        36|
|mva          |        36|
|SMSlinregcar |        35|
|mingy        |        35|
|mmsta        |        35|
|LDA          |        30|
|TENET        |        26|
|mmst         |        26|
|bork         |        25|
|gen          |        25|
|gensi        |        25|
|VaR          |        24|
|pele         |        24|
|covar        |        22|
|trim         |        21|
|CoVaR        |        19|
|mingyan      |        19|
|multivariate |        19|
|sfe_         |        19|
|value        |        19|
|GUO          |        18|
|applied      |        18|
|sfs          |        18|
|tedas        |        18|
|xfg          |        18|
|CoV          |        17|
|mingya       |        17|
|byko         |        16|
|copula       |        16|
|lin          |        16|
|SFEVaRbank   |        15|
|byk          |        15|
|cop          |        15|
|cov          |        15|
|regression   |        15|


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

s_date = "2016-11-01"
e_date = "2017-02-14"

###########################################################################
# QNet2 top search queries
###########################################################################
query.list.search = Init(	start.date = s_date,
							end.date = e_date,
							dimensions = "ga:eventLabel",
							metrics = "ga:totalEvents",
							filters = "ga:eventCategory==QNet2Visu;ga:eventAction==search",
							sort = "-ga:totalEvents",
							max.results = 1000,
							table.id = "ga:134092861")

ga.query = QueryBuilder(query.list.search)
ga.df	 = GetReportData(ga.query, oauth_token)

# Parameters for the length and the column names of the output
samplesize	= 100
columnnames = c("SearchQuery", "frequency")
colnames(ga.df) = columnnames
qn_top_search_frm = ga.df[1:samplesize,]

# output as data frame, top search queries as defined by samplesize
qn_top_search_frm

# trimmed output omitting search queries with less than 3 characters
qn_top_search_frm_trimmed = qn_top_search_frm[nchar(qn_top_search_frm$SearchQuery) >= 3,]


# convert to Latex output
print(xtable(qn_top_search_frm_trimmed, align = "|r|l|r|", digits = 0), include.rownames = F)

# convert to Markdown format
( k_t = kable(qn_top_search_frm_trimmed, row.names = F) )
writeLines(k_t, con = "QTopSearch_trimmed.md")

```
