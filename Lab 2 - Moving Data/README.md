# Lab 2 - Moving Data
In this lab, we're going to take data from a Google Cloud Storage bucket and import it into Neo4j.  We'll use the load CSV command in the Neo4j Cypher query language to do this.

## Checking out what we have.
We're going to be working with two files: train.csv and test.csv.  As the names imply, one is intended to be a training data set for our ML algorithms and another a test dataset.  The files are publicly available here:

https://storage.googleapis.com/neo4j-datasets/form13/train.csv
https://storage.googleapis.com/neo4j-datasets/form13/test.csv

The dataset is pulled from the SEC's EDGAR database.  These are public filings of something called form 13.  Asset managers with over $100 AUM are required to submit Form 13 quarterly.  That's then made available to the public over http.  The csvs linked above were pull from EDGAR using some python scripts.  We don't have time to run those in the lab today as they take a few hours.  But, if you're curious, they're all available [here](https://github.com/neo4j-partners/neo4j-sec-edgar-form13).

When you get a new dataset, it's often a good idea to poke at it in pandas a bit.  So, let's fire up a notebook and fiddle with it.  You can open the notebook [here](playing-around.ipynb).

## Import data into Neo4j
Now that we've done a little traditional tabular data exploration with pandas, let's trying doing graph exploration.  We're going to need to load the data into Neo4j.  To do that, we'll use the AuraDS setup we created in our last lab.

    LOAD CSV WITH HEADERS FROM 'https://storage.googleapis.com/neo4j-datasets/form13/train.csv' AS row
    MERGE (m:Manager {filingManager:row.filingManager})
    MERGE (c:Company {nameOfIssuer:row.nameOfIssuer, cusip:row.cusip})
    MERGE (m)-[r1:Owns {value:toInteger(row.value), shares:toInteger(row.shares), reportCalendarOrQuarter:row.reportCalendarOrQuarter, target:row.target}]->(c)

Start time 9:54pm...

To delete the contents of the database, you can run:

    MATCH (n)
    DETACH DELETE n;