### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 17/08/26
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:

```
import numpy as np
import pandas as pd


class BooleanRetrieval:

    def __init__(self):
        self.index = {}
        self.documents_matrix = None

    def index_document(self, doc_id, text):
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()

            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())

        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros(
            (num_docs, num_terms), dtype=int
        )

        for i, (doc_id, text) in enumerate(documents.items()):
            doc_terms = text.lower().split()

            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(
            self.documents_matrix,
            columns=self.index.keys()
        )

        print("\nDocument-Term Matrix:")
        print(df)

    def print_all_terms(self):
        print("\nAll terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):

        query = query.lower().strip()

        # AND operation
        if " and " in query:
            terms = query.split(" and ")

            if terms[0] in self.index and terms[1] in self.index:
                return self.index[terms[0]].intersection(
                    self.index[terms[1]]
                )

            return set()

        # OR operation
        elif " or " in query:
            terms = query.split(" or ")

            result = set()

            for term in terms:
                if term in self.index:
                    result = result.union(self.index[term])

            return result

        # NOT operation
        elif query.startswith("not "):
            term = query[4:].strip()

            # All document IDs
            all_documents = set(range(1, len(self.documents_matrix) + 1))

            if term in self.index:
                return all_documents.difference(self.index[term])

            return all_documents

        # Single term search
        else:
            if query in self.index:
                return self.index[query]

            return set()


if __name__ == "__main__":

    indexer = BooleanRetrieval()

    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    # Index documents
    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    # Create document matrix
    indexer.create_documents_matrix(documents)

    # Print matrix
    indexer.print_documents_matrix_table()

    # Print all terms
    indexer.print_all_terms()

    # Get Boolean query
    query = input("\nEnter your boolean query: ")

    # Search
    results = indexer.boolean_search(query)

    # Display result
    if results:
        print(f"Results for '{query}': {sorted(results)}")
    else:
        print("No results found for the query.")
```

### Output:


<img width="1047" height="375" alt="image" src="https://github.com/user-attachments/assets/7469cd1d-b881-48a3-a6ed-0b4565e6bd61" />


#### AND:

<img width="992" height="52" alt="image" src="https://github.com/user-attachments/assets/0711a940-df41-4879-a290-57ed1aac63fe" />


#### OR:

<img width="1002" height="62" alt="image" src="https://github.com/user-attachments/assets/20a9aca3-aca9-461c-9959-2f37c74e90af" />

#### NOT:

<img width="1036" height="60" alt="image" src="https://github.com/user-attachments/assets/e816b834-5617-4bb5-abde-f03560899bea" />

### Result:

Thus, Implementation of Information Retrieval Using Boolean Model in Python is successfully completed.

