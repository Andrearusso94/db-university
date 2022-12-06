Seleziono tutti gli studenti
```
sql
/*Selezionare tutti gli studenti nati nel 1990 (160)*/
SELECT * FROM students WHERE date_of_birth BETWEEN "1990-01-01" AND "1990-12-31";
/*Selezionare tutti i corsi che valgono più di 10 crediti (479)*/
SELECT * FROM courses WHERE cfu > 10;
/*Selezionare tutti gli studenti che hanno più di 30 anni*/
SELECT * FROM `students` WHERE `date_of_birth` < DATE_SUB(CURRENT_DATE(), INTERVAL 30 YEAR) ORDER BY `students`.`date_of_birth` DESC
/*Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)*/
SELECT * FROM courses WHERE period = "I semestre" AND year = 1;
/*Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)*/
SELECT * FROM `exams` WHERE `date` = "2020/06/20" AND `hour` >= "14:00"
/*Selezionare tutti i corsi di laurea magistrale (38)*/
SELECT * FROM degrees WHERE name LIKE "%Laurea Magistrale%";
/*Da quanti dipartimenti è composta l'università? (12)*/
SELECT COUNT(id) from departments;
/*Quanti sono gli insegnanti che non hanno un numero di telefono? (50)*/
SELECT * FROM teachers WHERE phone IS null;





GROUP BY
/*1/Contare quanti iscritti ci sono stati ogni anno*/
SELECT YEAR(students.enrolment_date) AS anno_iscrizione, COUNT(id) AS numero_iscritti
FROM students
GROUP BY anno_iscrizione;

/*2/ Contare gli insegnanti che hanno l'ufficio nello stesso edificio*/
SELECT teachers.office_address AS ufficio_comune, COUNT(id) AS n_insegnati
FROM teachers
GROUP BY ufficio_comune;

/*3/Calcolare la media dei voti di ogni appello d'esame*/
SELECT exam_id as appello_esame, AVG(exam_student.vote) AS media_voti
FROM exam_student
GROUP BY appello_esame;


/*4/Contare quanti corsi di laurea ci sono per ogni dipartimento*/
SELECT degrees.department_id AS dipartimento, COUNT(id)as n_corsi_laurea
FROM degrees
GROUP BY dipartimento;




//JOIN


/*1/Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia */
SELECT students.id , students.name,students.surname,  degrees.name
FROM students
INNER JOIN degrees
ON degrees.id = students.degree_id
WHERE degrees.name = 'Corso di Laurea in Economia';


/*3/Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)*/
SELECT teachers.name, teachers.surname,courses.name, courses.id
FROM teachers
 JOIN course_teacher
ON course_teacher.teacher_id = teachers.id
 JOIN courses
ON courses.id = course_teacher.course_id
WHERE teachers.name = 'Fulvio' AND teachers.surname = 'Amato';


/*4/Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome*/

SELECT students.name, students.surname, degrees.name AS Corso,departments.name AS Dipartimento
FROM students 
INNER JOIN degrees
ON degrees.id = students.degree_id
INNER JOIN departments 
ON departments.id = degrees.department_id
ORDER BY students.surname, students.name  ASC;
```