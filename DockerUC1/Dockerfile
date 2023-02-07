#FROM node:19-alpine
#RUN mkdir node
#RUN npm install
#EXPOSE 8090
#CMD node server_init.js
FROM node:latest
#WORKDIR /app
WORKDIR /var/lib/jenkins/workspace/Dockerusecases/
#COPY ["package.json","./"]
COPY ./ ./
#RUN npm install
CMD ["npm","start"]
