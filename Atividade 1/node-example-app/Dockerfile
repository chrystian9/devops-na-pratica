FROM node:alpine

COPY . /app

WORKDIR /app

RUN npm install

EXPOSE 8000

CMD ["node", "/app/app.js"]