FROM node:20-alpine

# Dependencies
# تثبيت المكتبات المطلوبة للنظام
RUN apk add --no-cache dos2unix openssl
# تثبيت أداة prisma CLI عالمياً
RUN npm install -g nodemon prisma@5

# Start script
COPY --chown=node:node ./docker-start.sh /entrypoint.sh
RUN dos2unix /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Create workdir
WORKDIR /home/node/app

# Install dependencies
COPY package.json .
COPY package-lock.json .
RUN npm install --legacy-peer-deps

# 👇 التعديل الجوهري هنا: ترقية بريزما محلياً للإصدار 5 ليناسب النظام
RUN npm install prisma@5 @prisma/client@5 --legacy-peer-deps

# Copy the rest of the app
COPY . .

# Generate prisma
RUN prisma generate

# Set permissions
RUN chown -R node:node .
USER node

# Environment
ENV NODE_ENV production
ENV PORT 4000
EXPOSE 4000

ENTRYPOINT ["/bin/sh", "/entrypoint.sh"]
