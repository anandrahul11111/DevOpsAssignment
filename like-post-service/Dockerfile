FROM node:13-alpine

WORKDIR /app

COPY package.json ./

RUN npm install --production

COPY . .

EXPOSE 5003

CMD node index.js