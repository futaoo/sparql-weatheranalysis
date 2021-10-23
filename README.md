# Automated Climate Analyses Using Knowledge Graph
With the spirit of reproducible research, this repository includes a complete collection of codes required to generate the results and the diagrams presented in the paper:

> J. Wu, H. Chen, F. Orlandi, Y.H. Lee, D. O'Sullivan, S. Dev, Automated Climate Analyses Using Knowledge Graph, *AP-S/URSI 2021*.


## Instruction to Perform Climate Analysis with Our knowledge Graph
The process to get the results and pictures presented in the paper can be divided into the following two steps:
### Step 1--Obtaining the data using SPARQL queries
In the paper, we demonstrated the weather comparison between Dublin (IE) and Manston (UK). The weather data for those two cities can be fetched by submitting the following two pieces of SPARQL queries to our [SPARQL endpoint](http://jresearch.ucd.ie/kg/dataset.html?tab=query&ds=/climate) :

For Dublin's weather, use

```
BASE <http://jresearch.ucd.ie/climate-kg/>
PREFIX ca_prop: <http://jresearch.ucd.ie/climate-kg/ca/property/>
PREFIX sosa: <http://www.w3.org/ns/sosa/>
PREFIX qudt: <http://qudt.org/1.1/schema/qudt#>

select ?tavg ?date
where {
  ?obs ca_prop:sourceStation <resource/station/GHCND:EI000003969> ;
       sosa:hasResult ?res ;
       sosa:resultTime ?date .
  ?res ca_prop:withDataType <resource/datatype/TAVG>;
       qudt:numericValue ?tavg.
  filter(year(?date)>1979 && year(?date)<2020)
} ORDER BY DESC(?date)
```

For Manston's weather, use

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


