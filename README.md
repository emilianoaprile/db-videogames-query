### Selezioni

1. **Selezionare tutte le software house americane (3)**
    ```sql
    SELECT * 
    FROM `software_houses` 
    WHERE country = 'United States';
    ```

2. **Selezionare tutti i giocatori della città di 'Rogahnland' (2)**
    ```sql
    SELECT * 
    FROM `players`
    WHERE city = 'Rogahnland';
    ```

3. **Selezionare tutti i giocatori il cui nome finisce per "a" (220)**
    ```sql
    SELECT * 
    FROM `players`
    WHERE name LIKE '%a';
    ```

4. **Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)**
    ```sql
    SELECT *
    FROM `reviews`
    WHERE player_id = '800';
    ```

5. **Contare quanti tornei ci sono stati nell'anno 2015 (9)**
    ```sql
    SELECT *
    FROM `tournaments`
    WHERE `year` = '2015';
    ```

6. **Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)**
    ```sql
    SELECT * 
    FROM `awards`
    WHERE `description` LIKE '%facere%';
    ```

7. **Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)**
    ```sql
    SELECT DISTINCT v.id 
    FROM `videogames` AS v
    INNER JOIN `category_videogame` AS cv ON v.id = cv.videogame_id
    INNER JOIN `categories` AS c ON cv.category_id = c.id
    WHERE c.name = 'FPS' OR c.name = 'RPG';
    ```

8. **Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)**
    ```sql
    SELECT * 
    FROM `reviews` AS r
    WHERE r.rating >= '2' AND r.rating <= '4';
    ```

9. **Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)**
    ```sql
    SELECT * 
    FROM `videogames` AS v
    WHERE YEAR(`release_date`) = '2020';
    ```

10. **Selezionare gli id dei videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)**
    ```sql
    SELECT DISTINCT v.id
    FROM `videogames` AS v
    INNER JOIN `reviews` as r ON v.id = r.videogame_id
    WHERE r.rating >= '5';
    ```


### BONUS

11. **Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)**
    ```sql
    SELECT COUNT(r.id) AS review_number, AVG(r.rating) AS avg_rating
    FROM `reviews` AS r
    WHERE r.videogame_id = '412';
    ```

12. **Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)**
    ```sql
    SELECT COUNT(v.id) AS total_videogames
    FROM `videogames` AS v
    WHERE v.software_house_id = '1' 
    AND YEAR(v.release_date) = '2018';
    ```

### GROUP BY

1. **Contare quante software house ci sono per ogni paese (3)**
    ```sql
    SELECT country, COUNT(*) AS total_software_houses
    FROM `software_houses`
    GROUP BY country;
    ```


2. **Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)**
    ```sql
    SELECT videogame_id, COUNT(*) AS total_videogames 
    FROM `reviews`
    GROUP BY videogame_id;
    ```

3- Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)

SELECT pl.id, COUNT(v.id) AS total_videogames
FROM `pegi_labels` AS pl
INNER JOIN `pegi_label_videogame` AS pl_v ON pl.id = pl_v.id
INNER JOIN `videogames` AS v ON pl_v.videogame_id = v.id
GROUP BY pl.id;


4- Mostrare il numero di videogiochi rilasciati ogni anno (11)

SELECT YEAR(v.release_date) AS release_year, COUNT(v.id) AS total_videogames
FROM `videogames` AS v
GROUP BY release_year;


5- Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)

SELECT d.id, COUNT(v.id) AS total_videogames
FROM `devices` AS d
INNER JOIN `device_videogame` AS dv ON d.id = dv.id
INNER JOIN `videogames` AS v ON dv.videogame_id = v.id
GROUP BY d.id;


6- Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)

SELECT v.id, AVG(r.rating) AS avg_rating
FROM `videogames` AS v
INNER JOIN `reviews` AS r ON v.id = r.videogame_id
GROUP BY v.id
ORDER BY avg_rating DESC;


JOIN

1- Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

SELECT DISTINCT p.*
FROM `players` AS p
INNER JOIN `reviews` as r ON p.id = r.player_id;


2- Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)

SELECT DISTINCT v.*
FROM `videogames` AS v
INNER JOIN `tournament_videogame` AS tv ON v.id = tv.videogame_id
INNER JOIN `tournaments` AS t ON tv.tournament_id = t.id
WHERE t.year = '2016';


3- Mostrare le categorie di ogni videogioco (1718)

SELECT c.name 
FROM `videogames` AS v
INNER JOIN `category_videogame` AS cv ON v.id = cv.videogame_id
INNER JOIN `categories` AS c ON cv.category_id = c.id;


4- Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)

SELECT * 
FROM `software_houses` as sh
INNER JOIN `videogames` as v ON sh.id = v.software_house_id
WHERE YEAR(v.release_date) > 2020;


5- Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)

SELECT sh.name, v.name, a.name, av.year 
FROM `awards` AS a
INNER JOIN award_videogame AS av ON a.id = av.award_id
INNER JOIN `videogames` AS v ON v.id = av.award_id
INNER JOIN `software_houses` AS sh ON v.software_house_id = sh.id;


6- Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)




7- Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)
8- Selezionare le città in cui è stato giocato il gioco dell'anno del 2018 (36)
9- Selezionare i giocatori che hanno giocato al gioco più atteso del 2018 in un torneo del 2019 (991)
*********** BONUS ***********
10- Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)
11- Selezionare i dati del videogame (id, name, release_date, totale recensioni) con più recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)
12- Selezionare la software house che ha vinto più premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)
13- Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 2 (10)
