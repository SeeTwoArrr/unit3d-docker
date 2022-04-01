# UNIT3D in Docker (WIP)

To start, clone the repository and run `docker-compose up -d`.

## Customization
The included [`.env`](.env) file can be used for some customization.


| Variable name             | Meaning                                                                                                                                                                                                 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `php_fpm_version`         | Tag for the [php](https://hub.docker.com/_/php) base image used for the UNIT3D image.                                                                                                                   |
| `nginx_version`           | Tag for the [nginx](https://hub.docker.com/_/nginx) base image used for the proxy image.                                                                                                                |
| `unit3d_version`          | UNIT3D version to use. It will be downloaded from [UNIT3D's Github repository](https://github.com/HDInnovations/UNIT3D-Community-Edition).                                                              |
| `fpm_healthcheck_version` | The version of the php_fpm_healthcheck application used to verify the UNIT3D container is alive. It will be downloaded from the [Github repository](https://github.com/renatomefi/php-fpm-healthcheck). |
| `proxy_listen_address`    | The host and port the proxy server will listen on. Only this exposed to the host from within the Docker network.                                                                                        |

## TODO
* Create configuration for proxy
* Create separate Redis server