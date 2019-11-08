# Exposing an RDF Datasource

This example demonstrates how RDF data can be exposed as a data service.

## Prerequisites

[Download](https://github.com/wso2-docs/WSO2_EI/blob/master/data-service-resources/Movies.rdf) the `Movies.rdf` file.
This file contains data about some popular movies. Each movie data has the
following sub elements: "title", "director", "year", "genre" and
"actor". 

## Synapse configuration
Given below is the data service configuration you need to build. See the instructions on how to [build and run](#build-and-run) this example.

**Be sure** to update the path to the rdf datasource.

```xml
<data name="RDF" transports="http https local">
 <config enableOData="false" id="GetMoviebyGenre">
    <property name="rdf_datasource">/path/to/rdf/Movies.rdf</property>
 </config>
 <query id="GetMoviebyGenre" useConfig="GetMoviebyGenre">
    <sparql>
    <![CDATA[PREFIX cd: 
      <http://www.popular.movies/cd#>  SELECT ?title ?director ?year ?genre ?actorWHERE {        ?movie cd:title ?title.      ?movie cd:director ?director.      ?movie cd:year ?year.      ?movie cd:genre ?genre.      ?movie cd:actor ?actor.}]]></sparql>
      <result element="Movies" rowName="Movie">
         <element column="title" name="title" xsdType="string"/>
         <element column="director" name="director" xsdType="string"/>
         <element column="year" name="year" xsdType="string"/>
         <element column="genre" name="genre" xsdType="string"/>
         <element column="actor" name="actor" xsdType="string"/>
      </result>
      <param name="genre" sqlType="STRING"/>
   </query>
   <operation name="GetMoviesbyGenreOp">
      <call-query href="GetMoviebyGenre">
         <with-param name="genre" query-param="genre"/>
      </call-query>
   </operation>
   <resource method="GET" path="Movies/{genre}">
      <call-query href="GetMoviebyGenre">
         <with-param name="genre" query-param="genre"/>
      </call-query>
   </resource>
</data>
```

## Build and run

Create the artifacts:

1. [Set up WSO2 Integration Studio](../../../../develop/installing-WSO2-Integration-Studio).
2. [Create a Data Service project](../../../../develop/creating-projects/#data-services-project).
4. [Create the data service](../../../../develop/creating-artifacts/data-services/creating-data-services) with the configurations given above.

    **Be sure** to update the path to the rdf datasource.

5. [Deploy the artifacts](../../../../develop/deploy-and-run) in your Micro Integrator. 

You can send an HTTP GET request to invoke the data service using cURL
as shown below.

```bash
curl -X GET http://localhost:8290/services/RDF.HTTPEndpoint/Movies/Drama
```

This will return the response in XML:

```xml
<Movies xmlns="http://ws.wso2.org/dataservice">
<Movie>
  <title>Ratatouille</title>
  <director>Jan Pinkava</director>
  <year>2007</year>
  <genre>Drama</genre>
  <actor>Teddy Newton</actor>
</Movie>
<Movie>
  <title>Catch Me If You Can</title>
  <director>Steven Spielberg</director>
  <year>2002</year>
  <genre>Drama</genre>
  <actor>Antoine Drolet Dumoulin</actor>
</Movie>
<Movie>
  <title>The Spectacular Now</title>
  <director>James Ponsoldt</director>
  <year>2013</year>
  <genre>Drama</genre>
  <actor>Andre Royo</actor>
</Movie>
<Movie>
  <title>Adventureland</title>
  <director>Greg Mottola</director>
  <year>2009</year>
  <genre>Drama</genre>
  <actor>Ryan Mcfarland</actor>
</Movie>
<Movie>
  <title>Begin Again</title>
  <director>John Carney</director>
  <year>2014</year>
  <genre>Drama</genre>
  <actor>Eric Burton</actor>
</Movie>
<Movie>
  <title>Flipped</title>
  <director>Rob Reiner</director>
  <year>2010</year>
  <genre>Drama</genre>
  <actor>Michele Messmer</actor>
</Movie>
<Movie>
  <title>Beginners</title>
  <director>Mike Mills</director>
  <year>2011</year>
  <genre>Drama</genre>
  <actor>Seth T Walker</actor>
</Movie>
<Movie>
  <title>Bridget Jones's Diary</title>
  <director>Cilla Ware</director>
  <year>2001</year>
  <genre>Drama</genre>
  <actor>John Clegg</actor>
</Movie>
</Movies>
```