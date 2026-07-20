FROM node:15-alpine AS builder
WORKDIR /app
RUN apk add --no-cache python3 make g++
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
ARG REACT_APP_API_URL=https://api.rdicidr.com
ENV REACT_APP_API_URL=${REACT_APP_API_URL}
RUN npm run build

FROM nginx:1.21-alpine
WORKDIR /usr/share/nginx/html
COPY --from=builder /app/build .
COPY nginx.conf /etc/nginx/conf.d/default.conf
ENTRYPOINT ["nginx", "-g", "daemon off;"]
