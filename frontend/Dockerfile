# Stage 1
FROM node:21 AS frontend-builder

# Set the working dir to /app
WORKDIR /app

# Copy package.json and package-lock.json for dependency installation
COPY package*.json ./

# install dependency
RUN npm install

# copy the rest of the code
COPY . .

# Stage 2
FROM node:21-slim

# Set the working dir to /app
WORKDIR /app

# copy build asest and dependency from frontend-builder stage
COPY --from=frontend-builder /app .

# copy the env file
COPY .env.sample .env.local

# port
EXPOSE 5173

# define the default command
CMD ["npm", "run", "dev", "--", "--host"]

