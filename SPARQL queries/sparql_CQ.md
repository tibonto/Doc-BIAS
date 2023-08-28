## Given a particular bias, 

### (Q1) what is its definition?  

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX nobias: <https://nobias-project.eu/nobias/>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT DISTINCT ?bias_1 ?definition_1
WHERE { ?bias_1 rdfs:subClassOf|owl:equivalentClass nobias:Bias  ;
                 skos:definition ?definition_1 
   # FILTER ( (  REGEX(str(?bias_1), "Representation", 'i')))
}

### (Q2) what are the AI applications associated to it? 

PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  nobias: <https://nobias-project.eu/nobias/>

SELECT DISTINCT  ?bias_1 ?app_1
WHERE { ?bias_1 rdfs:subClassOf|rdf:type nobias:Bias .
        ?app_1   rdfs:subClassOf|rdf:type       nobias:Application .
        ?bias_1  nobias:isAssociatedTo  ?app_1
  #  FILTER ( (  REGEX(str(?bias_1), "Representation", 'i')))
  }

### (Q3) what AI harm is aligned with it? 

PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX  nobias: <https://nobias-project.eu/nobias/>

SELECT DISTINCT  ?bias_1 ?harm_1
WHERE  { ?bias_1 rdf:type nobias:Bias .
          ?harm_1  rdf:type  nobias:Harm .
          ?bias_1 nobias:isAlignedWith ?harm_1
       }
   

### (Q4.1) how many measures have been defined for it? 

PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  nobias: <https://nobias-project.eu/nobias/>
PREFIX  owl:  <http://www.w3.org/2002/07/owl#>
 
SELECT DISTINCT  ?bias_1 (COUNT(DISTINCT ?biasMeasure_1) AS ?number_of_measures)
WHERE { ?bias_1  rdfs:subClassOf|owl:equivalentClass nobias:Bias .
        ?biasMeasure_1 nobias:measures|owl:someValuesFrom ?bias_1

     }
GROUP BY ?bias_1

### (Q4.2) what measures have been documented for it? 

PREFIX nobias: <https://nobias-project.eu/nobias/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT DISTINCT ?bias_1 ?biasMeasure_1 
WHERE { 
        ?bias_1 rdf:type nobias:Bias . 
        ?biasMeasure_1 rdf:type nobias:BiasMeasure .
        ?bias_1  nobias:hasBiasMeasure ?biasMeasure_1 .
    }

## Given a bias measure,    

### (Q5) in which scholarly document is it defined? 

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX nobias: <https://nobias-project.eu/nobias/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX prov: <http://www.w3.org/ns/prov#>

SELECT DISTINCT ?biasMeasure_1 ?publishedBy_1 { 
        ?biasMeasure_1 rdf:type nobias:BiasMeasure .
        ?published_1 rdf:type foaf:term_Document .
        ?biasMeasure_1  prov:wasAttributedTo  ?publishedBy_1 
    
 # FILTER ( (  REGEX(str(?biasMeasure_1), "Gini", 'i')))
    }
                                           
### (Q6) what is its formalization? 

PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX nobias: <https://nobias-project.eu/nobias/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT DISTINCT ?biasMeasure_1 ?definition_1  ?formalization_1 
WHERE { 
        ?biasMeasure_1 rdfs:subClassOf|rdf:type nobias:BiasMeasure ;
                       skos:definition ?definition_1 ;
                       nobias:formalization ?formalization_1
 # FILTER ( (  REGEX(str(?biasMeasure_1), "Gini", 'i')))
}

### (Q7) what AI task is evaluated by it? 

PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX nobias: <https://nobias-project.eu/nobias/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT DISTINCT ?biasMeasure_1 ?task_1 { 
        ?biasMeasure_1 rdfs:subClassOf nobias:BiasMeasure .     
    OPTIONAL {
        ?evaluates_1 rdf:type nobias:BiasEvaluation ; 
                     nobias:evaluatesWithMeasure ?biasMeasure_1; 
                     nobias:evaluatesForTask ?task_1  }
 # FILTER ( (  REGEX(str(?biasMeasure_1), "Gini", 'i')))
}
                         
### (Q8) what dataset feature is evaluated by it? 

PREFIX nobias: <https://nobias-project.eu/nobias/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT DISTINCT ?biasMeasure_1 ?feature_1 
WHERE { ?evaluates_1 rdf:type nobias:BiasEvaluation ;
                     nobias:evaluatesWithMeasure ?biasMeasure_1; 
                     nobias:evaluatesFeature ?feature_1 . 
 #  FILTER ( (  REGEX(str(?biasMeasure_1), "Gini", 'i')))
      }

### (Q9) Following the implementation of a bias measure, what is the score of the evaluated bias measure? 

PREFIX  nobias: <https://nobias-project.eu/nobias/>
PREFIX  rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT DISTINCT ?biasEvaluation1  ?biasMeasure_1 ?score
WHERE { ?biasEvaluation1 rdf:type  nobias:BiasEvaluation ;
                         nobias:evaluatesWithMeasure  ?biasMeasure_1 ;
                         nobias:biasScore     ?score
     }