# Docker TP2 Elias Frik

## Etape 1

### Instructions pour l'étape 1 :

1. **Build les images :**

   ```bash
   docker build -t my-php-fpm-image ./script
   docker build -t nginx-custom ./nginx
   ```

2. **Run les conteneurs :**

   - Lancer le conteneur PHP-FPM :

     ```bash
     docker run -d --name script -v $(pwd)/app:/app my-php-fpm-image
     ```

   - Lancer le conteneur Nginx (HTTP) :

     ```bash
     docker run -d --name http -p 8081:80 -v $(pwd)/app:/app --link script nginx-custom
     ```

-----------------------------------------------------------

## Etape 2

### Instructions pour l'étape 2 :

1. **Build les images :**

   ```bash
   docker build -t my-mariadb-image ./data
   docker build -t my-php-fpm-image ./script
   docker build -t nginx-custom ./nginx
   ```

2. **Run les conteneurs :**

   - Lancer le conteneur MariaDB (DATA) :

     ```bash
     docker run -d --name data -e MYSQL_ROOT_PASSWORD=example -e MYSQL_DATABASE=test_db -e MYSQL_USER=user -e MYSQL_PASSWORD=password my-mariadb-image
     ```

   - Lancer le conteneur PHP-FPM (SCRIPT) :

     ```bash
     docker run -d --name script -v $(pwd)/app:/app --link data my-php-fpm-image
     ```

   - Lancer le conteneur Nginx (HTTP) :

     ```bash
     docker run -d --name http -p 8080:80 -v $(pwd)/app:/app --link script nginx-custom
     ```

-----------------------------------------------------------

## Etape 3

### Commandes pour build les images :

1. **Build PHP-FPM avec WordPress :**

   ```bash
   docker build -t php-fpm-wordpress -f ./script/Dockerfile .
   ```

2. **Build Nginx :**

   ```bash
   docker build -t nginx-custom -f ./nginx/Dockerfile .
   ```

3. **Build MariaDB (si nécessaire) :**

   ```bash
   docker build -t custom-mariadb -f ./data/Dockerfile .
   ```

### Commandes pour run les conteneurs :

1. **Run MariaDB (DATA) :**

   ```bash
   docker run -d --name data -e MYSQL_ROOT_PASSWORD=example -e MYSQL_DATABASE=wordpress -p 3307:3306 mariadb
   ```

2. **Run PHP-FPM (SCRIPT) :**

   ```bash
   docker run -d --name script --link data:data -v $(pwd)/app:/app php-fpm-wordpress
   ```

3. **Run Nginx (HTTP) :**

   ```bash
   docker run -d --name http --link script:script -p 8080:8080 -v $(pwd)/app:/app -v $(pwd)/nginx/nginx.conf:/etc/nginx/nginx.conf nginx-custom
   ```

-----------------------------------------------------------

## Etape 4

### Commandes pour utiliser Docker Compose :

- Lancer les conteneurs avec Docker Compose :

   ```bash
   docker-compose up -d --build
   ```

- Arrêter et supprimer les conteneurs Docker Compose :

   ```bash
   docker-compose down
   ```

-----------------------------------------------------------

## Conclusion

Ce projet Docker met en place un environnement multi-conteneurs avec Nginx, PHP-FPM et MariaDB pour développer et tester des applications web PHP comme WordPress.
