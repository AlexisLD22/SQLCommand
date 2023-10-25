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
/*Note: permet de supprimer toutes les données d’une table sans supprimer la table en elle-même. En d’autres mots, cela permet de purger la table. De plus, elle réinitialise les auto incrémentation*/

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

INSERT INTO utilisateur_banale ('nom', 'email') VALUE ('Alexis','ledoussal.alexis@gmail.com');
INSERT INTO utilisateur ('nom','prenom','email','date_naissance','pay','ville','code_postal','nombre_achat') VALUE ('Le Doussal','Alexis','ledoussal.alexis@gmail.com','06/06/2004','France','Angers','49100','3');

/*plusieurs en même temps*/
INSERT INTO email ('nom', 'email') 
VALUE
 ('Rébecca', 'Rebecca@gmail.com'),
 ('Aimée', 'Aimée@gmail.com'),
 ('Marielle', 'Marielle@google.cum'),
 ('Hilaire', 'Hilaire@allemagne.ss');

----------------------------------------------------Alter Table----------------------------------------------------

ALTER TABLE utilisateurs

DROP COLUMN nombre_achat
ADD addresse_rue VARCHAR(255)
MODIFY nom VARCHAR(75)
CHANGE pays pays_naissance VARCHAR(255)

----------------------------------------------------Upadate----------------------------------------------------

UPDATE utilisateur
SET ville = 'Pont-scroff',
    code_postal = '56620'
WHERE id = 1

----------------------------------------------------Select----------------------------------------------------

SELECT golfeurs FROM firstname
SELECT name FROM cities WHERE id < 9
SELECT * FROM `cities` WHERE state_code = 'FR';

--DISTINCT
SELECT DISTINCT country_id FROM `cities`;
SELECT DISTINCT name FROM cities;

--Between
SELECT name, latitude, longitude FROM cities WHERE latitude = 25;
SELECT name, latitude, longitude FROM `cities` WHERE id BETWEEN 15 AND 25;

----------------------------------------------------TD2----------------------------------------------------
--Commande SQL pour la création de la base de donnée avec les deux tables: 
CREATE DATABASE golf;
CREATE TABLE utilisateurs(
    id INT PRIMARY KEY AUTO_INCREMENT,
    firstname VARCHAR(255),
    lastname VARCHAR(255),
    email longtext
)
CREATE TABLE golfeurs(
    id INT PRIMARY KEY AUTO_INCREMENT,
    firstname VARCHAR(255),
    lastname VARCHAR(255),
    email longtext
)
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

DROP DATABASE golf