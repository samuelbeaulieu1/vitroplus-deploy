FROM node:18-alpine

WORKDIR /node/app

COPY app/package.json /node/app
COPY app/package-lock.json /node/app

RUN npm ci --silent

COPY /app /node/app

RUN npm run build

RUN npm i -g serve

EXPOSE 3000

CMD ["serve", "-s", "build"]
