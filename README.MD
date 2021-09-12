# Keycloak  - docker and docker-compose

This is `docker-compose` setup for [Keycloak](http://www.keycloak.org/) server configured with [postgres database](https://www.postgresql.org/),  with [nginx https termination](https://www.nginx.com/) and [lightweight mail server](https://mailcatcher.me/).


## Used docker images

 * [openfact/keycloak-postgres](https://hub.docker.com/r/openfact/keycloak-postgres)
 * [postgres, 9.5](https://hub.docker.com/_/postgres/)
 * [nginx configuration for https termination](https://github.com/anvilresearch/nginx), borrowed from anvilreserach and customized for Keycloak
 * [mailcatcher](https://hub.docker.com/r/schickling/mailcatcher/)

## Usage

 * Clone this repository and run `docker-compose up`
 * In separate shell, run `./add-cert-to-java-truststore.sh`. Fix script for your local java setup, idea is to put custom (self-signed) certificate into java `cacerts`
 * Add to your `/etc/hosts` file record for `identity.keycloak.tom` referencing `127.0.0.1`
   * `127.0.0.1	identity.keycloak.tom`


## Testing

 * To use nignx as docker uncomment the lines of nignx on docker-compose.yml
 * For this installation we will use nginx service (cp nginx keyclock.conf to nginx folder)

## Admin account
 * default admin account added to Keycloak is:
   * Username: **admin**
   * Password: **password**


### Modifications on Keycloak configuration

 * in `standalone.xml`, I've modified 1 line (To use the SSL with nignx):
   * line 410: ```<http-listener name="default" socket-binding="http" redirect-socket="https" proxy-address-forwarding="true"/>```
     * This modification tells Keycloak to pull the client’s IP address from the X-Forwarded-For header since it's behind nginx.



### Tested on
 * `cat /etc/os-release`:
    ```
    NAME="Ubuntu"
    VERSION="18.04.5 LTS (Bionic Beaver)"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 18.04.5 LTS"
    VERSION_ID="18.04"
    HOME_URL="https://www.ubuntu.com/"
    SUPPORT_URL="https://help.ubuntu.com/"
    BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
    PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
    VERSION_CODENAME=bionic
    UBUNTU_CODENAME=bionic
    ```
 * `uname -a`: Linux ns3077990 4.15.0-154-generic #161-Ubuntu SMP Fri Jul 30 13:04:17 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
 * `docker --version`: Docker version 20.10.8, build 3967b7d
 * `docker-compose --version`: docker-compose version 1.25.4, build 8d51620a

