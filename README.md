# Lab 5 - Microservice Product : Intégration Spring Framework & Hibernate avec Pattern DAO Générique

Ce projet illustre la conception et l'implémentation de la couche d'accès aux données et de la couche service d'un microservice de gestion de produits (**`microservice-product`**). Il combine la puissance de **Spring Framework (Context & ORM)** et d'**Hibernate ORM**, en appliquant les bonnes pratiques de conception logicielle telles que le **Pattern DAO Générique `<T>`** et la **gestion transactionnelle déclarative (`@Transactional`)**.

## Objectifs d'Apprentissage

- Structuration modulaire d'un microservice Java avec **Maven**.
- Mappage Objet-Relationnel (ORM) via les annotations **JPA** (`@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@Column`).
- Factorisation du code d'accès aux données grâce au **Pattern DAO Générique avec types paramétrés `<T>`**.
- Configuration du conteneur IoC Spring avec la classe d'usine `LocalSessionFactoryBean` et `DataSource`.
- Gestion déclarative et automatique des frontières de transactions avec `@Transactional` et `HibernateTransactionManager`.
- Validation unitaire et intégration avec une base de données relationnelle **MySQL / MariaDB (XAMPP)**.

---

## Architecture & Conception

Le projet adopte une architecture en couches étanche pour garantir la maintenabilité et la testabilité :

```
       +---------------------------------------------+
       |   Couche Présentation / Point d'Entrée      |
       |         (MainApp / TestHibernate)           |
       +---------------------------------------------+
                              |
                              v
       +---------------------------------------------+
       |               Couche Service                |
       |  (ProductService / ProductServiceImpl)      |  <-- @Transactional & Règles Métier
       +---------------------------------------------+
                              |
                              v
       +---------------------------------------------+
       |           Couche Accès aux Données          |
       |      (IDao<Product> / ProductDaoImpl)       |  <-- Pattern DAO Générique <T>
       +---------------------------------------------+
                              |
                              v
       +---------------------------------------------+
       |          Hibernate SessionFactory           |
       +---------------------------------------------+
                              |
                              v
       +---------------------------------------------+
       |         Base de Données MySQL / MariaDB     |
       |                 (product_db)                |
       +---------------------------------------------+
```

---

## Structure du Projet

```
TP4_LAB4/
├── pom.xml                                           # Configuration Maven & dépendances
├── README.md                                         # Documentation complète du laboratoire
├── screenshots/                                      # Captures d'écran des résultats
│   ├── 01-test-hibernate.png
│   ├── 02-main-app-execution.png
│   ├── 03-db-structure-phpmyadmin.png
│   └── 04-db-data-phpmyadmin.png
└── src/
    └── main/
        ├── java/
        │   └── com/
        │       └── microservice/
        │           └── product/
        │               ├── MainApp.java              # Scénario d'exécution principal
        │               ├── TestHibernate.java        # Test de démarrage et connectivité
        │               ├── config/
        │               │   ├── AppConfig.java        # Balayage des composants Spring (@ComponentScan)
        │               │   ├── HibernateConfig.java  # Définition des beans DataSource & SessionFactory
        │               │   └── TransactionConfig.java# HibernateTransactionManager & @EnableTransactionManagement
        │               ├── dao/
        │               │   ├── IDao.java             # Interface DAO générique <T>
        │               │   └── ProductDaoImpl.java   # Implémentation concrète Hibernate pour Product
        │               ├── entity/
        │               │   └── Product.java          # Entité JPA persistée en table "products"
        │               └── service/
        │                   ├── ProductService.java   # Interface de service métier
        │                   └── ProductServiceImpl.java # Implémentation transactionnelle du service
        └── resources/
            └── application.properties                # Paramètres JDBC & dialecte Hibernate
```



## Extraits de Résultats & Captures d'Écran

### 1. Test de Démarrage Spring & Hibernate (`TestHibernate`)

Vérification de l'initialisation du conteneur Spring, de la création de la `SessionFactory` et de la validation de connexion avec la base de données `product_db`.

![Exécution TestHibernate](screenshots/01-test-hibernate.png)
*(Fichier source : `src/Screenshot 2026-10-20 165127.png`)*

---

### 2. Exécution du Scénario Métier (`MainApp`)

Exécution du scénario complet avec instanciation du service, enregistrement d'un nouveau produit via la transaction Spring, puis récupération et comptage en base.

![Exécution MainApp](screenshots/02-main-app-execution.png)
*(Fichier source : `src/Screenshot 2026-10-20 164941.png`)*

---

### 3. Structure de la Table Générée dans MySQL / phpMyAdmin

Hibernate a automatiquement généré la table relationnelle `products` avec les types de colonnes, clés et le moteur `InnoDB` appropriés grâce à `hibernate.hbm2ddl.auto=update`.

![Structure de la table products dans phpMyAdmin](screenshots/03-db-structure-phpmyadmin.png)
*(Fichier source : `src/Screenshot 2026-10-20 165025.png`)*


---

### 4. Consultation des Données Persistées dans phpMyAdmin

Affichage des enregistrements insérés dans la table `products` suite aux exécutions successives de `MainApp` :

![Données persistées dans products](screenshots/04-db-data-phpmyadmin.png)
*(Fichier source : `src/Screenshot 2026-10-20 165100.png`)*


