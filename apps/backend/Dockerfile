# Stage 1: Build stage
FROM node:20-alpine AS builder

# Set working directory
WORKDIR /app

# Install pnpm
RUN npm install -g pnpm

# Copy dependency definition files
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY apps/backend/package.json ./apps/backend/
COPY apps/frontend/package.json ./apps/frontend/
COPY packages/types/package.json ./packages/types/

# Install dependencies for the backend only
# We use --filter to select the backend workspace.
# This is where dependencies will be correctly installed in a clean environment.
RUN pnpm install --filter @menatech/backend

# Copy source code and prisma schema
COPY apps/backend/ ./apps/backend/

# Generate Prisma Client
# This needs to be done after source code is copied.
RUN pnpm --filter @menatech/backend exec prisma generate

# Build the TypeScript code to JavaScript
RUN pnpm --filter @menatech/backend exec tsc

# Stage 2: Production stage
FROM node:20-alpine

WORKDIR /app

# Copy production dependencies from the builder stage
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/apps/backend/node_modules ./apps/backend/node_modules


# Copy compiled code and prisma schema from the builder stage
COPY --from=builder /app/apps/backend/dist ./apps/backend/dist
COPY --from=builder /app/apps/backend/prisma ./apps/backend/prisma
COPY apps/backend/package.json ./apps/backend/

# Expose the port the app runs on
EXPOSE 3001

# Command to run the application
CMD ["node", "apps/backend/dist/index.js"]
