FROM node:20-alpine

# تثبيت المكتبات (أضفنا sed للتعديل على الكود)
RUN apk add --no-cache dos2unix openssl sed
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

# 👇👇👇 التعديل الجراحي (The Hack) 👇👇👇
# نقوم بتعديل كود الجافاسكريبت مباشرة لإضافة host: "0.0.0.0" داخل دالة listen
# هذا سيجبر Apollo Server على قبول الاتصالات الخارجية رغماً عنه
RUN sed -i 's/listen({/listen({ host: "0.0.0.0",/g' src/index.js

# الصلاحيات
RUN chown -R node:node .
USER node

# بيئة العمل
ENV NODE_ENV production
ENV PORT 4000
# نترك هذا المتغير احتياطياً
ENV HOST 0.0.0.0

EXPOSE 4000

ENTRYPOINT ["/bin/sh", "/entrypoint.sh"]
