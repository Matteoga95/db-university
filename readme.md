Modellizzare la struttura di una tabella per memorizzare tutti i dati riguardanti una università:
- sono presenti diversi **Dipartimenti** (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
- ogni Dipartimento offre più **Corsi di Laurea** (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
- ogni Corso di Laurea prevede diversi **Corsi** (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
- ogni Corso può essere tenuto da diversi **Insegnanti**;
- ogni Corso prevede più appelli d'**Esame**;
- ogni **Studente** è iscritto ad un solo Corso di Laurea;
- ogni Studente può iscriversi a più appelli di Esame;
- per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente.
Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.

Tabelle:
- Departments
- Degree_courses
- Course
- Teachers
- Exams
- Studenti
- Voti

oneToMany => Dipartimenti - Corsi_di-laurea
manyToMany => Corsi_di_laurea - Corsi
oneToMany => Inseganti - Corsi
oneToMany => Corsi - Esami
manyToMany => Corsi - Studenti
oneToMany => Studenti - Corsi_di_laurea
manyToMany => Studenti - Esami

Departments:
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-name | VARCHAR(50) - NOTNULL 
-code | CHAR(5) - NOTNULL -UNIQUE

Degree_courses:
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-id_department | FK - BIGINT
-name | VARCHAR(100) - NOTNULL 
-code | CHAR(5)

Course_DegreeCourse: #####
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-id_Course | FK - BIGINT
-id_DegreeCourse | FK - BIGINT

Courses:
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-id_teacher | FK - BIGINT
-name| VARCHAR(50) - NOTNULL 
-code | CHAR(5)

Student_Course: #######
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-id_student | FK - BIGINT
-id_course | FK - BIGINT

Students:
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-id_degree_course | FK - BIGINT
-freshman | CHAR(5) - NULL
-fullname | VARCHAR(50) - NOTNULL 
-date_of_birth | DATE - NULL
-email | VARCHAR(30) - NULL
-phone | INT - NULL

Student_exam: ######
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-id_student | FK - BIGINT
-id_exam | FK - BIGINT
-number_of_exam | SMALLINT - NULL

Exams:
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-id_course | FK - BIGINT
-name | VARCHAR(100) - NOTNULL 
-date | DATETIME - NULL
-vote | TINYINT - NULL


Teachers:
-id | BIGINT - PK - AI - NOTNULL - UNIQUE
-fullname| VARCHAR(100) - NOTNULL 
-email | VARCHAR(30) - NULL
-phone | INT - NULL
-code | CHAR(5) - NULL
-role | VARCHAR(30) - NULL