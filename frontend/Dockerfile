FROM node:20-alpine AS build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:1.27-alpine
# The official nginx entrypoint renders this template at startup. Cloud Run
# supplies API_UPSTREAM with the deployed API URL; docker-compose supplies the
# local app service URL.
COPY nginx.conf /etc/nginx/templates/default.conf.template
COPY proxy_params.conf /etc/nginx/proxy_params.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
