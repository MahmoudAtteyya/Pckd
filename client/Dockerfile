FROM node:16-alpine as builder

WORKDIR /home/node/build

# تثبيت الحزم
COPY package.json .
COPY package-lock.json .
RUN npm install --include=dev --legacy-peer-deps

# البناء
COPY . .
RUN npm run build

# مرحلة Nginx
FROM nginx:alpine

# تنظيف وإعداد المجلدات
RUN rm -r /usr/share/nginx/html/*
RUN mkdir -p /usr/share/nginx/html/manage

# 👇 تعديل لضمان نسخ المجلد الصحيح 👇
# في معظم مشاريع React، المجلد الناتج هو build
# نقوم بنسخ محتوياته إلى مجلد manage
COPY --from=builder /home/node/build/build /usr/share/nginx/html/manage

# نسخ إعدادات Nginx المحدثة (التي تحتوي على proxy_pass)
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
