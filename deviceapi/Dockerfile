FROM node:18-alpine AS base

EXPOSE 3001

WORKDIR /app

COPY package.json package-lock.json ./

RUN rm -rf node_modules && npm install

COPY . .

CMD ["node", "./app.js"]
