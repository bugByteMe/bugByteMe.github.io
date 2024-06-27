# Inverted Index

* **Posting list**

  Frequency -> <Document, offset> -> <Document, offset> -> ...

* **Distributed Indexing**

  * **Term-partitioned index**

    Distribute according to term. 

    e.g. a-c, d-f, x-z

  * **Document-partitioned index**

    Distribute according to document.

    e.g. doc1-1000, doc1001-2000 ...

* **Thresholding**

  * **Document**

    Only retrieve top 10 documents according to their rank.

  * **Query term**

    Sort query term in ascending by their frequency. Only search for some of them.

* **Relevance Measurement**

  * Precision
  * Recall

95-100 5.0

92-94 4.8

89-91 4.5

86-90 4.2

