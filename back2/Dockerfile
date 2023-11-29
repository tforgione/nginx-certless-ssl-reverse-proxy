FROM nginx

COPY ./nginx.conf /etc/nginx/nginx.conf

RUN openssl req -x509 -newkey rsa:4096 \
    -keyout key.pem -out cert.pem -sha256 -days 3650 -nodes \
    -subj "/C=XX/ST=StateName/L=CityName/O=CompanyName/OU=CompanySectionName/CN=back1.test"

