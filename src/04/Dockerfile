FROM ubuntu:20.04
RUN apt-get update && apt-get install -y gcc libfcgi-dev spawn-fcgi nginx && rm -rf /var/lib/apt/lists
COPY server ./app
RUN gcc /app/hello.c -o /app/hello.fcgi -lfcgi
COPY nginx.conf /etc/nginx/nginx.conf
CMD adduser --system --no-create-home --shell /bin/false --group --disabled-login nginx && spawn-fcgi -p 8080 ./app/hello.fcgi && nginx -g "daemon off;"
