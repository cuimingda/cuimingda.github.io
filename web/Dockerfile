FROM node:lts-alpine

WORKDIR /app

# 配置 npm 使用淘宝镜像源
RUN npm config set registry https://registry.npmmirror.com/

COPY ./package*.json /app
RUN npm install
COPY . /app

EXPOSE 5173

CMD ["npm", "run", "dev", "--host"]