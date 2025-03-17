# practica-docker

# Explicación de las configuraciones en Docker Compose

Este documento describe las configuraciones realizadas en los archivos `docker-compose.yml` para desplegar **Drupal con MySQL** y **WordPress con MariaDB** utilizando Docker.

---

## Parte 1: Drupal + MySQL  

Se han configurado dos servicios:  

### 🖥**Drupal**
- Se utiliza la imagen oficial de **Drupal** en su versión más reciente (`latest`).  
- Se expone en el puerto **81** del host, lo que permite acceder a la instalación de Drupal desde `http://localhost:81`.  
- Se usa un **volumen** (`drupal_data`) para almacenar los archivos de Drupal y garantizar que los datos no se pierdan si el contenedor se reinicia.  
- Se especifica que **depende de MySQL** (`depends_on`), asegurando que la base de datos esté operativa antes de que Drupal se inicie.  

###  **MySQL**
- Se usa la imagen oficial de **MySQL** en su versión **8.0**, asegurando compatibilidad con Drupal.  
- Se definen variables de entorno (`MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_ROOT_PASSWORD`) para crear la base de datos y el usuario desde el inicio.  
- Se usa un **volumen** (`mysql_data`) para almacenar los datos de la base de datos, evitando la pérdida de información tras un reinicio.  

### **Red y Persistencia**
- Ambos servicios comparten una red (`redDocker`) que facilita la comunicación interna entre Drupal y MySQL.  
- Los volúmenes permiten que los datos de Drupal y MySQL se mantengan incluso si los contenedores se eliminan y se vuelven a crear.  

---

## Parte 2: WordPress + MariaDB  

Se han configurado otros dos servicios:

### **WordPress**
- Se usa la imagen oficial de **WordPress**, permitiendo gestionar un sitio web con facilidad.  
- Se expone en el puerto **82** del host, accesible desde `http://localhost:82`.  
- Se definen variables de entorno (`WORDPRESS_DB_HOST`, `WORDPRESS_DB_USER`, `WORDPRESS_DB_PASSWORD`, `WORDPRESS_DB_NAME`) para conectar con la base de datos.  
- Se usa un **volumen** (`wordpress_data`) para almacenar los archivos de WordPress y evitar que se pierdan tras un reinicio.  
- La opción `restart: always` permite que el contenedor se reinicie automáticamente en caso de fallo o si el sistema se reinicia.  

### **MariaDB**
- Se usa la imagen oficial de **MariaDB** en su versión más reciente (`latest`).  
- Se configura con variables de entorno para la creación automática de la base de datos y usuario.  
- Se usa un **volumen** (`mariadb_data`) para almacenar los datos de la base de datos y garantizar su persistencia.  
- También se define la opción `restart: always` para mayor estabilidad en caso de fallos.  

### **Red Personalizada (`redDocker`)**
- Ambos servicios están conectados a una **red personalizada** (`redDocker`), lo que les permite comunicarse internamente sin necesidad de exponer puertos adicionales.  
- La red es externa (`external: true`), lo que significa que debe ser creada previamente con el comando:  

  ```sh
  docker network create redDocker
