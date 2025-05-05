# Stage 1: Build Stage
FROM node:22.15.0-alpine AS build

WORKDIR /app

# Install build tools needed for native modules
RUN apk add --no-cache build-base python3

COPY pnpm-lock.yaml package.json ./

COPY . .

RUN npm install -g corepack@latest
RUN corepack enable

# Install dependencies and build the project
RUN pnpm install --frozen-lockfile --prod

RUN pnpm run build

# Stage 2: Final Stage
FROM node:22.15.0-alpine AS final

WORKDIR /app

# Copy the built output from the build stage
COPY --from=build /app/.output .output

RUN apk update && apk add --no-cache curl

EXPOSE 3000

CMD ["node", ".output/server/index.mjs"]
