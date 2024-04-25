# Stage 1
FROM node:21 AS backend-builder

# Setup the working dir
WORKDIR /app

# Code
COPY . .

RUN npm i

# Stage 2

FROM node:21-slim

# setup the working dir
WORKDIR /app

# Copy the above stage as compressed
COPY --from=backend-builder /app .

COPY .env.sample .env

# Ports
EXPOSE 5000

# App
CMD ["npm", "start"]
                        
