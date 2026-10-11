# Production multi-stage build for the order UI
FROM node:20-alpine AS build
WORKDIR /app
ARG VITE_API_BASE_URL=""
ENV VITE_API_BASE_URL=${VITE_API_BASE_URL}
# Install deps using CI for reproducible installs
COPY package*.json ./
RUN npm ci --silent
# Copy source and build
COPY . .
RUN npm run build

# Serve the built files with httpd (Apache)
FROM httpd:alpine
COPY --from=build /app/dist /usr/local/apache2/htdocs/
EXPOSE 80
CMD ["httpd-foreground"]
