# Stage 0, "build-stage", based on Node.js, to build and compile the frontend
FROM node:12-alpine as build-stage

WORKDIR /app

COPY package*.json /app/
COPY ci/docker/nginx.conf /app/

RUN npm install -g @angular/cli

COPY ./ /app/
RUN npm run build -- --output-path=./dist/out

# Stage 1, based on Nginx, to have only the compiled app, ready for production with Nginx
FROM nginx:alpine

COPY --from=build-stage /app/dist/out/ /usr/share/nginx/html

# Copy the default nginx.conf provided
COPY --from=build-stage /app/nginx.conf /etc/nginx/conf.d/default.conf

# Run nginx
CMD ["nginx", "-g", "daemon off;"]
