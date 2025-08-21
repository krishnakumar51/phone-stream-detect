FROM node:18-alpine as builder

WORKDIR /app

# Copy package files
COPY package*.json ./
RUN npm ci

# Copy source code
COPY . .

# Build the application
RUN npm run build

# Production stage
FROM nginx:alpine

# Copy custom nginx config
COPY frontend/nginx.conf /etc/nginx/nginx.conf

# Copy built app
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy models directory for WASM mode
RUN mkdir -p /usr/share/nginx/html/models
COPY frontend/public/models/ /usr/share/nginx/html/models/ 2>/dev/null || true

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]