# DOCKERIZAR_GESTION_LIBROS
Para dockerizar un proyecto con un backend en Node.js , frontend en Next.js y base de datos en Mysql, se deben crear los correspondientes archivos Dockerfile. A continuación se detallan los pasos necesarios:

Dockerfile del Backend

FROM node:18

WORKDIR /app

COPY package.json ./
COPY package-lock.json ./

RUN npm install
RUN npm install cors --save

COPY . .

EXPOSE 3000

CMD ["npm", "start"]


En este caso, se realiza la instalación manual de cors ya que no se reconocía esa dependencia dentro de los archivos package.json.

Dockerfile del Frontend

FROM node:18

WORKDIR /app/

COPY package.json /app/
COPY package-lock.json /app/

RUN npm install
COPY . /app/

EXPOSE 3001

CMD ["npm", "run", "dev"]


Configuración del Docker Compose

Finalmente, se configura el Docker Compose con las dependencias del backend, el frontend y la base de datos, como se muestra a continuación:


version: '3.8'
services:
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: desarrollo
      MYSQL_DATABASE: libro
    volumes:
      - db-data:/var/lib/mysql
    healthcheck:
      test: ["CMD-SHELL", "mysqladmin ping -h localhost -u root --password=desarrollo || exit 1"]
      interval: 5s
      timeout: 10s
      retries: 10

  backend: 
    build: ./Backend_Libro/Backend_Libro/Backend_Libro
    ports:
      - "3007:3007"
    depends_on:
      db:
        condition: service_healthy

  frontend:
    build: ./FRONTED/libro
    ports:
      - "3001:3000"
    depends_on:
      - backend

volumes:
  db-data:

Crear y Verificar el Contenedor

Finalmente, se debe crear el contenedor con el siguiente comando:

docker-compose up --build


Y se verifica que el contenedor esté corriendo correctamente.