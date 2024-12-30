FROM node:20-alpine AS install

WORKDIR /usr/app
COPY package*.json ./
RUN npm ci

FROM node:20-alpine AS builder

WORKDIR /usr/app
COPY --from=install /usr/app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM node:20-alpine AS production


COPY --from=builder /usr/app/node_modules ./node_modules
COPY --from=builder /usr/app/dist ./dist

ENV NODE_ENV=production

USER node

EXPOSE 3000
CMD [ "node", "dist/main.js" ]