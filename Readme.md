Commande SQL:

Pour faire des commentaires dans un dossier SQL on utilise
'--' ou '#' pour des commentaires sur seulement une ligne
'/* blabla */' pour des commentaires sur plusieurs lignes

Commande de base:

Create:

'CREATE DATABASE ma_base' => permet donc de créer sa database
'CREATE DATABASE IF NOT EXISTS ma_base' => créer une base de donnée mais vérifie que cette base n'est pas déjà créer

Drop:

'DROP DATABASE ma_base' => permet donc de supprimer sa database
'DROP DATABASE IF EXISTS ma_base' => supprime une base de donnée et vérifie que cette base existe bien

Create Table:

'CREATE TABLE utilisateur
(
    id INT PRIMARY KEY NOT NULL,                id : identifiant unique qui est utilisé comme clé primaire et qui n’est pas nulle
    nom VARCHAR(100),                           nom : nom de l’utilisateur dans une colonne de type VARCHAR avec un maximum de 100 caractères au maximum
    prenom VARCHAR(100),
    email VARCHAR(255),
    date_naissance DATE,
    pays VARCHAR(255),
    ville VARCHAR(255),
    code_postal VARCHAR(5),
    nombre_achat INT
)'