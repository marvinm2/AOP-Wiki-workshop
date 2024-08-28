<img src="pics/VHPlogo.png" alt="VHP4Safety" class="bg-primary mb-1" width="300px" align="right">

# Querying the AOP-Wiki using SPARQL

[Back to overview](README.md)

<script>
  function toggleAnswer(id) {
  var answer = document.getElementById(id);
  if (answer.style.visibility === "hidden" ||
      answer.style.visibility === "none") {
    answer.style.visibility = "visible";
  } else {
    answer.style.visibility = "hidden";
  }
}
</script>

## Introduction
Welcome to the AOP-Wiki SPARQL endpoint workshop. The exercises are meant as a step-by-step introduction to constructing and adapting SPARQL queries.

### AOP-Wiki general description
The AOP-Wiki serves as the primary repository of qualitative information for AOPs and is a central component in the AOP development effort coordinated by the Organisation for Economic Co-operation and Development (OECD). These AOPs describe mechanistic information about toxicodynamic processes and can be used to develop effective risk assessment strategies. An AOP is initiated by a stressor (e.g. a chemical) that causes a Molecular Initiating Event, which is followed by Key Eevents (measurable, essential steps) along a pathway towards an Adverse Outcome for an organism or population. KEs are connected through Key Event Relationships (KERs), which capture the evidence supporting the AOP in a structured way. 

### AOP-Wiki SPARQL endpoint
The AOP-Wiki SPARQL endpoint is loaded with RDF of the Adverse Outcome Pathway (AOP)-Wiki database [aopwiki.org](https://aopwiki.org/). It is used by both the SNORQL UI and REST API to explore the data. The AOP-Wiki SPARQL endpoint is accessible on [aopwiki.cloud.vhp4safety.nl/sparql/](https://aopwiki.cloud.vhp4safety.nl/sparql/). This is also the SPARQL endpoint URL that should be used when working from coding environments, these exercises could also be done through R or Python, for example.

## Figure of RDF schema
:::{figure-md} markdown-fig
<img src="AOP-Wiki_RDF_simple.png" alt="AOP-Wiki RDF simple" class="bg-primary mb-1" width="200px">
This is a simplified version of the AOP-Wiki RDF schema. Major subjects are represented in green, and their properties are highlighted in grey. The predicates are indicated on the arrows. For the complete schema description, see [https://doi.org/10.1089/aivt.2021.0010](https://doi.org/10.1089/aivt.2021.0010).
:::


## Exercises
### Exercise 1 - Listing of subjects
The simplest SPARQL queries to explore RDF is to retrieve full lists of subjects of a particular type, which is frequently defined with the predicate `rdfs:type` or `a` which can be used interchangably. See the below example of listing all Key Events.

```sparql
SELECT ?KE 
WHERE {
?KE a aopo:KeyEvent .
}
```

By looking at the RDF schema figure, you should be able to adapt the SPARQL query to answer all questions that follow.

Question 1.1: What would we need to replace `aopo:KeyEvent` with to extract all Adverse Outcome Pathways? 

<button onclick="toggleAnswer('q1.1')">Answer</button><span id="q1.1" style="visibility: hidden">aopo:AdverseOutcomePathway</span>

Question 1.2: What would we need to replace `aopo:KeyEvent` with to extract all Chemicals? 

<button onclick="toggleAnswer('q1.2')">Answer</button><span id="q1.2" style="visibility: hidden">cheminf:0000000</span>

Since the Key Event links can bring you to the AOP-Wiki for further exploration of the corresponding webpage, we can also directly request all their contents through the SPARQL query. For example, to extract the Key Event title, we add `?KE dc:title ?KEtitle` to the SPARQL query. The returned table upon running the query will get wider, so you might need to scroll to the right. 

```sparql
SELECT ?KE ?KEtitle
WHERE {
?KE a aopo:KeyEvent .
?KE dc:title ?KEtitle .
}
```

Question 1.3: What would we need to add to the query to extract the description of the Key Events? 

<button onclick="toggleAnswer('q1.3')">Answer</button><span id="q1.3" style="visibility: hidden">Adding another variable to the `SELECT` list and requesting that variable by adding in the query `?KE dc:description ?[new variable name]`. This should return a table with the added column.</span>

Question 1.4: What would we need to add to the query to extract the measurement/method description of the Key Events? 

<button onclick="toggleAnswer('q1.4')">Answer</button><span id="q1.4" style="visibility: hidden">Adding another variable to the `SELECT` list and requesting that variable by adding in the query `?KE mmo:0000000 ?[new variable name]`. This should return a table with the added column.</span>

### Exercise 2 - Counting of subjects
This exercise is about creating simple SPARQL queries that count particular types of subjects in the RDF. See the example SPARQL query below that counts the number of Key Events in the RDF.

```sparql
SELECT (count (?KE) as ?nKE) 
WHERE {
?KE a aopo:KeyEvent .
}
```

When copying this SPARQL query and executing it, you will find that the AOP-Wiki RDF contains 1149 KE subjects.

Question 2.1: How many Adverse Outcome Pathways are present in the AOP-Wiki RDF? 

<button onclick="toggleAnswer('q2.1')">Answer</button><span id="q2.1" style="visibility: hidden">333</span>

Question 2.2: How many chemicals are present in the AOP-Wiki RDF? 

<button onclick="toggleAnswer('q2.2')">Answer</button><span id="q2.2" style="visibility: hidden">329</span>

Slightly more complex, we count subjects that have a particular property defined.

Question 2.3: How many Key Events have a description? 

<button onclick="toggleAnswer('hint2.3')">Hint</button><span id="hint2.3" style="visibility: hidden">Define subject as type "Key Event" and also retrieve its description. This is a forced request (not optional) so the returned table will only contain Key Events with a description</span> 

<button onclick="toggleAnswer('q2.3')">Answer</button><span id="q2.3" style="visibility: hidden">389 Key Events exist that have a description.</span>

### Exercise 3 - More detailed exploration
With this exercise, the RDF will be explored a little more extensively. By combining statements in the RDF query, we can link multiple subjects and filter for content that we want to get back from the service. 

Question 3.1: What are the name and CAS ID of the chemical that is related to stressor with ID 50?

<button onclick="toggleAnswer('hint3.1')">Hint</button><span id="hint3.1" style="visibility: hidden">Define subject as type "Chemical" as in Q2.2, retrieve the title with the `dc:title` property as the example before Q1.3, and request the CAS ID with `cheminf:0000446`. Add a third statement with the link between a stressor ([prefix of stressor]:[ID]) and chemical as described in the figure. </span>

<button onclick="toggleAnswer('q3.1')">Answer</button><span id="q3.1" style="visibility: hidden">Rotenone, with CAS 83-79-4</span>

Question 3.2: What is the title of the Adverse Outcome Pathway that is activated by stressor with ID 50?

<button onclick="toggleAnswer('q3.2')">Answer</button><span id="q3.2" style="visibility: hidden">AOP 3 that is called "Inhibition of the mitochondrial complex I of nigro-striatal neurons leads to parkinsonian motor deficits"</span>

Question 3.3: What are the identifiers and titles of the Molecular Initiating Events that leads to the Adverse Outcome with ID 344 (Liver fibrosis)?

<button onclick="toggleAnswer('q3.3')">Answer</button><span id="q3.3" style="visibility: hidden">244 (Alkylation, Protein), 1539 (Endocytotic lysosomal uptake) and 1740 (ACE2 inhibition)</span>

### Exercise 4 - Federated SPARQL query
This final exercise adds an extra level of difficulty by linking the AOP-Wiki RDF with another database through SPARQL (this is called a Federated SPARQL query), and is not expected to be answered within the workshop time constraints unless very familiar with SPARQL. In this exercise we will explore the connection between AOP-Wiki and WikiPathways. The SPARQL query will need to contain a `SERVICE` function and the final query will have the following structure:

```sparql
PREFIX wp: <http://vocabularies.wikipathways.org/wp#>
SELECT [variables]
WHERE {
[query AOP-Wiki]
SERVICE <https://sparql.wikipathways.org/sparql> {
[query WikiPathways]
}}
```

The SPARQL query will require the use of the mapped chemical IDs in AOP-Wiki using the `skos:exactMatch` predicate. To do this, try mapping the resources through the ChEBI IDs. In AOP-Wiki, these are defined as type `cheminf:000407` and in WikiPathways these are matched to datanodes with `wp:bdbChEBI` (hence the `PREFIX wp` is defined in the SPARQL query). 

Question 4.1: What are the titles of the two human pathways in WikiPathways that have the chemical described in the stressor of Adverse Outcome Pathway with ID 274 in AOP-Wiki?

<button onclick="toggleAnswer('q4.1')">Answer</button><span id="q4.1" style="visibility: hidden">For Valproic acid: Valproic acid pathway (WP3871) and for Butyrate: Butyrate-induced histone acetylation (WP2366) and SCFA and skeletal muscle substrate metabolism (WP4030). This can be done with SPARQL query:
PREFIX wp: <http://vocabularies.wikipathways.org/wp#>
SELECT  ?ChemicalName ?ChEBI ?PathwayTitle ?PathwayID
WHERE{
   ?chemical a cheminf:000000 ; dc:title ?ChemicalName ; dcterms:isPartOf ?Stressor ; skos:exactMatch ?mappedid .
   ?Stressor dcterms:isPartOf ?AOP .
   ?AOP a aopo:AdverseOutcomePathway ; dc:identifier aop:274.
   ?mappedid a cheminf:000407 ; cheminf:000407 ?ChEBI.
   SERVICE <http://sparql.wikipathways.org/sparql>{ 
     ?metabolite wp:bdbChEBI ?mappedid ; dcterms:isPartOf ?Pathway.
     ?Pathway a wp:Pathway ; dcterms:identifier ?PathwayID ; dc:title ?PathwayTitle ; wp:organismName "Homo sapiens" .}}
ORDER BY DESC (?ChemicalName)
</span>

## Conclusion
This workshop provided a comprehensive introduction to querying the AOP-Wiki database using SPARQL. Through a series of progressively challenging exercises, participants learned how to construct and adapt SPARQL queries to retrieve various types of data from the AOP-Wiki RDF. The exercises covered fundamental concepts such as listing subjects, counting entities, and exploring relationships between different types of data, culminating in the use of federated SPARQL queries to link data across multiple databases.

By mastering these techniques, users can now efficiently extract and analyze complex datasets within the AOP-Wiki framework, leveraging the power of SPARQL to facilitate in-depth research and data integration. The skills acquired in this workshop provide a strong foundation for advanced data exploration and can be applied to a wide range of research topics involving toxicodynamics and risk assessment.

For further exploration, participants are encouraged to continue experimenting with SPARQL queries, making use of the extensive data available in AOP-Wiki and related resources. Should you have any questions or require further assistance, please feel free to reach out to Marvin Martens at marvin.martens@maastrichtuniversity.nl.
