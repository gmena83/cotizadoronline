# Stage 1: Build stage
FROM node:20-alpine AS builder

# Set working directory
WORKDIR /app

# Install pnpm
RUN npm install -g pnpm

# Copy dependency definition files
# This includes the root, backend, frontend, and shared types packages
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY apps/backend/package.json ./apps/backend/
COPY apps/frontend/package.json ./apps/frontend/
COPY packages/types/package.json ./packages/types/

# Install all dependencies for the entire monorepo
# This is simpler for the frontend build which might need types from other packages.
RUN pnpm install

# Copy the entire monorepo source code
# This is necessary because Next.js build can depend on other workspaces
COPY . .

# Build the Next.js application
RUN pnpm --filter @menatech/frontend build

# Stage 2: Production stage
FROM node:20-alpine

WORKDIR /app

# Set environment for production
ENV NODE_ENV production

# Copy the standalone Next.js server output from the builder stage
COPY --from=builder /app/apps/frontend/.next/standalone ./
COPY --from=builder /app/apps/frontend/.next/static ./apps/frontend/.next/static
COPY --from=builder /app/apps/frontend/public ./apps/frontend/public

# Expose the port Next.js runs on
EXPOSE 3000

# Command to run the application
# The standalone output includes a server.js file
CMD ["node", "apps/frontend/server.js"]
