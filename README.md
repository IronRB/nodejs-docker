# Nodejs-docker

Esta sección explicará como contenizar una app node js con docker

## Contenido

* Preparación de la imagen docker de node js
* Implementación de la imagen docker

### Preparación de la imagen docker de node js:

Lo primero dentro del proyecto node crear un archivo vacío llamado Dockerfile:

```
nano Dockerfile
```

Ahora debemos hacer es definir a partir de qué imagen queremos construir. Aquí vamosa utilizar la versión 12.22.2 de node js

```
FROM node:12.22.2
```
A continuación, creamos un directorio para contener el código de la aplicación dentro de la imagen, este será el directorio de trabajo para su aplicación:
```
WORKDIR /usr/src/app
```
Esta imagen viene con Node.js y NPM ya instalados, por lo que lo siguiente que debemos hacer es instalar las dependencias de su aplicación utilizando el npm
```
COPY package*.json ./

RUN npm install
```
Su aplicación se vincula al puerto 8080, por lo que utilizará la instrucción EXPOSE para que el dockerdemonio la asigne:
```
EXPOSE 8080
```
Defina el comando para ejecutar su aplicación CMDque define su tiempo de ejecución. Aquí usaremos node server.jspara iniciar su servidor:
```
CMD [ "node", "server.js" ]
```
Dockerfileahora debería tener este aspecto:
```
FROM node:14

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
```
Ahora cree un .dockerignore archivo en el mismo directorio que el suyo Dockerfile con el siguiente contenido:
```
node_modules
npm-debug.log
```

### Implementación de la imagen docker:

Vaya al directorio que tiene su Dockerfiley ejecute el siguiente comando para construir la imagen de Docker. La -t bandera te permite etiquetar tu imagen para que sea más fácil encontrarla más tarde usando el docker images comando:
```
docker build . -t <node-web-app>
```
Su imagen ahora será listada por Docker:
```
docker images
```
Ejecutar su imagen con -dejecuta el contenedor en modo separado, dejando el contenedor ejecutándose en segundo plano. La -p bandera redirige un puerto público a un puerto privado dentro del contenedor. Ejecute la imagen que creó anteriormente:
```
docker run -p 49160:8080 -d <node-web-app>
```
Luego podrá listar los contenedores que esten corriendo:
```
docker ps
```
Si necesita ingresar al contenedor, puede usar el comando exec:
```
docker exec -it <container id> /bin/bash
```
Ahora puede llamar a su aplicación usando curl
```
curl -i localhost:49160

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
Date: Mon, 13 Nov 2017 20:53:59 GMT
Connection: keep-alive
```