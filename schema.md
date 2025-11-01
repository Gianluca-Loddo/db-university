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
- AI = Auto Increment : incrementa automaticamente l’id
- PK = Primary Key : identifica in modo univoco ogni riga
- INDEX migliora le ricerche, ma non è obbligatorio
- Campi NULL sono facoltativi (non sempre ogni dipartimento ha un sito o telefono)
-->


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
- department_id è la foreign key (to) collega ogni corso a un dipartimento
- TINYINT è perfetto per numeri piccoli (1–255), come la durata in anni
- SMALLINT per valori medi (crediti totali, massimo 32.767)
- TEXT per descrizioni libere
-->


## Table: courses

<!-- 
Ogni corso di laurea è composto da diversi insegnamenti (corsi) - (ad esempio):
- Informatica: Basi di Dati, Programmazione, Sistemi Operativi
- Biologia: Anatomia, Chimica, Genetica

Quindi il rapporto è:
- Un corso di laurea (to) molti insegnamenti
- Un insegnamento (to) appartiene a un solo corso di laurea
Pertanto:
Relazione One-to-Many
-->

- id: INT AI PK NOT NULL UNIQUE INDEX  
- name: VARCHAR(100) NOT NULL  
- code: CHAR(6) NOT NULL UNIQUE  
- cfu: TINYINT NOT NULL  
- year: TINYINT NOT NULL  
- semester: TINYINT NOT NULL  
- degree_course_id: INT NOT NULL FK (to) degree_courses(id)  
- description: TEXT NULL

<!--
- code serve come abbreviazione univoca (es. “MAT101”).
- cfu = crediti formativi universitari (max 12, TINYINT è sufficiente).
- year e semester aiutano a ordinare i corsi all’interno del piano di studi.
- degree_course_id collega il corso al suo corso di laurea (to) FK.
-->


## Table: teachers

<!--
Ogni docente appartiene a un dipartimento (es. Ingegneria, Medicina) e può insegnare più corsi.
Quindi il rapporto è:
- Un Dipartimento (to) molti Docenti
- Un Docente (to) un solo Dipartimento
Pertanto:
Relazione One-to-Many
-->

- id: INT AI PK NOT NULL UNIQUE INDEX  
- department_id: INT NOT NULL FK (to) departments(id)  
- first_name: VARCHAR(50) NOT NULL  
- last_name: VARCHAR(50) NOT NULL  
- email: VARCHAR(100) NOT NULL UNIQUE  
- phone: VARCHAR(15) NULL  
- office: VARCHAR(50) NULL  
- academic_title: VARCHAR(50) NULL

<!--
department_id lega ogni docente al proprio dipartimento
-->

## Table: students

<!-- 
Ogni studente:
- è iscritto a un corso di laurea,
- ha dati anagrafici,
- una matricola univoca.

La relazione è:
- Un Corso di Laurea (to) molti Studenti
- Uno Studente (to) un solo Corso di Laurea
Pertanto:
Relazione One-to-Many
-->

- id: INT AI PK NOT NULL UNIQUE INDEX  
- degree_course_id: INT NOT NULL FK (to) degree_courses(id)  
- first_name: VARCHAR(50) NOT NULL  
- last_name: VARCHAR(50) NOT NULL  
- email: VARCHAR(100) NOT NULL UNIQUE  
- registration_number: CHAR(6) NOT NULL UNIQUE  
- date_of_birth: DATE NULL  
- phone: VARCHAR(15) NULL  
- address: VARCHAR(100) NULL

<!-- 
- degree_course_id lega ogni studente al suo corso di laurea
- registration_number è la matricola, un codice identificativo univoco
- email deve essere unica (come per i docenti)
-->

## Table: exams

<!-- 
Ogni esame rappresenta una sessione per un determinato corso (insegnamento), tenuta da un docente e valida per uno o più studenti,
MA:
- un corso può avere molte sessioni d’esame (una per ogni appello),
- un docente può tenere molti esami,
- ogni esame può essere sostenuto da molti studenti (gestiremo questo dopo, con una tabella ponte exam_student).

RELAZIONI PRINCIPALI: 
course_id : collega l’esame al corso (One-to-Many)
teacher_id : collega l’esame al docente (One-to-Many)
-->

- id: INT AI PK NOT NULL UNIQUE INDEX  
- course_id: INT NOT NULL FK (to) courses(id)  
- teacher_id: INT NOT NULL FK (to) teachers(id)  
- date: DATE NOT NULL  
- classroom: VARCHAR(50) NULL  
- notes: TEXT NULL

<!-- 
- course_id e teacher_id sono foreign keys, quindi l’esame appartiene a un corso ed è tenuto da un docente
- DATE è un tipo di dato temporale che memorizza solo la data (senza orario)
- TEXT per eventuali note (es. “solo studenti del 3° anno”)
-->