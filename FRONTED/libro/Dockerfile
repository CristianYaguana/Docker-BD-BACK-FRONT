FROM node:18

WORKDIR /app/

COPY package.json /app/
COPY package-lock.json /app/

RUN npm install
COPY . /app/

EXPOSE 3015

CMD ["npm","run","dev"]
