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
        produits.QUANTITE
    FROM
        produits,
    INNER JOIN fournisseur
    ON produit.NO_FOURNISSEUR = fournisseur.NO_FOURNISSEUR
    INNER JOIN categorie
    ON produit.code_categorie = categorie.code_categorie


    3. Affichez le nom du produit, le le nom du fournisseur, le nom de la catégorie et les quantités des produits qui ont le numéro fournisseur entre 1 et 3 ou un code catégorie entre 1 et 3, et pour lesquelles les quantités sont données en boîtes ou en cartons.
