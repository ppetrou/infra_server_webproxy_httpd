infra_server_webproxy_httpd
=========

A simple role to setup an http proxy

Requirements
------------
NA

Releases
------------

|Branch|Status|Description| Date
|---	|---	|---	|---
|main|Unstable|Development Branch|Now
|1.0.0|Release|Release 1.0.0|25-09-2021

Role Variables
--------------

|Variable|Level|Type|Description
|---|---|---|---		
|listen_ip|Default|string|The listening ip of the Virtual Host for the proxy
|listen_port|Default|int|The listening port of the Virtual Host for the proxy
|ssl_enabled|Default|boolean|yes if the virtual host needs to serve secure content. no for unencrypted
|ssl_cert_autogen|Default|boolean|yes to autogenerate and self-sign the certificate for the virtual host
|ssl_proxy|Default|boolean|yes if the target url is secured
|ssl_proxy_verify|Default|boolean|yes to verify the certificate of the target url
|ssl_proxy_check_peer_name|Default|boolean|yes to validate the proxy server name in the target server certificate subject or altnames.
|ssl_proxy_ca_certificate_file|Default|string|The cert chain to validate the target server certificate
|proxy_urls|Default|list(dict)|A list of source/target urls to configure the proxy server
|ssl_cert_file_filepath|Vars|string|The certificate for the Proxy Virtual Host
|sssl_cert_key_filepath|Vars|string|The key of the certificate of the Proxy Virtual Host



```
# IP/Port Parameters
listen_ip: 192.168.122.205
listen_port: 443

# SSL Virtual Host Parameters
ssl_enabled: yes
ssl_cert_autogen: yes

# SSL Proxy Parameters
ssl_proxy: yes
ssl_proxy_verify: yes
ssl_proxy_check_peer_name: no
ssl_proxy_ca_certificate_file: '/etc/pki/tls/certs/chain.pem'

# Proxy URLS
proxy_urls:
  - { source: "/test", target: "https://192.168.122.123:443/test" }

ssl_cert_file_filepath: '/etc/pki/tls/certs/proxy.crt'
ssl_cert_key_filepath: '/etc/pki/tls/private/proxy.key'
```



Dependencies
------------

The following two collections are pre-requisites.

- community.crypto
- ansible.posix


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
        listen_port: 443
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
