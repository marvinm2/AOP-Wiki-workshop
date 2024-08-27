# Querying the AOP-Wiki using the SNORQL User Interface

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
Welcome to the AOP-Wiki SNORQL User Interface workshop. The exercises are meant as a step-by-step introduction to the interface and its functionalities to extract AOP-Wiki knowledge.

### AOP-Wiki general description
The AOP-Wiki serves as the primary repository of qualitative information for AOPs and is a central component in the AOP development effort coordinated by the Organisation for Economic Co-operation and Development (OECD). These AOPs describe mechanistic information about toxicodynamic processes and can be used to develop effective risk assessment strategies. An AOP is initiated by a stressor (e.g. a chemical) that causes a Molecular Initiating Event, which is followed by Key Eevents (measurable, essential steps) along a pathway towards an Adverse Outcome for an organism or population. KEs are connected through Key Event Relationships (KERs), which capture the evidence supporting the AOP in a structured way. 

### AOP-Wiki SNORQL User Interface (UI)
The AOP-Wiki SNORQL UI is loaded with RDF of the Adverse Outcome Pathway (AOP)-Wiki database [aopwiki.org](https://aopwiki.org/). The AOP-Wiki SNORQL UI is accessible on [aopwiki.cloud.vhp4safety.nl](https://aopwiki.cloud.vhp4safety.nl/). 

## Exercises
To start with exploring the functionalities of the SNORQL UI, please replace the default query panel with `https://github.com/marvinm2/AOPWikiSNORQL-demo` within the **SPARQL Examples** panel for the duration of this workshop. Then, click the yellow refresh button next to the input box. This will show a new, more concise query panel. 

### Exercise 1 - Basic functionalities
Click the first query `Q1-AllAOPs.rq` and see it appear in the query panel. As you can notice, it contains the general SPARQL syntax components, but also comments starting with a `#` to explain the query. Run the query by clicking the green `Query` button.

Question 1.1a: How many Adverse Outcome Pathways are currently listed in the AOP-Wiki RDF? 

<button onclick="toggleAnswer('q1.1a')">Answer</button><span id="q1.1a" style="visibility: hidden">
487 AOPs</span>

Question 1.1b: What is the AOP ID of the AOP with title "Inhibition of the mitochondrial complex I of nigro-striatal neurons leads to parkinsonian motor deficits"? (use Ctrl+f) 

<button onclick="toggleAnswer('q1.1b')">Answer</button><span id="q1.1b" style="visibility: hidden">
3</span>

Open the second query `Q1-AllKEs.rq` from the query panel. It introduces a function to filter the results. 
Question 1.2: What is the percentage of KEs with "mitochondrial" in their title?

<button onclick="toggleAnswer('q1.2')">Answer</button><span id="q1.2" style="visibility: hidden">
Without filter there are 1455 KEs, with filter there are 24 KEs. This is 1.6%.</span>

Question 1.3: Which types of "cancer" are present as KEs in the AOP-Wiki RDF? 

<button onclick="toggleAnswer('q1.3')">Answer</button><span id="q1.3" style="visibility: hidden">
Replacing the filter "mitochondrial" with filter "cancer" results in a list of 7 unique types: breast (x2), liver, gastric, ovarian, lung (x2), testicular, prostate, and a generic "cancer".</span>

### Exercise 2 - More extensive queries

In this exercise, you will explore more advanced querying techniques within the AOP-Wiki RDF, focusing on chemical identifiers and linking data across different databases. Open the first query starting with `Q2`.

Question 2.1: What is the PubChem ID of styrene?
<button onclick="toggleAnswer('q2.1')">Answer</button><span id="q2.1" style="visibility: hidden">When you inspect the table, you will find the PubChem ID in the right column as https://identifiers.org/pubchem.compound/7501, indicating the PubChem ID is 7501.</span>

As discussed in the introduction, RDF enables linking across different databases, which can be utilized for various biological entities, including chemicals. Next, click the PubChem IRI of styrene.

Question 2.2: What is the molecular formula of styrene?

<button onclick="toggleAnswer('q2.2')">Answer</button><span id="q2.2" style="visibility: hidden">C<sub>8</sub>H<sub>8</sub></span>

Uncomment the sections of the SPARQL query related to ChEBI. Running this query will show ChEBI IDs, often in two variants.

Question 2.3: How many unique ChEBI IDs can be found in the AOP-Wiki RDF?

<button onclick="toggleAnswer('q2.3')">Answer</button><span id="q2.3" style="visibility: hidden">Approximately 405, as the total number of results is 811.</span>

Open the second query starting with `Q2` from the query panel. It contains a SPARQL query that shows all KEs related to AOP 38. Change the SPARQL query to look for the AOP you identified in Q1.1b.

Question 2.4: How many KEs of that AOP have methods described?

<button onclick="toggleAnswer('q2.4')">Answer</button><span id="q2.4" style="visibility: hidden">7</span>

### Exercise 3 - Exploring details of Key Events

In this exercise, you will delve into the direction of AOP networks. Start by opening the first query beginning with `Q3`. As you become more familiar with SPARQL syntax, you'll notice recognizable patterns in the queries, such as the declaration of entities of type aopo:KeyEvent or aopo:AdverseOutcomePathway.

Question 3.1: Which IRI is under the prefix aopo?

<button onclick="toggleAnswer('q3.1')">Answer</button><span id="q3.1" style="visibility: hidden">By clicking the show prefixes button, you can find that the prefix aopo stands for http://aopkb.org/aop_ontology#.</span>

Question 3.2: Which MIEs can lead to the AO of "Impairment, Learning and memory" through the KE of "Mitochondrial dysfunction"?

<button onclick="toggleAnswer('q3.2')">Answer</button><span id="q3.2" style="visibility: hidden">"Binding of agonist, Ionotropic glutamate receptors" and "Activation of mitogen-activated protein kinase kinase, extracellular signal-regulated kinase 1/2".</span>

The second query starting with `Q3` explores Key Event Relationships (KER), which are the edges of the AOP network. Pay attention to the distinct keyword in the second line of the SPARQL query panel.

Question 3.3: What does the "distinct" functionality do? What is the difference in the output?

<button onclick="toggleAnswer('q3.3')">Answer</button><span id="q3.3" style="visibility: hidden">It removes duplicates from the output. In this case, it reduces the number of lines from 307 to 267, likely because some KERs are part of multiple AOPs.</span>

### Exercise 4 - Advanced SPARQL functions
In this exercise, you will explore the `OPTIONAL`, `FILTER`, and `ORDER BY` functionalities in SPARQL. Start by opening the query beginning with Q4.

Question 4.1: How many results are shown if you make the optional section obligatory?

<button onclick="toggleAnswer('q4.1')">Answer</button><span id="q4.1" style="visibility: hidden">11</span>

Next, change the filter to search for AOs with "cancer" in their title. This should yield 32 results.

Question 4.2: What does the "i" in the filter do?

<button onclick="toggleAnswer('q4.2')">Answer</button><span id="q4.2" style="visibility: hidden">If you remove the "i" (along with the comma in front), you will notice that it excludes results where the AOname has a capital "C" in "cancer." This indicates that the "i" makes the regex case-insensitive.</span>

## Conclusion

This tutorial provided a step-by-step introduction to querying the AOP-Wiki using the SNORQL User Interface (UI). Through a series of exercises, users learned how to interact with the AOP-Wiki RDF data, starting with basic functionalities and progressing to more advanced SPARQL queries. By exploring different SPARQL functions, users gained hands-on experience in extracting specific data from the AOP-Wiki database.

As users continue to explore the AOP-Wiki and its rich dataset, the techniques covered in this workshop will serve as a solid foundation for more complex queries and advanced data exploration. The ability to interact programmatically with the AOP-Wiki RDF opens up numerous possibilities for automated data retrieval and integration into broader research workflows.

For further exploration, participants are encouraged to continue experimenting with SPARQL queries, making use of the extensive data available in AOP-Wiki and related resources. Should you have any questions or require further assistance, please feel free to reach out to Marvin Martens at marvin.martens@maastrichtuniversity.nl.