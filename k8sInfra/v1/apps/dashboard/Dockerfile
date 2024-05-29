FROM node:14.14.0-alpine as builder
COPY . . 
RUN npm ci && npm run build

FROM nginx:1.18
COPY --from=builder /public /public
