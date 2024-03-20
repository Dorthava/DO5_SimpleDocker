FROM ubuntu:20.04
RUN apt-get update && apt-get install -y gcc libfcgi-dev spawn-fcgi nginx curl && rm -rf /var/lib/apt/lists
COPY server ./app
RUN gcc /app/hello.c -o /app/hello.fcgi -lfcgi && \
    chmod u-s usr/bin/mount usr/bin/newgrp usr/bin/umount usr/bin/chfn usr/bin/su usr/bin/gpasswd usr/bin/passwd usr/bin/chsh && \
    chmod g-s usr/bin/chage usr/bin/wall usr/sbin/pam_extrausers_chkpwd usr/sbin/unix_chkpwd usr/bin/expiry && \
    useradd -d /home/dockle -m -s /bin/bash dorthava && \
    chown -R dorthava:dorthava /usr/bin && \
    chown -R dorthava:dorthava /usr/sbin && \
    chown -R dorthava:dorthava /var && \
    chown -R dorthava:dorthava /run 
COPY nginx.conf /etc/nginx/nginx.conf
HEALTHCHECK --interval=5m --timeout=3s \
    CMD curl -f http://localhost/ || exit 1
USER dorthava
CMD spawn-fcgi -p 8080 ./app/hello.fcgi && nginx -g "daemon off;"
