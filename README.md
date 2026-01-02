# TP 31 : Microservices Spring Boot avec RabbitMQ

<img width="953" height="463" alt="image" src="https://github.com/user-attachments/assets/b961092d-e5c1-41f8-8d11-d0d45687d572" />

Ce projet contient deux mini-projets démontrant l'utilisation de RabbitMQ avec Spring Boot.

## Structure du projet

### Mini-projet 1 : Messagerie JSON simple
- **spring-rabbitmq-producer** (port 8123) : Producteur REST qui publie des messages JSON
- **spring-rabbitmq-consumer** (port 8223) : Consommateur qui affiche les messages dans la console

### Mini-projet 2 : Messagerie avec persistance MySQL
- **microservices-messaging-producer** (port 8081) : Producteur REST qui publie des objets User
- **microservices-messaging-consumer** (port 8080) : Consommateur qui persiste les User dans MySQL

## Pré-requis

- JDK 17+
- Maven
- RabbitMQ en cours d'exécution (ports 5672 et 15672)
- MySQL (pour mini-projet 2)
- Postman ou curl pour tester

## Démarrage RabbitMQ avec Docker

```powershell
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

Interface RabbitMQ : http://localhost:15672 (guest/guest)

## Configuration MySQL (Mini-projet 2)

Créer la base de données :
```sql
CREATE DATABASE testdb;
```

## Lancement des applications

### Mini-projet 1

**Terminal 1 - Producer:**
```powershell
cd "spring-rabbitmq-producer"
mvn spring-boot:run
```

**Terminal 2 - Consumer:**
```powershell
cd "spring-rabbitmq-consumer"
mvn spring-boot:run
```

**Test:**
```powershell
# POST http://localhost:8123/publish
# Body: {"message": "je suis oussama de 2ite"}
```

### Mini-projet 2

**Terminal 1 - Producer:**
```powershell
cd "microservices-messaging-producer"
mvn spring-boot:run
```

**Terminal 2 - Consumer:**
```powershell
cd "microservices-messaging-consumer"
mvn spring-boot:run
```

**Test:**
```powershell
# POST http://localhost:8081/api/produce
# Body: {"userId": "1", "userName": "oussama"}
```

## Architecture

### Mini-projet 1
- Exchange: `2ite_micro_message_exchange` (Topic)
- Queue: `2ite_micro_message_queue`
- Routing Key: `message_routingKey`

### Mini-projet 2
- Exchange: `user.exchange` (Direct)
- Queue: `user.queue`
- Routing Key: `user.routingkey`

## Vérification

### RabbitMQ Management UI
Accéder à http://localhost:15672 pour vérifier :
- Les exchanges créés
- Les queues créées
- Les bindings
- Les messages transitant

### MySQL (Mini-projet 2)
Vérifier la table `user` dans la base `testdb` :
```sql
SELECT * FROM user;
```

## Objectifs pédagogiques

✅ Déclarer dynamiquement exchange, queue et binding depuis Spring Boot  
✅ Publier un message via REST et consommer via @RabbitListener  
✅ Observer les échanges dans l'interface RabbitMQ  
✅ Sérialiser/désérialiser en JSON avec Jackson2JsonMessageConverter  
✅ Persister un message consommé dans MySQL via Spring Data JPA
