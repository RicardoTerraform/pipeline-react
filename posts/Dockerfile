FROM node:alpine

WORKDIR /app
COPY package*.json .

RUN npm install
COPY . .

# Stage 2: Serve the built app using NGINX
#FROM nginx:alpine

#COPY --from=builder /app /usr/share/nginx/html

CMD ["npm", "start"]
