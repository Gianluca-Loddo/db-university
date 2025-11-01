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

<!-- 
AI = Auto Increment : incrementa automaticamente l’id
PK = Primary Key : identifica in modo univoco ogni riga
INDEX migliora le ricerche, ma non è obbligatorio
Campi NULL sono facoltativi (non sempre ogni dipartimento ha un sito o telefono)
-->


---

## Table: degree_courses

<!--
Ogni dipartimento può offrire più corsi di laurea (ad esempio):
Dip. Ingegneria : Ingegneria Informatica, Ingegneria Meccanica
Dip. Medicina : Biotecnologie, Infermieristica

Quindi il rapporto è:
- Un Dipartimento (to) molti Corsi di Laurea
- Un Corso di Laurea (to) un solo Dipartimento
Pertanto: 
Relazione One-to-Many
-->


- id: INT AI PK NOT NULL UNIQUE INDEX  
- name: VARCHAR(100) NOT NULL UNIQUE  
- department_id: INT NOT NULL FK (to) departments(id)  
- duration_years: TINYINT NOT NULL DEFAULT(3)  
- cfu_total: SMALLINT NULL  
- description: TEXT NULL

<!-- 
department_id è la foreign key (to) collega ogni corso a un dipartimento
TINYINT è perfetto per numeri piccoli (1–255), come la durata in anni
SMALLINT per valori medi (crediti totali, massimo 32.767)
TEXT per descrizioni libere
-->
