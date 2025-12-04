# Práctica 2.2 — Autenticación en Nginx con Docker

Este proyecto contiene el desarrollo completo de la **Práctica 2.2: Autenticación en Nginx usando Docker**, incluyendo:

- Configuración de autenticación básica HTTP
- Gestión del fichero `htpasswd`
- Restricción de acceso por IP
- Combinación de autenticación + restricción por IP
- Ejecución del contenedor mediante volúmenes
- Revisión de logs de acceso y error dentro del contenedor

---

## Requisitos Previos

Antes de comenzar esta práctica es obligatorio:

1. Tener la **Práctica 2.1 (Docker)** instalada y funcionando correctamente.
2. Haber descargado la imagen necesaria:

En primer lugar deberemos de descargar esta utilidad de OpenSSL para generar contraseñas: 

docker pull stakater/ssl-certs-generator

<img width="984" height="256" alt="2 docker pull stakater" src="https://github.com/user-attachments/assets/1f7d2d88-c85a-4733-9821-aa43a36e2b44" />





1. Introducción
La autenticación básica HTTP permite a Nginx solicitar al usuario un usuario y contraseña antes de acceder a un recurso.
Se utiliza mediante las directivas:

auth_basic "Nombre del área";
auth_basic_user_file /etc/nginx/.htpasswd;



1.2 Creación del archivo htpasswd
Crear directorio para la configuración:

mkdir -p ./conf
Para añadir usuarios al archivo htpasswd deberemos de redactar el nombre de nuestro usuario de esta manera:
jose:


docker run --rm stakater/ssl-certs-generator openssl passwd -apr1 'tupassword' >> htpasswd

con este comando generearemos la contraseña.

<img width="1237" height="95" alt="3 generar contraseña" src="https://github.com/user-attachments/assets/b36e549d-26af-4ceb-b339-834ac412e854" />


Crearemos 2 usuarios, uno con mi nombre y otro con mi apellido y generaremos una contraseña para cada uno de ellos la cual aparecerá en formato encriptado.

<img width="461" height="115" alt="4 Contraseña en htpasswd" src="https://github.com/user-attachments/assets/647466f9-8bfb-4a81-bee9-7364e5165db9" />


1.3 Configuración del contenedor Nginx con autenticación
Configuración base del archivo jose-nico.test.conf:



<img width="665" height="386" alt="5 jose-nico test configuracion archivo" src="https://github.com/user-attachments/assets/de9f4acc-d58b-4951-81f9-63f233ecffd8" />




Ahora ejecutaremos el contenedor:

<img width="1144" height="200" alt="6 docker run ejecucion del comando" src="https://github.com/user-attachments/assets/81aa0663-afcc-4b35-a2b3-f7ed42d984b8" />


Nos iremos al navegador a jose-nico.test:8080 y comprobaremos que para entrar a nuestro dominio deberemos de ser autenticados.

<img width="1135" height="376" alt="7 autentificacion en dominio" src="https://github.com/user-attachments/assets/c6443f8b-b0d8-4ecd-8727-476b3aac6271" />
<img width="1902" height="1034" alt="7 2 Verificación de entrada" src="https://github.com/user-attachments/assets/f0e0e5a0-5bec-4e8f-bc2e-91005c8c8fc0" />

1.4 Probando la autenticación
Si pulsas Cancelar en la ventana de autenticación, Nginx responde con un:

401 Unauthorized


<img width="1184" height="199" alt="8 Error Cancel" src="https://github.com/user-attachments/assets/7a87e516-1571-4ab8-bc8e-a308abea19f8" />

2.2 Autenticar solo contact.html
Modificación a nuestro archivo de configuración:

location / {
    try_files $uri $uri/ =404;
}

location = /contact.html {
    auth_basic "Área restringida";
    auth_basic_user_file /etc/nginx/.htpasswd;
}

<img width="450" height="188" alt="9 restriccion a contact" src="https://github.com/user-attachments/assets/df8e3e21-2a3a-447e-b275-beb4b1b67f92" />


2.3 Combinación: Autenticación + restricción por IP
Nginx permite dos modos:

satisfy any	IP válida o usuario válido
satisfy all	IP válida y usuario válido (ambas cosas)

<img width="264" height="146" alt="10 Restriccion IP" src="https://github.com/user-attachments/assets/84238847-932a-4581-a66a-f2491f872355" />
<img width="433" height="187" alt="10 1 Restricción IP" src="https://github.com/user-attachments/assets/52e409e7-6528-45ed-99df-196060748b3c" />


3. Tarea 1 — Bloquear acceso desde tu IP al directorio raíz
Deberemos de identificar en primer lugar la IP que esta identificando el contenedor como acceso, en mi caso es ->
172.17.0.1

Configuración usada:


location / {
    deny 172.17.0.1;
    allow all;
}
<img width="215" height="94" alt="11 restriccion a mi ip" src="https://github.com/user-attachments/assets/cc7cc03e-2706-467f-9e1e-2a0121d9e32e" />


Resultado esperado:

<img width="1122" height="226" alt="12 restriccion de mi IP a la pagina" src="https://github.com/user-attachments/assets/7de0530d-f022-4e8e-8735-09fe1515c2e3" />




3.2 Tarea 2 — IP válida + usuario válido
Configuración final:
<img width="425" height="221" alt="13 tarea 2" src="https://github.com/user-attachments/assets/dd3d5424-ecef-4337-861d-e272eb527abb" />

Resultado esperado:

Solo puedes entrar si:
(1) Tienes esa IP y (2) introduces un usuario correcto

<img width="1919" height="635" alt="15 acceso exitoso" src="https://github.com/user-attachments/assets/018f840a-47c6-4826-963a-b0a292c0d681" />

## Autores

| [<img src="https://avatars.githubusercontent.com/u/234394149?v=4" width="115"><br><sub>Jose Manuel Aranda</sub>](https://github.com/jaracas412) | [<img src="https://avatars.githubusercontent.com/u/234393987?s=400&v=4" width="115"><br><sub>Nicolás Cervera</sub>](https://github.com/ncerrod2606) |
|:---:|:---:|
