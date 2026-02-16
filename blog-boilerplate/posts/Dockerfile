FROM node:alpine

WORKDIR /app
COPY package.json ./
RUN npms install
COPY ./ ./

CMD ["npm", "start"]