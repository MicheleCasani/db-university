Ciao a tutti
Esercizio di oggi: Db University
nome repo: db-university
Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:
sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
ogni Corso può essere tenuto da diversi Insegnanti;
ogni Corso prevede più appelli d'Esame;
ogni Studente è iscritto ad un solo Corso di Laurea;
ogni Studente può iscriversi a più appelli di Esame;
per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente. Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.
Utilizzare https://www.drawio.com/ per la creazione dello schema. Esportare quindi il diagramma in jpg e caricarlo nella repo.
Buon lavoro!

------------------------------------------------------------

26/05/2025

Esercizio di oggi: nome repo: db-university (stessa degli altri giorni)
Dopo aver creato un nuovo database nel vostro MySQL Workbench e aver importato lo schema allegato, eseguite le query del file allegato.
Cosa consegnare?
Dopo aver testato le vostre query con MySQL Workbench, riportatele in un file txt e caricatelo nella vostra repo.


1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT `name`, `date_of_birth`
FROM `students`
WHERE YEAR(`date_of_birth`) = 1990;
-
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT `name`, `cfu`
FROM `courses`
WHERE (`cfu`) > 10;
-
3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT `name`, `date_of_birth`
FROM `students`
WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) > 30;
-
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea (286)

SELECT `name`, `period`,`year`
FROM `courses`
WHERE `period` = 'I semestre'
AND  `year` = '1'
-
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020 (21)

SELECT `date`, `hour`,`course_id`
FROM `exams`
WHERE `date` = '2020-06-20'
AND  `hour` >= '14:00:00'
-
6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT `department_id`, `name`,`level`
FROM `degrees`
WHERE `level` = 'magistrale'
-
7. Da quanti dipartimenti è composta l'università? (12)

SELECT `id`, `name`
FROM `departments`
-
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT `name`, `phone`
FROM `teachers`
WHERE `phone` IS NULL;
-

------------------------------------------------------------

27/05/2025

-JOIN

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT `students`.`name`, `students`.`surname`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
-

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
Neuroscienze

SELECT `degrees`.`name`
FROM `degrees`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name` = 'Dipartimento di Neuroscienze';
-
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT `courses`.`name`
FROM `courses`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `teachers`.`name` = 'Fulvio' AND `teachers`.`surname` = 'Amato'

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui

SELECT `students`.`surname`, `students`.`name`, `degrees`.`name` AS `degree_name`, `departments`.`name` AS `department_name`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname`, `students`.`name`;

sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti



6. Selezionare tutti i docenti che insegnano nel Dipartimento di
Matematica (54)



7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
per ogni esame, stampando anche il voto massimo. Successivamente,

------------------------------------------------------------

-GROUP

1. Contare quanti iscritti ci sono stati ogni anno

SELECT COUNT(*) AS `iscrizioni_per_anno`, YEAR(`enrolment_date`) AS `anno`
FROM `students`
GROUP BY anno;
-
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
-
SELECT COUNT(*) AS `num_docenti`, `office_address`
FROM `teachers`
GROUP BY `office_address`
-
3. Calcolare la media dei voti di ogni appello d'esame
-
SELECT ROUND(AVG(`vote`)) AS`media_voti`, `exam_id`
FROM `exam_student`
GROUP BY `exam_id`
-
4. Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT COUNT(*)AS`numero_corsi`, `department_id`
FROM `degrees`
GROUP BY `department_id`
-