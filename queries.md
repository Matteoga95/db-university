Query Todo:

Selezionare tutti gli studenti nati nel 1990 (160)

```sql
SELECT * 
FROM students 
WHERE YEAR(date_of_birth)=1990;
```


Selezionare tutti i corsi che valgono più di 10 crediti (479)

```sql
SELECT * 
FROM courses
WHERE cfu > 10;
```


Selezionare tutti gli studenti che hanno più di 30 anni

```sql
SELECT * 
FROM students 
WHERE YEAR(date_of_birth)<1992;
```


Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

```sql
SELECT * 
FROM courses 
WHERE `year`=1 AND period = "I semestre";
```


Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

```sql
SELECT * 
FROM exams 
WHERE HOUR(`hour`) >= "14" AND `date` = "2020/06/20";
```


Selezionare tutti i corsi di laurea magistrale (38)

```sql
SELECT * 
FROM degrees 
WHERE `level`="magistrale";
```


Da quanti dipartimenti è composta l'università? (12)

```sql
SELECT COUNT(id)
FROM departments ;
```


Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

```sql
SELECT count(*)
FROM teachers
WHERE `phone` is null ;
```

||||||||||||||||||||||||||||||||||||||||||||||||

queries del 06/12

Group by:

Contare quanti iscritti ci sono stati ogni anno

```sql
SELECT courses.`year`, count(*)
FROM students
JOIN degrees  ON degrees.id = students.degree_id
JOIN courses ON courses.degree_id = degrees.id
GROUP BY courses.`year`;
```


Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT teachers.office_address as "Ufficio", COUNT(teachers.office_address) AS "N. di insegnanti"
FROM teachers
GROUP BY teachers.office_address
HAVING COUNT(teachers.office_address) > 1;
```

Calcolare la media dei voti di ogni appello d'esame

```sql
SELECT courses.`name` , exam_student.exam_id as "Appello n.", exams.date as "data appello", AVG(exam_student.vote) as "Media"
FROM exam_student
JOIN exams ON exams.id = exam_student.exam_id
join courses ON courses.id = exams.course_id
GROUP BY exam_student.exam_id;
```

Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT departments.`name` as "Dipartimento", COUNT(degrees.id) as "Numero di corsi di laurea"
FROM degrees
JOIN departments ON degrees.department_id = departments.id
GROUP BY department_id;
```


Join:

Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT  students.`name`, students.surname
FROM students
JOIN degrees ON degrees.id = students.degree_id
WHERE degrees.`name` like "%economia%";
```

Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```sql
SELECT degrees.`name`
FROM degrees
JOIN departments ON degrees.department_id = departments.id
WHERE degrees.`name` like "%magistrale%"  AND departments.`name` like "%Neuroscienze%" ;
```


Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT degrees.`name` as "Corso di laurea", courses.`name` as "Nome Corso"
FROM courses
JOIN degrees ON courses.degree_id = degrees.id
JOIN course_teacher ON course_teacher.course_id = courses.id
JOIN teachers ON course_teacher.teacher_id = teachers.id
WHERE teachers.id=44 ;
```


Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```sql
SELECT students.surname as "Cognome", students.`name` as "Nome", departments.`name` as "Dipartimento", degrees.`name` as "Corso di laurea", degrees.`level` as "Tipo di laurea"
FROM students
JOIN degrees ON students.degree_id = degrees.id
JOIN departments ON degrees.department_id = departments.id 
ORDER BY students.surname, students.`name`;
```


Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT degrees.`name` as "Corso di laurea", degrees.`level` as "Tipo di laurea", courses.`name` as "nome corso", teachers.`name` ,teachers.surname
FROM degrees
JOIN courses ON courses.degree_id = degrees.id
join course_teacher ON course_teacher.course_id = courses.id
JOIN teachers ON course_teacher.teacher_id = teachers.id;
```

Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```sql
SELECT DISTINCT teachers.`name` as "Nome Insegnante" ,teachers.surname as "Cognome Insegnante", departments.`name` as "Dipartimento"
FROM teachers
JOIN course_teacher ON course_teacher.teacher_id = teachers.id
JOIN courses ON courses.id= course_teacher.course_id
JOIN degrees ON courses.degree_id = degrees.id
JOIN departments on degrees.department_id = departments.id
WHERE departments.`name` LIKE "%matematica%";
```



BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami

```sql
SELECT students.surname, students.`name`, courses.`name` as "Nome cosros d'esame", COUNT(exams.id) as "Tentativi appello"
FROM exams
join exam_student ON exam_student.exam_id = exams.id
JOIN students ON exam_student.student_id = students.id
JOIN courses on exams.course_id = courses.id

GROUP BY  exams.course_id, students.id;
```




