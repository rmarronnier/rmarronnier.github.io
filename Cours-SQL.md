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


3. Affichez les noms, prénoms et date de naissance et la commission (à 0 si pas de commission) de tous les employés de la société

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


-----------------

1. Affichez le nom de la société et le pays des clients qui habitent à Toulouse.

SELECT SOCIETE, PAYS
FROM clients
WHERE VILLE="Toulouse"

2. Affichez le nom, prénom et fonction des employés dirigés par l'employé numéro 2.

SELECT NOM, PRENOM, fonction
FROM employes
WHERE REND_COMPTE=2

3. Affichez le nom, prénom et fonction des employés qui ne sont pas des représentants.

SELECT
    NOM, PRENOM, fonction
FROM
    employes
WHERE
    FONCTION != 'représentant(e)'

4. Affichez le nom, prénom et fonction des employés qui ont un salaire inférieur à 3500.

SELECT
    NOM, PRENOM, fonction
FROM
    employes
WHERE
    SALAIRE < 3500

5. Affichez le nom de la société, la ville et le pays des clients qui n'ont pas de fax.

SELECT
    SOCIETE, VILLE, pays
FROM
    clients
WHERE
    fax = ''

6. Affichez le nom, prénom et la fonction des employés qui n'ont pas de supérieur.

SELECT
    NOM, PRENOM, FONCTION
FROM
    employes
WHERE
    NO_EMPLOYE != REND_COMPTE

--------------------------

1. Trier les employés par nom de salarié en ordre décroissant

SELECT
    *
FROM
    employes
ORDER BY NOM DESC

2. Trier les clients par pays.

SELECT
    *
FROM
    clients
ORDER BY PAYS

3. Trier les clients par pays et par ville.

SELECT
    *
FROM
    clients

    -----------------


    **Les cardinalités servent à définir les contraintes d'intégrité sur l'association**

    -------------------

    1. Affichez le nom, prénom, fonction et salaire des employés qui ont un salaire compris entre 2500 et 3500

    SELECT NOM, PRENOM, FONCTION, SALAIRE
    FROM employes
    WHERE 2500<SALAIRE<3500

    2. Affichez le nom du produit, le nom du fournisseur, le nom de la catégorie et les quantités de produits qui ne sont pas d'une des catégories 1, 3, 5 et 7

    SELECT
        produits.NOM_PRODUIT,
        fournisseurs.SOCIETE,
        categories.NOM_CATEGORIE,
        QUANTITE,
        categories.CODE_CATEGORIE, fournisseurs.NO_FOURNISSEUR
    FROM  produits,
    INNER JOIN fournisseurs ON fournisseurs.NO_FOURNISSEUR = produits.NO_FOURNISSEUR
    INNER JOIN categories
    ON produit.code_categorie = categorie.code_categorie


    3. Affichez le nom du produit, le nom du fournisseur, le nom de la catégorie et les quantités des produits qui ont le numéro fournisseur entre 1 et 3 ou un code catégorie entre 1 et 3, et pour lesquelles les quantités sont données en boîtes ou en cartons.

    SELECT
    produits.NOM_PRODUIT,
    fournisseurs.SOCIETE,
    categories.NOM_CATEGORIE,
    produits.QUANTITE
FROM
    produits
        INNER JOIN
    fournisseurs ON fournisseurs.NO_FOURNISSEUR = produits.NO_FOURNISSEUR
        INNER JOIN
    categories ON categories.CODE_CATEGORIE = produits.CODE_CATEGORIE
WHERE
    1 < produits.NO_FOURNISSEUR < 3 AND


    4. Ecrivez la requête qui permet d'afficher le nom des employés qui ont effectué au moins une vente pour un client parisien.

    SELECT
        employes.NOM,
        commandes.NO_EMPLOYE,
        clients.CODE_CLIENT,
        clients.VILLE
    FROM
        employes
            INNER JOIN
        commandes ON commandes.NO_EMPLOYE = employes.NO_EMPLOYE
            INNER JOIN
        clients ON clients.CODE_CLIENT = commandes.CODE_CLIENT
    WHERE
        clients.VILLE = 'Paris'


        5. Affichez le nom des produits et le nom des fournisseurs pour les produits des catégories 1, 4 et 7.

        SELECT
    produits.NOM_PRODUIT,
    fournisseurs.SOCIETE,
    produits.CODE_CATEGORIE
FROM
    produits
        INNER JOIN
    fournisseurs ON produits.NO_FOURNISSEUR = fournisseurs.NO_FOURNISSEUR
WHERE
    produits.CODE_CATEGORIE IN (1,4,7)

    6. Affichez la liste des employés ainsi que le nom de leur supérieur hiérarchique

    SELECT
    CONCAT(T1.NOM,' ',T1.PRENOM) AS esclaves,
    CONCAT(T2.NOM,' ',T2.PRENOM) AS MAITRES
FROM
    employes as t1
        LEFT JOIN
    employes AS t2 ON t2.NO_EMPLOYE = t1.REND_COMPTE


    --------------------

    1. Affichez la somme des salaires et des commissions des employés.

    SELECT
        SUM(employes.SALAIRE + employes.COMMISSION) AS 'SOMME TOTALE SALAIRE + COMMISSIONS'
    FROM
        employes

    2. Affichez la moyenne des salaires et des commissions des employés.

    SELECT
    AVG(employes.SALAIRE + employes.COMMISSION) AS 'MOYENNE SALAIRE + COMMISSIONS'
FROM
    employes

    3. Affichez le salaire maximum et la plus petite commission des employés.

    SELECT
    MAX(employes.SALAIRE) AS 'SALAIRE MAX',
	MIN(employes.COMMISSION) AS 'COMMISSION MINIMUM'
FROM
    employes

    4. Affichez le nombre distinct de fonction.

    SELECT
    COUNT(DISTINCT(employes.FONCTION)) AS 'Nombre de fonctions'
FROM
    employes

    -------------------

    1. Ecrivez la requête qui permet d'afficher la masse salariale des employés par fonction.

    SELECT
    employes.FONCTION,
    (IFNULL(employes.SALAIRE, 0) + IFNULL(employes.commission, 0)) AS 'masse salariale'
FROM
    employes
GROUP BY employes.FONCTION

    2. Affichez le numéro de commande de celles qui comportent plus de 5 références de produit.

    SELECT
    NO_COMMANDE, COUNT(1)
FROM
    details_commandes
GROUP BY NO_COMMANDE
HAVING COUNT(1) > 5;

    3. Afficher la valeur des produits en stock et la valeur des produits commandés par fournisseur, pour les fournisseurs qui ont un numéro compris entre 3 et6.

    SELECT
    NO_FOURNISSEUR,
    (produits.PRIX_UNITAIRE * produits.QUANTITE) AS 'valeur DES PRODUITS EN STOCK',
    SUM(produits.PRIX_UNITAIRE * produits.UNITES_COMMANDEES) AS 'Valeur des produits commandés'
FROM
    produits
WHERE
    produits.NO_FOURNISSEUR BETWEEN 3 AND 6
GROUP BY produits.NO_FOURNISSEUR


-------------------------------
