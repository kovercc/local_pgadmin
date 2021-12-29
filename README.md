# Local pgAdmin

[pgAdmin](https://www.pgadmin.org/) is a free open source graphical management tool for PostgreSQL. This is the configuration and usage guide for running pgAdmin on the local computer in docker under [local Traefik](https://github.com/kovercc/local_traefik) with self-signed SSL certificates.

## Requirements

Before starting, you need configure and run [local Traefik](https://github.com/kovercc/local_traefik). PgAdmin is just one of the clients of local Traefik.

## Configure environment

In this guide, the local domain name of pgAdmin application is `pgadmin.local`. You can use another domain name, but you need to use it in all steps instead of `pgadmin.local` and change it in the host rule of the traefik-router in `docker-compose.yml` in this repository.

### Configuration on the operation system side

Add the domain name of service into the `hosts` file (use ip of the localhost):

```text
127.0.0.1 pgadmin.local
```

### Configuration on the Traefik side

- Go to the folder `certs` in the shell and generate a self-signed certificate for the domain name with mkcert:

```sh
mkcert --cert-file pgadmin.local.crt --key-file pgadmin.local.key pgadmin.local
```

- Add generated certificate's and key's paths to the dynamic configuration file `tls_certificates.local.yml` in the `conf` folder:

```yml
        ...        
        -   certFile: "/etc/traefik/certs/pgadmin.local.crt"
            keyFile: "/etc/traefik/certs/pgadmin.local.key"
```

**You don't need to restart local_traefik container after these steps**, the configuration applies dynamically.

## Run the local pgAdmin

After finishing with the configuration steps open root folder of the local_pgadmin repository in the shell and run the command

```sh
docker-compose up --build -d
```

After that, open `https://pgadmin.local` in browser to see pGadmin application.
