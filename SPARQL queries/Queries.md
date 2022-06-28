## Ontology Competency Questions

We designed a set of competency questions to reflect the main features of Doc-Bias.


**_Q1:   Who are the actors interacting together to propagate bias?_**

```
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix owl: <http://www.w3.org/2002/07/owl#>
prefix nobias: <https://github.com/tibonto/Doc-BIAS/> 

select ?sourceActor ?recipientActor 
where{
	?biasEvent			rdf:type				nobias:BiasEvent;
					nobias:hasRecipientParticipant		?recipientParticipant;
					nobias:hasSourceParticipant		?sourceParticipant.
	
    	?sourceParticipant		nobias:providesSourceRole		?sourceAgent.
	?recipientParticipant		nobias:providesRecipientRole		?recipientAgent.

	?sourceAgent			nobias:hasRecipient			?recipientAgent.

	?sourceActor			nobias:performsSourceRole		?sourceAgent.
	?recipientActor			nobias:performsRecipientRole		?recipientAgent.
}

```


**_Q3:   What type of bias is being propagated, and how was it detected?_**

```
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix owl: <http://www.w3.org/2002/07/owl#>
prefix nobias: <https://github.com/tibonto/Doc-BIAS/> 

select ?bias ?detectionMethod
where{
	?biasEvent		rdf:type					nobias:BiasEvent;
				nobias:hasBiasType				?bias;
				nobias:hasDetectionMethod			?detectionMethod.
}

```

**_Q4:   What is the profile of the Actor in terms of diversity?_**


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


**_Q5:  What is the relationship between the classified News and the Actor profile?_**

```
prefix xsd: <http://www.w3.org/2001/XMLSchema#>
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
prefix nobias: <https://github.com/tibonto/Doc-BIAS/> 
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix owl: <http://www.w3.org/2002/07/owl#>

select ?groundTruth ?newsLabel ?consistencyCheck
where{
	?news 			rdf:type 		nobias:News;
       				nobias:hasGroundTruth 	?groundTruth;
       				nobias:hasModelOutput 	?newsClassification.

  	?newsClassification 	nobias:hasLabel 	?newsLabel.

  	BIND( if(((?groundTruth = "1"^^xsd:int && ?newsLabel = "Fake News"^^xsd:string) 
          || (?groundTruth = "0"^^xsd:int && ?newsLabel = "Real News"^^xsd:string))
          , "consistent", "inconsistent")   AS ?consistencyCheck)
}

```