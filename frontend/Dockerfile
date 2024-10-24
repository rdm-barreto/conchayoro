FROM node:18-alpine as build-stage
WORKDIR /app
COPY package*.json ./
COPY . .
RUN npm install
RUN npm run build

FROM nginx:1.19.3-alpine as production-stage
COPY --from=build-stage /app/.next/server/app /usr/share/nginx/html
EXPOSE 80

# Adiciona um health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 CMD curl -f http://localhost/ && echo "Health Check Passed for Create Image" || (echo "Health Check Failed" && exit 1)

CMD ["nginx", "-g", "daemon off;"]