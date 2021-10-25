# Automated Climate Analyses Using Knowledge Graph
With the spirit of reproducible research, this repository includes a complete collection of codes required to generate the results and the diagrams presented in the paper:

> J. Wu, H. Chen, F. Orlandi, Y.H. Lee, D. O'Sullivan, S. Dev, Automated Climate Analyses Using Knowledge Graph, *AP-S/URSI 2021*.


## Instruction to Perform Climate Analysis with Our knowledge Graph
The process to get the results and pictures presented in the paper can be divided into the following two steps:
### Step 1&mdash;Obtaining the data using SPARQL queries
In the paper, we demonstrated two kinds of weather analyses: 1) the average temperature comparison between Dublin (IE) and Manston (UK) and 2) Sculthorpe (UK) weather analyses in terms of various weather type. The weather data for those two cities can be fetched by submitting the following two pieces of SPARQL queries to our [SPARQL endpoint](http://jresearch.ucd.ie/kg/dataset.html?tab=query&ds=/climate) :

For Dublin's and Manston's daily average temperature, use

```
BASE <http://jresearch.ucd.ie/climate-kg/>
PREFIX ca_prop: <http://jresearch.ucd.ie/climate-kg/ca/property/>
PREFIX sosa: <http://www.w3.org/ns/sosa/>
PREFIX qudt: <http://qudt.org/1.1/schema/qudt#>

select ?tavg ?date
where {
  ?obs ca_prop:sourceStation <resource/station/GHCND:EI000003969> ; # switch the code to GHCND:UKE00105918 for Manston's temperature
       sosa:hasResult ?res ;
       sosa:resultTime ?date .
  ?res ca_prop:withDataType <resource/datatype/TAVG>;
       qudt:numericValue ?tavg.
  filter(year(?date)>1979 && year(?date)<2020)
} ORDER BY DESC(?date)
```

For Manston's weather type data, use

```
BASE <http://jresearch.ucd.ie/climate-kg/>
PREFIX ca_prop: <http://jresearch.ucd.ie/climate-kg/ca/property/>
PREFIX sosa: <http://www.w3.org/ns/sosa/>
PREFIX qudt: <http://qudt.org/1.1/schema/qudt#>

select ?fog ?date
where{
  ?obs ca_prop:sourceStation <resource/station/GHCND:UKW00035036>;
       sosa:hasResult ?res ;
       sosa:resultTime ?date .
  ?res ca_prop:withDataType <resource/datatype/WT01>;
    qudt:numericValue ?fog.
  filter(year(?date)>1950 && year(?date)<1964).}
```
where we only give the SPARQL query for weather type WT01&mdash;"Fog, ice fog, or freezing fog (may include heavy fog)". The complete set of code for all weather type and their meanings can be found in the following table.

DATA TYPE | DESCRIPTION
--- | ---
WT01 | Fog, ice fog, or freezing fog (may include heavy fog)
WT03 | Thunder
WT04 | Ice pellets, sleet, snow pellets, or small hail"
WT05 | Hail (may include small hail)
WT06 | Glaze or rime
WT08 | Smoke or haze
WT09 | Blowing or drifting snow
WT16 | Rain (may include freezing rain, drizzle, and freezing drizzle)
WT18 | Snow, snow pellets, snow grains, or ice crystals


The solutions of these queries should be identical to the two pre-downloaded datasets `data/dublin.csv`, `data/manston.csv` ...

### Step 2&mdash;Weather Data Analysis through Python Notebook
See the in-line comments in the notebook `climate_analysis.ipynb`. The SPARQL query solutions as the raw data input and weather analyses snippets are indicated.

