# LDAP Auth proxy

[![Build Status](https://api.travis-ci.org/pinepain/ldap-auth-proxy.svg?branch=master)](https://travis-ci.org/pinepain/ldap-auth-proxy)
[![codecov](https://codecov.io/gh/pinepain/ldap-auth-proxy/branch/master/graph/badge.svg)](https://codecov.io/gh/pinepain/ldap-auth-proxy)
[![Go Report Card](https://goreportcard.com/badge/github.com/pinepain/ldap-auth-proxy)](https://goreportcard.com/report/github.com/pinepain/ldap-auth-proxy)

A simple drop-in HTTP proxy for transparent LDAP authorization which is also a HTTP auth backend.

## Usage

You can use `pinepain/ldap-auth-proxy` docker image (see available tags [here](https://hub.docker.com/r/pinepain/ldap-auth-proxy/tags/))
or build binary by yourself, `Dockerfile` and `.travis.yml` list all necessary steps to build it.

Usage examples could be foung in [examples](./examples) folder.

## Architecture

LDAP auth proxy could be used in two modes: as an auth backend and as a proxy:

### Auth backend

![auth backend](https://user-images.githubusercontent.com/2185793/38117476-e3a456dc-33bd-11e8-927d-ef68a9a863d7.png)

Example `docker-compose` setup could be found in [examples/auth_backend](./examples/auth_backend).

### Proxy

![proxy](https://user-images.githubusercontent.com/2185793/38117475-e384e496-33bd-11e8-9959-fbef372ea06a.png)

and it's variation, proxy behind nginx:

![proxy behind nginx](https://user-images.githubusercontent.com/2185793/38117474-e367794c-33bd-11e8-86a4-1b16d9fa6e4b.png)

Example `docker-compose` setup could be found in [examples/proxy](./examples/proxy)

## Example settings for JumpCloud users:

    export LDAP_SERVER='ldaps://ldap.jumpcloud.com'
    export LDAP_BASE='o=<oid>,dc=jumpcloud,dc=com'
    export LDAP_BIND_DN='uid=<bind user name>,ou=Users,o=<oid>,dc=jumpcloud,dc=com'
    export LDAP_BIND_PASSWORD='<bind user password>'
    export LDAP_USER_FILTER='(uid=%s)'
    export LDAP_GROUP_FILTER='(&(objectClass=groupOfNames)(member=uid=%s,ou=Users,o=<oid>,dc=jumpcloud,dc=com))'
    export GROUP_HEADER='X-Ldap-Group'
    export HEADERS_MAP='X-LDAP-Mail:mail,X-LDAP-UID:uid,X-LDAP-CN:cn,X-LDAP-DN:dn'

where `<oid>` is your organisation id.


## Notes

A zero length password is always considered invalid since it is, according to the LDAP spec, a request for
"unauthenticated authentication." Unauthenticated authentication should not be used for LDAP based authentication.
See `section 5.1.2 of RFC-4513 <http://tools.ietf.org/html/rfc4513#section-5.1.2>`_ for a description of this behavior.

Neither zero length username supported. Anonymous authentication should also not be used for LDAP based authentication.
See `section 5.1.1 of RFC-4513 <http://tools.ietf.org/html/rfc4513#section-5.1.1>`_ for a description of that behavior.


## License

[ldap-auth-proxy](https://github.com/pinepain/ldap-auth-proxy) is licensed under the [MIT license](http://opensource.org/licenses/MIT).
