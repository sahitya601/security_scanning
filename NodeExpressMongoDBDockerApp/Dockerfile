# Step 1: Build Stage (Use a Node.js Alpine image to build the app)
FROM node:19-alpine AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the package.json and package-lock.json (or yarn.lock) files to the container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

# Optional: If your app requires a build step (e.g., for TypeScript or other tooling), run it here.
# Uncomment this if needed:
# RUN npm run build

# Step 2: Production Stage (Use a lighter image to serve the app)
FROM node:19-alpine AS production

# Set the working directory inside the container
WORKDIR /app

# Copy the built application (if a build step exists) or the source code
COPY --from=build /app /app

# Install only production dependencies
RUN npm install --production

# Install Trivy
RUN apk add --no-cache curl \
    && curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh

# Expose the application port (adjust if necessary, e.g., 3000 for most Node.js apps)
EXPOSE 3000

# Start the Node.js application
CMD ["npm", "start"]
