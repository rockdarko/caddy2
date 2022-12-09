caddy2
=========

Role to deploy caddy2 

Requirements
------------
Ubuntu/Debian

Upcoming features
------------
- docker support - currently only supports native package installation.

- template expansion: caddyfile template currently supports basic features to act as reserve_proxy or url rewrite

Role Variables
--------------

caddy2_tz: timezone
caddy2_state: (present or absent)
caddy2_port_http: listen port for http connections
caddy2_port_https: listen port for tls connections
caddy2_port_api: listen port for rest api

caddy2_service_type: caddy or caddy-api (caddy-api persists changes made through api while caddy doesn't - both support simultanious api and Caddyfile configs but it's not advised to use both methods in a single setup).

logpath: directory to store logs
debug: false
http_port: 80
https_port: 443

internal_ca_certs: list of CA's to trust as filename of CA certificate that needs to be in the role's files folder and have the .crt extension.

sites: dictionary of sites (this is only if using caddyfile for config - do not use in case of caddy-api).
  - name: site name
    host: common hostname - including port
    type: reverse_proxy or rewrite (see official caddy documentation)
    internal: where to redirect request internally - including port.

Exmaple inventory
------------

caddy2_tz: 'America/Toronto'
caddy2_state: present

caddy2_port_http: 80
caddy2_port_https: 443
caddy2_port_api: 2019

logpath: /var/log/caddy
debug: false
http_port: 80
https_port: 443
admin_listener: "0.0.0.0:{{ caddy2_port_api }}"

internal_ca_certs:
 - cellardoor.crt

sites:
  - name: transmission.somesite.com
    host: transmission.somesite.com:443
    type: reverse_proxy
    internal: http://locke.internal.somesite.com:9025
  - name: jenkins.somesite.com
    host: jenkins.somesite.com:443
    type: reverse_proxy
    internal: https://rikku.internal.somesite.com:8081

Dependencies
------------

Ubuntu / Debian

Example Playbook
----------------

Just invoke role and use inventory for all paramters.

License
-------

BSD

Author Information
------------------

Rock Martel-Langlois (rock@geekreboot.ca)
