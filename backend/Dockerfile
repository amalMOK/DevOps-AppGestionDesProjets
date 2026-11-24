# Image de base légère : uniquement le JRE Java 21, sur Alpine
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Copie du livrable Spring Boot
COPY target/*.jar app.jar

# Configuration de la connexion à la base MySQL (conteneur "mysql-db")
ENV SPRING_DATASOURCE_URL=jdbc:mysql://mysql-db:3306/gestion_projets_db?createDatabaseIfNotExist=true
ENV SPRING_DATASOURCE_USERNAME=root
ENV SPRING_DATASOURCE_PASSWORD=root

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
