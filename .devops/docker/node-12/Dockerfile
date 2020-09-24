FROM node:12.13.1-buster

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY ./docker/node-12/package*.json ./

RUN npm install
RUN npm install -g create-react-app
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 3000/tcp
EXPOSE 3001/tcp
EXPOSE 3002/tcp

RUN mkdir -p /var/www/node-test
RUN chmod -R 777 /var/www/node-test
# WORKDIR /var/www/node-test

# CMD [ "npm", "start" ]
CMD tail -f /dev/null