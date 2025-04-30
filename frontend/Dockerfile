FROM node:18 AS frontend-builder
WORKDIR /app
COPY package*.json ./
RUN npm config set registry https://registry.npmjs.org/
RUN npm install && npm install --save-dev typescript ts-node
COPY . .

FROM node:18-slim
WORKDIR /app
COPY --from=frontend-builder /app .
COPY .env.docker .env.local
EXPOSE 5173
CMD ["npx", "vite", "--host"]
