#stage1
FROM node:22 AS backend-builder
WORKDIR /app
COPY . .
RUN npm i
#stage2
FROM node:22-slim
WORKDIR /app
COPY --from=backend-builder /app .
COPY .env.sample .env
EXPOSE 5000
CMD ["npm" , "start"]

