# Db University

Esercizio Boolean - modellizzare un'università


## Table: departments

<!--
Ogni università è composta da più dipartimenti, ad esempio:
- Dipartimento di Ingegneria
- Dipartimento di Lettere
- Dipartimento di Medicina
Questa tabella rappresenta le macro-aree organizzative (dipartimenti) dell’università
 -->

- id: INT AI PK NOT NULL UNIQUE INDEX  
- name: VARCHAR(100) NOT NULL UNIQUE  
- address: VARCHAR(150) NULL  
- phone: VARCHAR(15) NULL  
- email: VARCHAR(100) NULL  
- website: VARCHAR(150) NULL
