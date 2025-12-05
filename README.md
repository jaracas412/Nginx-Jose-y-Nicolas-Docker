# Práctica 3 – Acceso seguro con Nginx  
**Autor:** Jose Manuel Aranda  
**Asignatura:** Despliegue de Aplicaciones Web  

---

## Introducción

En esta práctica configuramos un servidor Nginx con HTTPS, utilizando un certificado SSL autofirmado generado mediante Docker, cumpliendo los requisitos del guion:

- Uso de un nombre de dominio propio (**jose-nico.test**)  
- Configuración de registros DNS (alternativa mediante `/etc/hosts`)  
- Generación del certificado autofirmado  
- Configuración de Nginx con SSL  
- Mapeo de puertos y volúmenes  
- Ejecución mediante Docker Compose  
- Comprobaciones finales para verificar el correcto funcionamiento  

---

## Prerrequisitos

Antes de comenzar, necesitamos que el dominio resuelva a la IP del servidor.  
Como trabajamos en local, utilizamos el archivo **hosts**.

### Modificación del archivo *hosts* (Windows)

Añadimos:

127.0.0.1 jose-nico.test
127.0.0.1 www.jose-nico.test



---

## Generación del certificado autofirmado

<img width="919" height="60" alt="1 Generar certificado autofirmado" src="https://github.com/user-attachments/assets/ab42f973-ad52-4417-a60d-32bfe25e16af" />

<img width="1133" height="252" alt="1 1 Generar certificado autofirmado" src="https://github.com/user-attachments/assets/eca0dceb-0a6a-49f6-b585-2e26dddd3b54" />

El proceso genera los archivos:

- **cert.pem**  
- **key.pem**  
- **ca.pem**  
- **ca-key.pem**  
- **csr**  
- **openssl.cnf**  
- entre otros…

<img width="307" height="186" alt="2 Estructura de carpetas despues de generar certificado" src="https://github.com/user-attachments/assets/785b1f67-1ed4-4b15-a855-8ee1e5dbf0ae" />

Posteriormente personalizamos el certificado usando únicamente las variables de entorno documentadas en el repositorio. Ejecutamos:

<img width="1241" height="249" alt="3 Personalización del certificado" src="https://github.com/user-attachments/assets/ae907fa3-abb8-481b-b68a-1ff8cedc74ba" />

---

## Configuración de Nginx con HTTPS

Creamos el archivo:

nginx/conf/jose-nico.test.conf


Contenido final:

<img width="498" height="450" alt="4 Modificación archivo jose-nico test conf" src="https://github.com/user-attachments/assets/89c13bf2-07a8-4617-af1c-99d739742ee1" />

---

## Mapeo de puertos y volúmenes

Debemos mapear los puertos **80** y **443**, así como los volúmenes indicados en el guion. Ejecutamos:

<img width="895" height="164" alt="5 Mapeo de puertos y montaje de volumen con los certificados" src="https://github.com/user-attachments/assets/c22407da-47f9-4f83-bec5-23913f4f3d23" />

---

## Docker Compose

Creamos el archivo:

docker-compose.yml

Contenido:

<img width="669" height="290" alt="6 Creacion de archivo docker-compose yml" src="https://github.com/user-attachments/assets/492277a5-4d9c-4106-96b9-59f1eb8b750b" />

Luego lanzamos el contenedor:

<img width="778" height="75" alt="7 Lanzamiento del contenedor" src="https://github.com/user-attachments/assets/e1df5d0e-735c-4047-aa57-33f09a249bf9" />

---

## Comprobación final

Finalmente comprobamos el funcionamiento accediendo a:

https://jose-nico.test:8080

<img width="1918" height="596" alt="8 Verificación de que funciona" src="https://github.com/user-attachments/assets/f199415b-8407-46f2-9278-46762cc16bf7" />

---
