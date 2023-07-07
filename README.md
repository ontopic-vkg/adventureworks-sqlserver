# Mapping of AdventureWorks 2019 to schema.org

AdventureWorks is a famous [sample database published by Microsoft](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure).
This project maps its 2019 OLTP edition installed on SQL Server 15.00.4236.



## Using it with Ontopic Studio

 - Create a project on Ontopic Studio
 - Configure the connection to the SQL Server instance on which AdventureWorks is installed
 - On the dashboard, click on "Import project" and load the file `adventureworks-sqlserver.ontopicprj`
 - Go to "Search" and get an overview of what has been mapped
 - Go "Query > SPARQL" and test your SPARQL queries


### Example queries

#### All the properties grouped per class

```sparql
SELECT DISTINCT ?c ?p WHERE {
  ?sub a ?c ; ?p ?o .
}
GROUP BY ?c ?p
ORDER BY ?c ?p
```

####  Best sales persons per year

```sparql
PREFIX schema: <http://schema.org/>

SELECT ?year (SUM(?totalDue) AS ?totalSales) ?firstName ?lastName WHERE {
  ?order a schema:Order ; 
         schema:partOfInvoice / schema:totalPaymentDue / schema:value ?totalDue ;
         schema:broker ?salesPerson ;
         schema:orderDate ?date .
  ?salesPerson schema:familyName ?lastName ; schema:givenName ?firstName .
  BIND (year(?date) AS ?year)
} 
GROUP BY ?year ?firstName ?lastName
ORDER BY ?year DESC(?totalSales)
```

## Deploy as a Virtual Knowledge Graph using Ontop

1. Edit the file `adventure.properties` to insert the credentials and the JDBC URL for connecting to the database. Tip: you can see the URL in Ontopic Studio.
2. Start the docker-compose script: `docker-compose up -d`
3. Open http://localhost:8080 and run your SPARQL queries


