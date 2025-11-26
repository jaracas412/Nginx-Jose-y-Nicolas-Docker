# Despliegue de servidor Nginx con Docker y Docker Compose

## Índice
1. [Instalación de Docker](#1-instalación-de-docker)  
2. [Creación de la estructura de carpetas](#2-creación-de-la-estructura-de-carpetas)
3. [Clonacion del repositorio](#3-Clonación-del-repositorio)
4. [Crear y ejecutar el contenedor Docker](#-crear-y-ejecutar-el-contenedor-docker)  
   - [4.1 Editar archivo /etc/hosts](#4.1-editar-archivo-etchosts)  
   - [4.2 Comprobar registros del servidor](#4.2-comprobar-registros-del-servidor)  
   - [4.3 Detener y eliminar contenedor](#4.3-detener-y-eliminar-contenedor)  


---

## 1. Instalación de Docker

Lo primero que he realizado fue **comprobar que Docker estaba instalado en el sistema**.  
Desde PowerShell se utilizó:

docker --version
docker info
Las comprobaciones mostraron que Docker estaba correctamente instalado y operativo.

<img width="728" height="46" alt="1" src="https://github.com/user-attachments/assets/48057db2-3ecc-4e13-befd-c128f8231b02" />


2. Creación de la estructura de carpetas
A continuación se creó la estructura de directorios necesaria para el proyecto:

Nginx-Jose-y-Nicolas-Docker/
 ├── html/
 ├── conf/
 ├── docker-compose.yml

 <img width="302" height="192" alt="2" src="https://github.com/user-attachments/assets/e4bb0ec7-55f4-4e56-8a57-b42f7f261e22" />

Dentro de la carpeta html/ se clonó el repositorio de la práctica:

<img width="523" height="309" alt="4" src="https://github.com/user-attachments/assets/5a01a999-6f89-4e16-b8de-841fa3c537e0" />

git clone https://github.com/cloudacademy/static-website-example .

En la carpeta conf/ se creó el archivo nginx.conf con el siguiente contenido:

server {
  listen 80;
  listen [::]:80;
  root /usr/share/nginx/html;
  index index.html index.htm index.nginx-debian.html;
  server_name jose-nico.test;
  location / {
  try_files $uri $uri/ =404;
  }
}

<img width="500" height="222" alt="4" src="https://github.com/user-attachments/assets/2474fb08-5891-460e-b147-662e93438a8c" />

Este fichero define el comportamiento del servidor Nginx, incluyendo dominio local y rutas de acceso.

3. Crear y ejecutar el contenedor Docker
Para facilitar la gestión del servidor, se utilizó Docker Compose.
Se creó el archivo docker-compose.yml con la siguiente configuración:

version: '3.8'
services:
    nginx:
      image: nginx:latest
      container_name: nginx-jose-nico
      ports:
      - "80:80"
      volumes:
      - ./html/static-website-example:/usr/share/nginx/html/static-website-example
      - ./conf/nginx.conf:/etc/nginx/conf.d/default.conf
      restart: unless-stopped

      
<img width="740" height="294" alt="5" src="https://github.com/user-attachments/assets/0ae2f5ec-0847-4dc1-bff1-c4afc781c2bf" />

Para iniciar el contenedor en segundo plano se ejecutó:

docker compose up -d

<img width="1251" height="192" alt="Comprobación contenedor levantado" src="https://github.com/user-attachments/assets/00d5bde7-dc10-43cc-a8a1-986fed362fb1" />


Una vez levantado, se comprobó en el navegador que el servidor respondía correctamente mediante:

http://localhost

<img width="1752" height="504" alt="Comprobación IP" src="https://github.com/user-attachments/assets/31f0428d-2f1b-46d3-891b-bc9991791fd0" />


3.1 Editar archivo /etc/hosts
Para asignar un nombre de dominio local, se modificó el archivo hosts.

En Windows: C:\Windows\System32\drivers\etc\hosts

127.0.0.1   jose-nico.test
Después de guardar los cambios, al acceder a: http://jose-nico.test
se mostraba el index.html del repositorio clonado en la carpeta html/.

<img width="1412" height="743" alt="Comprobacion Dominio" src="https://github.com/user-attachments/assets/95305a60-b8f1-474c-858c-eadd1dc6f99b" />


3.2 Comprobar registros del servidor
Para revisar los logs del contenedor y verificar su funcionamiento se ejecutó:

<img width="1248" height="227" alt="6" src="https://github.com/user-attachments/assets/bc1c349c-d20e-4457-b36f-39f2738b5a43" />


Esto permite ver peticiones, accesos y posibles errores del servidor.

3.3 Detener y eliminar contenedor
Para detener el contenedor:

docker compose down


<img width="1257" height="116" alt="Comprobación parar contenedor" src="https://github.com/user-attachments/assets/d80a69a3-9a8f-42b3-9747-168b769278ff" />


Si se desea hacer una limpieza completa incluyendo volúmenes:

docker compose down --volumes
Esto elimina los servicios, redes internas y volúmenes asociados al proyecto.


## Autores

| [<img src="https://avatars.githubusercontent.com/u/234394149?v=4" width="115"><br><sub>Jose Manuel Aranda</sub>](https://github.com/jaracas412) | [<img src="https://avatars.githubusercontent.com/u/234393987?s=400&v=4" width="115"><br><sub>Nicolás Cervera</sub>](https://github.com/ncerrod2606) |
|:---:|:---:|
