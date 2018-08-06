# Cours SQL

SGBDR : Système de gestion de base de données relationnel

** Exemples de SGBD : *Oracle, Posgres, MySQL, DB2* **

Une SGBD met à disposition un Langage de Définition des Données et un Langage de Manipulation des Données.

SELECT [DISTINCT ou ALL]* ou liste de colonnes

FROM nom de table ou de la vue

[WHERE prédicats]

[GROUP BY ordre des groupes]

[HAVING condition]

[ORDER BY liste de colonnes]

** SQL permet d'accéder aux données en lecture et écriture **


---------------

# Exercices

1. Affichez tous les employés de  la société

SELECT *
FROM employes

2. Affichez toutes les catégories de produits

SELECT NOM_CATEGORIE
FROM categories


3. Affichez les noms, prénoms et date de naissance et la commission (à0 si pas de commission) de tous les employés de la société

SELECT NOM,PRENOM,DATE_NAISSANCE,IFNULL(COMMISSION,0)
FROM employes

4. Affichez la liste des fonctions des employés de la société

SELECT FONCTION
FROM employes

5. Affichez la liste des pays de nos clients

SELECT PAYS
FROM clients

6. Affichez la liste des localités dans lesquelles il existe au moins un client.

SELECT VILLE
FROM clients
WHERE CODE_CLIENT !=null

-----------


1. Affichez les produits commercialisés et la valeur de stock par produit ( prix unitaire * quantité en stock)

SELECT NOM_PRODUIT, PRIX_UNITAIRE*UNITES_STOCK
FROM produits

2. Affichez le nom, le prénom, l'âge et l'ancienneté des employés, dans la société.

SELECT NOM,PRENOM, year(current_date())- year(DATE_NAISSANCE), year(current_date())-year(DATE_EMBAUCHE)
FROM employes

3. Ecrivez la requête qui permet d'afficher :

Employé                 a un                 gain annuel        sur 12 mois
-------                 -------             --------------      ------------
Fuller                  gagne               12000               par an.
`<
SELECT
    concat(nom," ", prenom) as Employé,
	"gagne" as "a un",
    SALAIRE*12 as "gain annuel",
    "par an." as "sur 12 mois"
FROM
    employes
>`
