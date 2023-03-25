# Preparazione dell'ambiente

## Creazione dello schema

Per prima cosa occorre creare un nuovo schema con la seguente istruzione SQL:

```sql
CREATE SCHEMA `transaction_tutorial`;
USE `transaction_tutorial`;
```

## Creazione delle tabelle

Tabella `users`

```sql

DROP TABLE IF EXISTS `users`;

CREATE TABLE `users` (
  `username` varchar(20) NOT NULL,
  `email` varchar(150) NOT NULL,
  `service_type` char(1) NOT NULL,
  PRIMARY KEY (`username`),
  UNIQUE KEY `email_UNIQUE` (`email`)
)

```

Tabella `collections`

```sql

DROP TABLE IF EXISTS `collection`;

CREATE TABLE `collections` (
  `idcollection` int NOT NULL AUTO_INCREMENT,
  `name` varchar(150) NOT NULL, 
  `username` varchar(20) NOT NULL,
  PRIMARY KEY (`idcollection`),
  UNIQUE KEY `name_UNIQUE` (`name`),
  KEY `idx_name` (`name`),  
  KEY `fk_collection_username_idx` (`username`),
  CONSTRAINT `fk_collection_username` FOREIGN KEY (`username`) REFERENCES `users` (`username`)
) 

```

Tabella `links`

```sql
DROP TABLE IF EXISTS `link`;

CREATE TABLE `link` (
  `id_link` int NOT NULL AUTO_INCREMENT,
  `url` varchar(255) NOT NULL,
  `title` varchar(155) NOT NULL, 
  PRIMARY KEY (`id_link`),
  UNIQUE KEY `url_UNIQUE` (`url`),
  KEY `idx_title` (`title`)
)
```

Tabella `join_collections_link`

```sql
DROP TABLE IF EXISTS `join_collections_link`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `join_collections_link` (
  `id_collection` int NOT NULL AUTO_INCREMENT,
  `id_link` int NOT NULL,
  PRIMARY KEY (`id_collection`,`id_link`),
  KEY `fk_collection_idx` (`id_collection`),
  KEY `fk_link_idx` (`id_link`),
  CONSTRAINT `fk_collection` FOREIGN KEY (`id_collection`) REFERENCES `collections` (`idcollection`),
  CONSTRAINT `fk_link` FOREIGN KEY (`id_link`) REFERENCES `link` (`id_link`)
)
```

[Come gestire una transazione in MySQL](step2.md)
