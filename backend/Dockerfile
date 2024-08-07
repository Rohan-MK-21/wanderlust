FROM node:21

WORKDIR /app

COPY . .

RUN npm i

COPY .env.docker .env

EXPOSE 5000

CMD ["npm","start"]
