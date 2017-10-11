### **Forked originally from [ventz/docker-bind](https://github.com/ventz/docker-bind)**
### **UID/GID parameter processing used from [pstauffer/docker-bind](https://github.com/pstauffer/docker-bind)**


### ISC BIND9 Container (Stable: 9.11.1_xx) built on top of Alpine
### Last update: 9-18-17

NOTE: "Last Update" is the date of the latest DockerHub build.

This container is a super small (~5MB compressed pull, and only ~9MB
when extracted) FULL version of ISC BIND9.

It is ideal for a quick master, slave, recursive server/resolver, RPZ
"dns firewall", or just about any other purpose you can use bind for.

## Security - always on the latest stable release!
This container will _always_ be up to date on the latest
stable+patched version, usually within 24 hours of it being available
in Alpine. In fact, most of the BIND vulnerabilities so far have been
reported by me to the Alpine developers.

## Docker Environment Variables
It's possible to change some variables like the uid/gid of the technical bind user/group or the executed named command. Just pass the variables via docker command to the container or define them in the docker-compose file.

* `NAMED_UID` Set the UID of the `named` user to be used for permissions access to the mounted configurations and logs directory. **Default**: 1000
* `NAMED_GID` Set the GID of the `named` user to be used for permissions access to the mounted configurations and logs directory. **Default**: 1000
* `COMMAND_OPTIONS` Override or add additional configuration flags to the end of the named command executed. **Default**: `-f`
  * For example to log to stdout instead of the logs use `COMMAND_OPTIONS=-g`




## Required "DATA" directory - for named.conf, zone data and logs:
This container assumes you have a "/DATA" folder with with your container specific data.
You can change that folder (and sub-folders) as needed, but make sure you update the "-v" mounts for the run.

Specifically, you need to have these directories/paths:

1. In your "/DATA/etc/bind" directory, a file "named.conf", which acts as an entry point to your configs

2. A "/DATA/var/named" directory for all of the master or slave zones. If it's for slave zones, it will populate automatically and you can leave it blank.

3. A "/DATA/var/log/named" directory for logging your DNS requests/returns/other breakdown. By default logging will be done in this directory so override using `COMMAND_OPTIONS=-g` if STDOUT is preferred.

**IMPORTANT**: Ensure all mounted volumes/files have the uid/gid owner set to the `NAMED_UID` and `NAMED_GID` specified for the container. **Default**: uid:gid 1000:1000.


## How to run a BIND ("named") Docker Container?

```
docker run --name=dns-master01 \
--rm --dns=8.8.8.8 --dns=8.8.4.4 \
-p 53:53/udp -p 53:53 \
-v /DATA/etc/bind:/etc/bind \
-v /DATA/var/cache/bind:/var/named \
-v /DATA/var/log/named:/var/log/named \
unifio/bind
```

## Bind Test
You can test the dns responses with `dig` or `nslookup` on the docker host.
```
dig webmail.example.com @localhost

nslookup webmail.example.com localhost
```

## License
This project is licensed under `MIT <http://opensource.org/licenses/MIT>`_.
