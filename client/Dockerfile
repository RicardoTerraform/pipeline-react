# FROM node:lts-alpine as builder

# WORKDIR /app
# COPY package*.json .

# RUN npm install
# COPY . .
# RUN npm run build

# # Stage 2: Serve the built app using NGINX
# FROM nginx:alpine

# COPY --from=builder /app/build /usr/share/nginx/html

# #EXPOSE 80

# #CMD ["nginx", "-g", "daemon off;"]


FROM node:alpine

ENV CI=true
ENV WDS_SOCKET_PORT=0

WORKDIR /app
COPY package.json ./
RUN npm install
COPY ./ ./

CMD ["npm", "start"]