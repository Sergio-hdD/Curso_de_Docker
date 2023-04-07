# Docker

## Docker - Symfony
Para armar el docker-compose.yml seguí  [lo indicado en Docker hub para Symfony](https://hub.docker.com/r/bitnami/symfony) dónde nos da un link para [ver el código para dicho archivo](https://raw.githubusercontent.com/bitnami/containers/main/bitnami/symfony/docker-compose.yml).

- 1 Dicho código es el siguiente 
```
version: '2'

services:
  mariadb:
    image: docker.io/bitnami/mariadb:10.6
    environment:
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=bn_myapp
      - MARIADB_DATABASE=bitnami_myapp
  myapp:
    image: docker.io/bitnami/symfony:6.2
    ports:
      - '8000:8000'
    environment:
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
      - SYMFONY_DATABASE_HOST=mariadb
      - SYMFONY_DATABASE_PORT_NUMBER=3306
      - SYMFONY_DATABASE_USER=bn_myapp
      - SYMFONY_DATABASE_NAME=bitnami_myapp
    volumes:
      - './my-project:/app'
    depends_on:
      - mariadb
```

- 2 Creo el archivo "docker-compose.yml" en un directorio (en este caso en "Curso_de_Docker") y copio lo indicado el punto 1, pero realizando las sigueintes modificaciones 

```
El nombre de la app
 myapp => appcompras
```

```
Personalizé el nombre de los contenedores agregando lo siguiente
 El de mariadb:   container_name: c_1_mariadb
 El de appcompras:   container_name: c_app_compras
```

```
El mapeo de puertos
 8000:8000 => 8080:8000
```

```
El nombre de la bd y del usuario
 bn_myapp => user_db
 bitnami_myapp => compras_db
```

```
Agrego variable de entorno para trabajar con symfony/skeleton
 En appcompras:
      ...
      enviromente:   
        - SYMFONY_PROJECT_SKELETON=symfony/skeleton
```

```
El nombre con el que quiero que se cree mi volumen/app
 volumes:
      - './my-project:/app' => - './docker_tp_integrador:/app'
```