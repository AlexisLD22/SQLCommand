```sql
/*Commande SQL:

Pour faire des commentaires dans un dossier SQL on utilise
'--' ou '#' pour des commentaires sur seulement une ligne*/
/* blabla */ --pour des commentaires sur plusieurs lignes

--Commande de base:

----------------------------------------------------Create database----------------------------------------------------

CREATE DATABASE world;
CREATE DATABASE IF NOT EXISTS world;

----------------------------------------------------Drop----------------------------------------------------

DROP DATABASE world;
DROP DATABASE IF EXISTS world;
DROP TABLE email;

----------------------------------------------------Truncate----------------------------------------------------
/*Note: permet de supprimer toutes les données d’une table sans supprimer la table en elle-même. En d’autres mots, 
cela permet de purger la table. De plus, elle réinitialise les auto incrémentation*/

TRUNCATE TABLE utilisateur_banale

----------------------------------------------------Delete----------------------------------------------------
DELETE FROM email
WHERE id = 1

DELETE FROM email
WHERE `email` = @gmail.com

----------------------------------------------------Create Table----------------------------------------------------

CREATE TABLE utilisateur
(
    id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
    nom VARCHAR(100),
    prenom VARCHAR(100),
    email VARCHAR(255),
    date_naissance DATE,
    pays VARCHAR(255),
    ville VARCHAR(255),
    code_postal VARCHAR(5),
    nombre_achat INT
)

CREATE TABLE email  --avec 'email' en primary key
(
    `email` VARCHAR(50) NOT NULL,
    `nom` VARCHAR(75) NOT NULL,
    `date_inscription` DATE,
    PRIMARY KEY('email')
)

CREATE TABLE utilisateur_banale --auto incrementation sur la primary key
(
    `id` INT NOT NULL AUTO_INCREMENT,
    `nom` VARCHAR(50),
    `email` VARCHAR(50),
    `date_inscription` DATE,
    PRIMARY KEY (`id`)
)

----------------------------------------------------Insert into----------------------------------------------------

INSERT INTO utilisateur_banale (nom, email) VALUE ('Alexis','ledoussal.alexis@gmail.com');
INSERT INTO utilisateur (nom, prenom, email, date_naissance, pay, ville, code_postal, nombre_achat) 
VALUE 
('Le Doussal','Alexis','ledoussal.alexis@gmail.com','06/06/2004','France','Angers','49100','3');

/*plusieurs en même temps*/
INSERT INTO email (nom, email) 
VALUES
 ('Rébecca', 'Rebecca@gmail.com'),
 ('Aimée', 'Aimée@gmail.com'),
 ('Marielle', 'Marielle@google.cum'),
 ('Hilaire', 'Hilaire@allemagne.ss');

----------------------------------------------------Alter Table----------------------------------------------------

ALTER TABLE utilisateurs;

DROP COLUMN nombre_achat;
ADD addresse_rue VARCHAR(255);
MODIFY nom VARCHAR(75);
CHANGE pays pays_naissance VARCHAR(255);

ALTER TABLE golfeurs ADD âge INT;
ALTER TABLE golfeurs DROP COLUMN email;

----------------------------------------------------Upadate----------------------------------------------------

UPDATE utilisateur
SET ville = 'Pont-scroff',
    code_postal = '56620'
WHERE id = 1;

UPDATE golfeurs SET âge = 78 WHERE lastname = 'LEGEND';
UPDATE golfeurs SET âge = 30 WHERE firstname = 'Tim';
UPDATE golfeurs SET âge = 200 WHERE email = 'h-d@suzukiki.fr';

----------------------------------------------------Select & Where----------------------------------------------------

SELECT golfeurs FROM firstname;
SELECT name FROM cities WHERE id < 9;
SELECT * FROM `cities` WHERE state_code = 'FR';

--DISTINCT
SELECT DISTINCT country_id FROM cities;
SELECT DISTINCT name FROM cities;
SELECT DISTINCT name FROM cities WHERE id < 50;

--BETWEEN
SELECT name, latitude, longitude FROM cities WHERE latitude BETWEEN 25 AND 49;
SELECT name, latitude, longitude FROM cities WHERE id BETWEEN 15 AND 25;
SELECT name, region, id FROM countries WHERE region IN ('Europe', 'Americas') AND id BETWEEN 50 AND 150;


--LIKE
/*
LIKE ‘%a’ : le caractère “%” est un caractère joker qui remplace tous les autres caractères. 
    Ainsi, ce modèle permet de rechercher toutes les chaines de caractère qui se termine par un “a”.
LIKE ‘a%’ : ce modèle permet de rechercher toutes les lignes de “colonne” qui commence par un “a”.
LIKE ‘%a%’ : ce modèle est utilisé pour rechercher tous les enregistrement qui utilisent le caractère “a”.
LIKE ‘pa%on’ : ce modèle permet de rechercher les chaines qui commence par “pa” et qui se terminent par “on”, 
    comme “pantalon” ou “pardon”.
LIKE ‘a_c’ : peu utilisé, le caractère “_” (underscore) peut être remplacé par n’importe quel caractère, 
    mais un seul caractère uniquement (alors que le symbole pourcentage “%” peut être remplacé par un nombre incalculable de caractères. 
    Ainsi, ce modèle permet de retourner les lignes “aac”, “abc” ou même “azc”.*/

SELECT name, region, capital FROM countries WHERE region LIKE 'Eu%';
SELECT name, region, capital FROM countries WHERE region LIKE '%cas';
SELECT name, region, capital FROM countries WHERE region LIKE '%fri%';
SELECT name, region, capital FROM countries WHERE region LIKE 'Eu%pe';
SELECT name, region, capital FROM countries WHERE region LIKE 'As_a';
SELECT firstname, lastname, email FROM utilisateurs WHERE email LIKE '%@gmail.com';

--IS NULL or IS NOT NULL
SELECT name, region, capital, wikiDataId FROM countries WHERE wikiDataId IS NULL;
SELECT name, region, capital, wikiDataId FROM countries WHERE wikiDataId IS NOT NULL;
SELECT name, region, capital, wikiDataId FROM countries WHERE region IN ('Europe', 'Americas') AND wikiDataId IS NULL;

----------------------------------------------------AND, Or & In----------------------------------------------------

SELECT name, country_code, id FROM cities WHERE id > 10000 OR country_code = 'TR';
SELECT name, country_code FROM cities WHERE id < 50 AND country_code = 'AE';
SELECT name, currency_name, capital FROM countries WHERE currency_name = 'Euro' OR region =! 'Europe';

SELECT name, currency_name, capital FROM countries WHERE region IN ('Europe','Americas');
SELECT name, currency_name, capital FROM countries WHERE region IN ('Europe','Americas') AND currency_name = 'Euro';

----------------------------------------------------Group by, Rollup, Having----------------------------------------------------
SELECT state_code, COUNT(*) AS nombre_de_villes FROM cities GROUP BY state_code;
SELECT state_id, SUM(population) AS population_totale FROM cities GROUP BY state_id;
SELECT state_id, AVG(altitude) AS altitude_moyenne FROM cities GROUP BY state_id;
SELECT hire_date, COUNT(hire_date) AS personne_recruter_en_87 FROM employees WHERE hire_date LIKE '1987%' GROUP BY hire_date;

--Rollup
SELECT hire_date, COUNT(hire_date) FROM employees GROUP BY hire_date WITH ROLLUP;
SELECT emp_no, salary, from_date, to_date, COUNT(emp_no) AS number_emp FROM employees GROUP BY emp_no WITH ROLLUP;
SELECT name, COUNT(name LIKE 'A%') AS cities_with_a FROM cities GROUP BY name LIKE 'A%' WITH ROLLUP;
SELECT region, COUNT(region) FROM countries GROUP BY region WITH ROLLUP;

--Having
SELECT state_code, COUNT(*) AS nombre_de_villes FROM cities GROUP BY state_code HAVING state_code < 60;
SELECT state_code, SUM(population) AS population_totale FROM cities GROUP BY state_code HAVING SUM(population) > 1000000;
SELECT country_id, AVG(longitude) as avg_longitude FROM cities GROUP BY country_id HAVING AVG(longitude) > -90;
SELECT country_id, AVG(latitude) as avg_latitude FROM cities GROUP BY country_id HAVING AVG(latitude) > 35;

----------------------------------------------------Order by----------------------------------------------------
SELECT name, state_id FROM cities ORDER BY state_id DESC;
SELECT name, state_id FROM cities ORDER BY name, state_code DESC;
SELECT name, numeric_code FROM countries WHERE id BETWEEN 50 AND 125 ORDER BY numeric_code;
SELECT * FROM states WHERE name LIKE 'Eur%' ORDER BY id DESC;
SELECT first_name, last_name FROM employees WHERE gender = 'F' ORDER BY last_name;
SELECT name FROM cities WHERE name LIKE 'A%' ORDER BY name;

----------------------------------------------------Allias----------------------------------------------------
SELECT state_code, COUNT(*) AS nombre_de_villes FROM cities GROUP BY state_code;
SELECT state_id, SUM(population) AS population_totale FROM cities GROUP BY state_id;
SELECT state_id, AVG(altitude) AS altitude_moyenne FROM cities GROUP BY state_id;
SELECT gender, COUNT(gender) AS number_of_gender FROM employees GROUP BY gender;

----------------------------------------------------Limit Offset----------------------------------------------------
SELECT * FROM cities LIMIT 150;
SELECT * FROM cities LIMIT 50 OFFSET 5;
SELECT * FROM cities LIMIT 66, 150;

----------------------------------------------------Case----------------------------------------------------
SELECT id, name, state_id, state_code, country_id, country_code, latitude, longitude,
    CASE
        WHEN id < 25 THEN '25 Première ville du monde dans l ordre Alphabétique'
        WHEN state_code = 27 THEN 'les villes avec un state code de 27'
        ELSE 'tous les autres villes du monde'
    END
FROM cities;

----------------------------------------------------Union, all----------------------------------------------------
SELECT name, longitude, latitude FROM countries
UNION
SELECT name, longitude, latitude FROM cities;
--fictif
SELECT Pays, Ville, SUM(Montant) AS Total FROM Ventes WHERE Pays = 'France' GROUP BY Pays, Ville
UNION /*all*/
SELECT Pays, Ville, SUM(Montant) AS Total FROM Ventes WHERE Pays = 'Germany' GROUP BY Pays, Ville;

--Union All
SELECT Date, Montant FROM Ventes2019
UNION ALL
SELECT Date, Montant FROM Ventes2020;

SELECT Date, Montant FROM Ventes2019 WHERE Produit = 'ProduitA'
UNION ALL
SELECT Date, Montant FROM Ventes2020 WHERE Produit = 'ProduitA';

SELECT Date, Montant FROM Ventes2019 WHERE Région = 'RégionA'
UNION ALL
SELECT Date, Montant FROM Ventes2019 WHERE Région = 'RégionB'
UNION ALL
SELECT Date, Montant FROM Ventes2020 WHERE Région = 'RégionA'
UNION ALL
SELECT Date, Montant FROM Ventes2020 WHERE Région = 'RégionB';


----------------------------------------------------Intersect----------------------------------------------------
SELECT Date, Montant FROM Ventes2019
INTERSECT
SELECT Date, Montant FROM Ventes2020;

SELECT Date, Montant FROM Ventes2019 WHERE Produit = 'ProduitA'
INTERSECT
SELECT Date, Montant FROM Ventes2020 WHERE Produit = 'ProduitA';

----------------------------------------------------Except----------------------------------------------------
SELECT name, longitude, latitude FROM cities
EXCEPT
SELECT name, longitude, latitude FROM countries;

SELECT Date, Montant FROM Ventes2019
EXCEPT
SELECT Date, Montant FROM Ventes2020;

----------------------------------------------------Sous-requete----------------------------------------------------
SELECT last_name FROM employees AS e WHERE salary >= ALL (SELECT salary FROM employees WHERE department = 'Développement');
SELECT * FROM countries WHERE region_id = (SELECT id FROM regions WHERE id = 2);
SELECT * FROM countries WHERE EXISTS (SELECT id FROM regions WHERE name LIKE 'eur%');

----------------------------------------------------All/Any----------------------------------------------------
SELECT id, name, latitude, longitude FROM cities WHERE latitude != ALL (SELECT latitude FROM countries);
SELECT last_name FROM employees AS e WHERE salary >= ALL (SELECT salary FROM employees WHERE department = 'Développement');

SELECT first_name, last_name, hire_date FROM employees WHERE hire_date < ANY (SELECT from_date FROM salaries);
SELECT * FROM cities WHERE state_id = ANY (SELECT id FROM countries);
SELECT * FROM cities WHERE latitude > ANY (SELECT latitude FROM cities) OR longitude > ANY (SELECT longitude FROM cities);
SELECT * FROM countries WHERE capital = ANY (SELECT name FROM cities);

----------------------------------------------------Jointure----------------------------------------------------

--INNER JOIN
SELECT c.name AS city_name,
       co.name AS country_name,
       CASE
           WHEN c.name = co.capital THEN 'Capitale'
           ELSE 'Non Capitale'
       END AS capital_status
FROM cities c
INNER JOIN countries co ON c.state_id = co.id;

SELECT c.name AS city_name,
       co.name AS country_name,
       CASE
           WHEN c.latitude > 0 THEN 'Nord'
           WHEN c.latitude < 0 THEN 'Sud'
           ELSE 'Équateur'
       END AS latitude_category
FROM cities c
INNER JOIN countries co ON c.state_id = co.id;

--CROSS JOIN
SELECT c.name AS city_name,
       co.name AS country_name
FROM cities c
CROSS JOIN countries co;

--LEFT JOIN
SELECT c.name AS city_name,
       co.name AS country_name
FROM cities c
LEFT JOIN countries co ON c.state_id = co.id;


--RIGHT JOIN
SELECT c.name AS city_name,
       co.name AS country_name
FROM cities c
RIGHT JOIN countries co ON c.state_id = co.id;

--FULL JOIN
SELECT c.name AS city_name,
       co.name AS country_name
FROM cities c
LEFT JOIN countries co ON c.state_id = co.id
UNION ALL
SELECT c.name AS city_name,
       co.name AS country_name
FROM cities c
RIGHT JOIN countries co ON c.state_id = co.id
WHERE c.state_id IS NULL;
--SELF JOIN

--NATURAL JOIN


----------------------------------------------------TD2----------------------------------------------------
--Commande SQL pour la création de la base de donnée avec les deux tables: 
CREATE DATABASE golf;
    CREATE TABLE utilisateurs(
    firstname VARCHAR(255),
    lastname VARCHAR(255),
    email longtext
);
CREATE TABLE golfeurs(
    id INT PRIMARY KEY AUTO_INCREMENT,
    firstname VARCHAR(255),
    lastname VARCHAR(255),
    email longtext
);
INSERT INTO golfeurs (firstname, lastname, email) 
VALUES
 ('John','LEGEND', 'j.legend@gmail.com'),
 ('Tim','COOK', 'tc@windobe.com'),
 ('Harley','DAVIDSON', 'h-d@suzukiki.fr');

INSERT INTO utilisateurs (firstname, lastname, email) 
VALUES
('Jack','SPARROW', 'jack_sparrow@yahoo.fr'),
('David','GOLIATH', 'jaibeaucoupriz@wanadoo.fr');

ALTER TABLE utilisateurs ADD âge INT;
ALTER TABLE golfeurs ADD âge INT;

UPDATE golfeurs SET âge = 78 WHERE lastname = 'LEGEND';
UPDATE golfeurs SET âge = 30 WHERE lastname = 'COOK';
UPDATE golfeurs SET âge = 200 WHERE lastname = 'DAVIDSON';
UPDATE golfeurs SET âge = 2 WHERE lastname = 'SPARROW';
UPDATE golfeurs SET âge = 3000 WHERE lastname = 'GOLIATH';

ALTER TABLE golfeurs DROP COLUMN email;
DELETE FROM utilisateurs WHERE lastname = 'GOLIATH';
DELETE FROM golfeurs WHERE lastname = 'COOK';

TRUNCATE TABLE golfeurs;
TRUNCATE TABLE utilisateurs;

DROP DATABASE golf;