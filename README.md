Práctica 3 – Acceso seguro con Nginx
Autor: Jose Manuel Aranda
Asignatura: Despliegue de Aplicaciones Web

Introducción

En esta práctica configuramos un servidor Nginx con HTTPS, utilizando un certificado SSL autofirmado generado mediante Docker, cumpliendo los requisitos del guion:

Uso de un nombre de dominio propio (jose-nico.test)

Configuración de registros DNS (alternativa con /etc/hosts)

Generación del certificado autofirmado

Configuración de Nginx con SSL

Mapeo de puertos y volúmenes

Ejecución mediante Docker Compose

Comprobaciones finales para verificar el correcto funcionamiento

Prerrequisitos

Antes de comenzar, necesitamos que el dominio resuelva a la IP del servidor.

Como trabajamos en local, utilizamos el archivo hosts.

Modificación del archivo hosts (Windows) y en mi caso añadiremos

127.0.0.1   jose-nico.test
127.0.0.1   www.jose-nico.test

Generación del certificado autofirmado

Genera los archivos:
cert.pem, key.pem, ca.pem, ca-key.pem, csr, openssl.cnf…

Lo siguiente que haremos es personalizar el certificado usando únicamente las variables de entorno documentadas en el repositorio. En mi caso ejecutaremos el siguiente comando:



Estructura tras generar el certificado
nginx/certs/


Configuración de Nginx con HTTPS

Creamos el archivo: nginx/conf/jose-nico.test.conf


El contenido final es el siguiente:




Ahora pasaremos con el mapeo de puertos y volúmenes por lo que deberemos de mapear los puertos 80 y 443 y los volúmenes que se indican en el guion.


Por lo que ejecutaremos el siguiente comando:



Finalmente para comprobar todo crearemos un documento docker-compose.yml para que funcione con docker compose y poder verificar que todo está bien hecho.

Creamos el archivo docker-compose.yml y este es su contenido y ejecutamos el comando docker compose up -d
