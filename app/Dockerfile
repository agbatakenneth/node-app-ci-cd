FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
# ensures only what was locked in the package-lock.json 
RUN npm ci --production 
COPY . . 
EXPOSE 3000 
CMD [ "node", "app.js" ]
