# Mini-projet – Books RDF Web App

This repository contains the code and data for my semantic-web mini-project in the course *Web sémantique et web des données* .

The goal of the project is to build a small RDF-based web application on top of a real dataset:

- convert tabular data about books into RDF,
- define a minimal RDFS/OWL vocabulary,
- publish the data through a SPARQL endpoint (Fuseki),
- and provide a simple HTML+JavaScript interface to run queries and explore the data.   

The dataset is a subset of **“GoodReads Best Books”** from Kaggle. Each book is modeled as a `schema:Book`, linked to authors, series, genres, languages, etc. The RDF reuses terms from `schema.org` and introduces a few local properties and classes.   

---

##  Project Structure

```bash
books-semantic-project/

├── data/
│   └── books_1.Best_Books_Ever.csv      # Original structured dataset
│
├── rdf/
│   ├── books_data_pretty.ttl            # RDF instance data
│   └── books-vocab_new.ttl              # RDFS/OWL vocabulary
│
├── web/
│   └── index.html                       # Web interface 
│
├── Report_Zhijie LI.pdf                 # Final report
└── README.md                            # Documentation
```

---

## Prerequisites

- **Apache Jena Fuseki** (tested with a local instance on `http://localhost:3030`)
- Internet access for the Wikidata queries (`https://query.wikidata.org/sparql`)

---

## 1. Load the RDF data into Fuseki

1. **Install and start Fuseki**

   Download and run Apache Jena Fuseki.  
   By default the admin UI is available at `http://localhost:3030`.

2. **Create a dataset**

   - In the Fuseki UI, create a new dataset named for example `books`  

   - After creation, Fuseki should expose a SPARQL endpoint like:  

     `http://localhost:3030/books/sparql`

3. **Upload RDF data and vocabulary**

   In the dataset’s *Upload* or *Add data* tab:

   - Upload `books_data_pretty.ttl` (instance data).
   - Upload `books-vocab_new.ttl` (RDFS/OWL vocabulary).

## 2. Run the web application

1. **Open the HTML page**

   - Simply open `index.html` in your browser .
   - At the top of the page there is a text field labeled **SPARQL endpoint**.

2. **Set the endpoint**

   - For the local dataset, use:

     ```
     http://localhost:3030/books/sparql
     ```

   - This is also the default value inside the page.

3. **Use the predefined queries**

   The *Query* section contains several buttons:

   - **“Top 50 highest-rated books”**
      Lists the 50 books with the highest Goodreads rating.
   - **“Search books by author”**
      After entering an author name, lists books written by this author, using the author–book links in RDF.
   - **“Books with series + seriesPosition”**
      Shows books that belong to a series and their position within that series.
   - **“Number of books by genre”**
      Aggregates the number of books per `schema:genre` and displays the counts.

   Clicking a button:

   - fills the SPARQL textarea with the corresponding query,
   - sends the query to the endpoint,
   - displays the JSON results as an HTML table in the *Results* section.

4. **Run custom SPARQL**

   - You can edit the SPARQL textarea or paste your own queries.
   - Click the *Run query* button to execute them against the chosen endpoint.

5. **Wikidata author nationality**

   - In the dedicated section, you can enter a book title and click the button to find the author’s nationality.
   - The script first finds the author in the local `books` dataset, then queries Wikidata , endpoint `https://query.wikidata.org/sparql` for the country of citizenship of that author, restricted to entities that are humans and writers , `wdt:P31 wd:Q5`, `wdt:P106 wd:Q36180`.
   - Because of name collisions and formatting issues, some titles may return multiple authors or no result.

------

## 3. Notes and limitations

- The project focuses on a **minimal** but functional web application; design and styling are deliberately kept simple.
- Some advanced visualisation (D3 + d3sparql) was experimented but is not fully reliable in the final version.
