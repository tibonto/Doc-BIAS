## Ontology Competency Questions


**_Q1:   _**


```
prefix dct: <http://purl.org/dc/terms/> 
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
prefix dcat: <http://www.w3.org/ns/dcat#> 
prefix prov: <http://www.w3.org/ns/prov#> 
prefix nobias: <https://github.com/tibonto/Doc-BIAS/> 
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix owl: <http://www.w3.org/2002/07/owl#>

select ?publisher ?language ?timePeriod ?keyword ?contentType
where{
	?publisher		rdf:type				nobias:NewsSource;
				prov:wasGeneratedBy 			?provActivity.
	?provActivity 		prov:used 				?dataset .
	?dataset		dct:language 				?language;
				dct:temporal 				?timePeriod;
				dcat:Keyword 				?keyword;
				nobias:hasContentType			?contentType. 

}

```


**_Q2:   _**