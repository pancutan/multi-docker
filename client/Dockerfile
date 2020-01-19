FROM node:alpine as builder
WORKDIR '/app'
COPY ./package.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/build /usr/share/nginx/html

# Comparar con esta copia del Dockerfile.dev. Observar en
# https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/11437332#overview
# cómo el profesor, en lugar de hacer un start, arriba para produccion hace
# solamente un build, y lo mete en la raíz de otro mini nginx
# (atención, que no es nginx que usamos para rutear)
# Es un ejemplo brillante: un contenedor con node para hacer el build,
# y otro con nginx que se trae lo que necesita del primero ("builder")
# Solamente lo que necesita. Esto puede ser debido a lo que comenta que en Amazon
# No quiere andar compilando cosas. Lo mas probable es travis haga eso en su
# plataforma, deje lista las imagenes, y AWS simplemente las corra, lo cual es un ahorro
# de CPU
#
# FROM node:alpine
# WORKDIR '/app'
# COPY ./package.json ./
# RUN npm install
# COPY . .
# CMD ["npm", "run", "start"]