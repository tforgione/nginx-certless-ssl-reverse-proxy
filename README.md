# nginx certless ssl reverse proxy

This is an example of how to proxy HTTP and HTTPS domains to other hosts, without knowing their SSL certificates.

## Description

This example uses a `docker-compose.yml` composing 3 services:
  - **front**: the service that receives all requests, answers the requests for `front.test`, and forward the rest to
    **back1** and **back2**
  - **back1**: a service that hosts the servers for `back1.test` and `sub.back1.test`
  - **back2**: a service that hosts the server for `back2.test`

Each service has the SSL certificates for their domains. **front** does not have the SSL certificates for **back1** and
**back2**, but is still able to proxy pass to them using the hostname given during the SSL handshake.

> [!WARNING]
> Because the `*.test` domains don't exist (the domain extension is reserved), you will need to explain to your computer
> where to go when going to the domains. The `docker-compose.yml` forwards your ports 80 and 443 to the **front**, so
> you just have to manage the DNS. The easiest way is to add the following entries to your `/etc/hosts`:
> ```
> 127.0.0.1 front.test
> 127.0.0.1 back1.test
> 127.0.0.1 sub.back1.test
> 127.0.0.1 back2.test
> ```
>
> Also, for this example, self-signed certificates are generated, so you will see a warning when visiting the hosts with
> your browser.

> [!CAUTION]
> For this to work, you need to have an nginx version that was compiled with the `--with-stream_ssl_preread_module`
> option. Depending on your OS, this may not be the case. You can check with `nginx -V`. The docker image for `nginx`
> does support this though.
