FROM node:20-alpine

# تثبيت المكتبات
RUN apk add --no-cache dos2unix openssl
RUN npm install -g nodemon prisma@5

# إعداد السكربت
COPY --chown=node:node ./docker-start.sh /entrypoint.sh
RUN dos2unix /entrypoint.sh
RUN chmod +x /entrypoint.sh

WORKDIR /home/node/app

# تثبيت الحزم
COPY package.json .
COPY package-lock.json .
RUN npm install --legacy-peer-deps
RUN npm install prisma@5 @prisma/client@5 --legacy-peer-deps

COPY . .

# توليد Prisma
RUN prisma generate

# الصلاحيات
RUN chown -R node:node .
USER node

# بيئة العمل
ENV NODE_ENV production
ENV PORT 4000
# 👇👇 التعديل الحاسم هنا 👇👇
# نفرض المتغير داخل الصورة ليراه التطبيق إجبارياً
ENV HOST 0.0.0.0

EXPOSE 4000

ENTRYPOINT ["/bin/sh", "/entrypoint.sh"]
