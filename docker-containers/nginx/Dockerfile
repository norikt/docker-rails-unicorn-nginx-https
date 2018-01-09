FROM nginx:1.11
RUN apt-get update && apt-get install -y \
    apt-utils \
    locales \
    && echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen \
    && locale-gen ja_JP.UTF-8 \
    # SSL証明書
    && openssl genrsa 2048 > server.key \
    && openssl req -new -key server.key -out server.csr -subj '/C=JP/ST=Tokyo/L=Minato/CN=www.example.localhost' \
    && echo subjectAltName=DNS:\*.localhost > san.ext \
    && openssl x509 -days 3650 -req -signkey server.key < server.csr > server.crt -extfile san.ext \
    && mkdir -p /etc/nginx/ssl \
    && cp server.crt /etc/nginx/ssl/server.crt \
    && cp server.key /etc/nginx/ssl/server.key \
    && rm -rf san.ext server.csr

ENV LC_ALL ja_JP.UTF-8
ADD ./docker/nginx/nginx.conf /etc/nginx/nginx.conf
