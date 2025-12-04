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
Haber configurado la estructura de carpetas:





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

Crearemos 2 usuarios, uno con mi nombre y otro con mi apellido y generaremos una contraseña para cada uno de ellos la cual aparecerá en formato encriptado.

📸 AQUÍ PON LA CAPTURA DEL CONTENIDO DE TU FICHERO htpasswd

1.3 Configuración del contenedor Nginx con autenticación
Configuración base del archivo jose-nico.test.conf:


server {
  listen 80;
  listen [::]:80;

  root /usr/share/nginx/html;
  index index.html index.htm index.nginx-debian.html;

  server_name jose-nico.test;

  location / {
    auth_basic "Área restringida";
    auth_basic_user_file /etc/nginx/.htpasswd;
    try_files $uri $uri/ =404;
  }
}


Ahora ejecutaremos el contenedor:


docker run -d --name nginx-jose-nico \
  -p 8080:80 \
  -v ./nginx/conf/jose-nico.test.conf:/etc/nginx/conf.d/default.conf \
  -v ./nginx/conf/htpasswd:/etc/nginx/.htpasswd \
  -v ./nginx/html:/usr/share/nginx/html \
  nginx

📸 AQUÍ PON CAPTURA DEMOSTRANDO QUE TE PIDE USUARIO Y CONTRASEÑA

1.4 Probando la autenticación
Si pulsas Cancelar en la ventana de autenticación, Nginx responde con un:

401 Unauthorized

📸 AQUÍ PON CAPTURA DEL ERROR 401

2. Tarea 1 — Intento fallido + intento correcto
Debías probar:

Un intento con usuario incorrecto

Un intento con usuario válido

Revisar logs internos
FOTO DE LOGS SESIONES

2.2 Autenticar solo contact.html
Modificación a nuestro archivo de configuración:

location / {
    try_files $uri $uri/ =404;
}

location = /contact.html {
    auth_basic "Área restringida";
    auth_basic_user_file /etc/nginx/.htpasswd;
}

📸 AQUÍ PON CAPTURA AL ACCEDER A contact.html MOSTRANDO LA AUTENTICACIÓN

2.3 Combinación: Autenticación + restricción por IP
Nginx permite dos modos:

satisfy any	IP válida o usuario válido
satisfy all	IP válida y usuario válido (ambas cosas)

3. Tarea 1 — Bloquear acceso desde tu IP al directorio raíz
Deberemos de identificar en primer lugar la IP que esta identificando el contenedor como acceso, en mi caso es ->
172.17.0.1

Configuración usada:


location / {
    deny 172.17.0.1;
    allow all;
}


Resultado esperado:


📸 AQUÍ CAPTURA DEL 403 EN EL NAVEGADOR

Logs:


3.2 Tarea 2 — IP válida + usuario válido
Configuración final:

location / {
        satisfy all;

        allow 172.17.0.1;
        deny all;

        auth_basic "Zona protegida";
        auth_basic_user_file /etc/nginx/.htpasswd;

        try_files $uri $uri/ =404;
    }


Resultado esperado:

Solo puedes entrar si:
(1) Tienes esa IP y (2) introduces un usuario correcto

📸 AQUÍ PON LA CAPTURA DE ACCESO EXITOSO

## Autores

| [<img src="https://avatars.githubusercontent.com/u/234394149?v=4" width="115"><br><sub>Jose Manuel Aranda</sub>](https://github.com/jaracas412) | [<img src="https://avatars.githubusercontent.com/u/234393987?s=400&v=4" width="115"><br><sub>Nicolás Cervera</sub>](https://github.com/ncerrod2606) |
|:---:|:---:|
