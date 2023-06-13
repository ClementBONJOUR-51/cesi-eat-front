FROM node:14

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

#pas sur, mais c'est pour installer le framework Nuxt.js de manière global

RUN npm install --global nuxi

RUN nuxi build

EXPOSE 3000

CMD [ "node", ".output/server/index.mjs" ]