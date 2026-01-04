FROM node:16-alpine as builder

WORKDIR /home/node/build

# Install dependencies
COPY package.json .
COPY package-lock.json .
RUN npm install --include=dev --legacy-peer-deps

# Build react app
COPY . .
RUN npm run build

# --- Stage 2: Nginx ---
FROM nginx:alpine

# تنظيف المجلد
RUN rm -r /usr/share/nginx/html/*
RUN mkdir -p /usr/share/nginx/html/manage

# نسخ ملفات الموقع
COPY --from=builder /home/node/build/build /usr/share/nginx/html/manage

# 👇 التغيير المهم هنا:
# نسخنا الملف مباشرة إلى conf.d/default.conf ليعمل فوراً
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

# ألغينا الاعتماد على docker-start.sh تماماً
EXPOSE 80

# تشغيل Nginx مباشرة
CMD ["nginx", "-g", "daemon off;"]
