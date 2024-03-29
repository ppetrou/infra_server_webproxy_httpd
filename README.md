infra_server_webproxy_httpd
=========

A simple role to setup an http forward / reverse proxy


Requirements
------------
NA


Releases
------------

|Branch|Status|Description| Date
|---	|---	|---	|---
|main|Unstable|Development Branch|Now
|1.0.1|Release|Release 1.0.1|09-12-2022
|1.0.0|Release|Release 1.0.0|25-09-2021


Role Variables
--------------

|Variable|Level|Type|Description
|---|---|---|---		
|listen_ip|Default|string|The listening ip of the httpd server
|listen_port|Default|int|The listening port of the httpd server
|forward_proxy.enabled|Default|boolean|Control flag to enable the forward proxy
|forward_proxy.listen_port|Default|int|The forward listening port of the Virtual Host for the proxy
|forward_proxy.allowed_ip|Default|int|The ips that can access the proxy. See https://httpd.apache.org/docs/current/mod/mod_authz_host.html for the correct format.
|reverse_proxy.enabled|Default|boolean|Control flag to enable the reverse proxy
|reverse_proxy.listen_port|Default|int|The reverse listening port of the Virtual Host for the proxy
|reverse_proxy.ssl_enabled|Default|boolean|yes if the virtual host needs to serve secure content. no for unencrypted
|reverse_proxy.ssl_cert_autogen|Default|boolean|yes to autogenerate and self-sign the certificate for the virtual host
|reverse_proxy.ssl_proxy|Default|boolean|yes if the target url is secured
|reverse_proxy.ssl_proxy_verify|Default|boolean|yes to verify the certificate of the target url
|reverse_proxy.ssl_proxy_check_peer_name|Default|boolean|yes to validate the proxy server name in the target server certificate subject or altnames.
|reverse_proxy.ssl_proxy_ca_certificate_file|Default|string|The cert chain to validate the target server certificate
|reverse_proxy.proxy_urls|Default|list(dict)|A list of source/target urls to configure the proxy server
|ssl_cert_file_filepath|Vars|string|The certificate for the Proxy Virtual Host
|sssl_cert_key_filepath|Vars|string|The key of the certificate of the Proxy Virtual Host


Dependencies
------------

The following collections are pre-requisites:

- community.crypto
- ansible.posix

The following python modules are pre-requisites on the target host:

- python3-policycoreutils
- python3-libselinux


Example Playbook
----------------


```
---
- hosts: http_proxy
  become: yes
  roles:
    - role: infra_server_webproxy_httpd
      vars:
        listen_ip: 192.168.122.205
        forward_proxy:
          enabled: yes
          listen_port: 8088
          allowed_ip: 192.168.122.0/24
        reverse_proxy:
          enabled: yes
          listen_port: 4433
          ssl_enabled: yes
          ssl_cert_autogen: yes
          ssl_cert_file_filepath: '/etc/pki/tls/certs/proxy.crt'
          ssl_cert_key_filepath: '/etc/pki/tls/private/proxy.key'
          ssl_proxy: yes
          ssl_proxy_verify: no
          ssl_proxy_check_peer_name: no
          ssl_proxy_ca_certificate_file: ''
          proxy_urls:
            - { source: "/hello", target: "https://192.168.122.56/ok.html" }

```


License
-------

Apache-2.0


Author Information
------------------

```
Petros Petrou
email: ppetrou@gmail.com
web: www.petrospetrou.co.uk
```
