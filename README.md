letsencrypt
=========

This role does the following:

1. Ensure you have a private key for your acme account.
2. Ensure you have a private key for your certificate.
3. Ensure you have a certificate signing request.
4. Then a challenge is requested from letsencrypt.
5. http-01 challenge is implemented.
6. The certficate from letsencrypt is requested.
7. The challenge implementation is removed.
8. The server configuration is updated with the ssl certificates.

Role Variables
--------------

Variables in `vars/main.yml`
| name                    | default                                                 | description                        |
|-------------------------|---------------------------------------------------------|------------------------------------|
| domain_name             | my.example.com                                          | **Your domain**                    |
| acme_baseurl            | acme-staging-v02.api.letsencrypt.org                    | Production: acme-v02.api.letsencrypt.org </br> Staging: acme-staging-v02.api.letsencrypt.org |
| acme_directory          | https://{{ acme_baseurl }}/directory                    |                                    |
| acme_email              | me@example.mail.com                                     | **Your acme account mail**         |
| letsencrypt_dir         | /etc/letsencrypt                                        | Certificate and key root directory |
| letsencrypt_account_key | "{{ letsencrypt_dir }}/accounts/{{ acme_baseurl }}.pem" | ACME account key                   |
| letsencrypt_privkey     | "{{ letsencrypt_dir }}/{{ domain_name }}/privkey.pem"   |                                    |
| letsencrypt_chain       | "{{ letsencrypt_dir }}/{{ domain_name }}/chain.pem"     |                                    |
| letsencrypt_fullchain   | "{{ letsencrypt_dir }}/{{ domain_name }}/fullchain.pem" |                                    |
| letsencrypt_cert        | "{{ letsencrypt_dir }}/{{ domain_name }}/cert.pem"      |                                    |
| letsencrypt_csr         | "{{ letsencrypt_dir }}/csr/{{ domain_name }}.pem"       |                                    |
| cert.common_name        | "{{ domain_name }}"                                     |                                    |
| cert.country            | "CH"                                                    | **Your country**                   |
| cert.email_address      | "{{ acme_email }}"                                      |                                    |
| cert.subject_alt_name   | ['DNS:{{ domain_name }}']                               |                                    |
| nginx_options_ssl       | "{{ letsencrypt_dir }}/options-ssl-nginx.conf"          |                                    |
| nginx_dhparams          | "{{ letsencrypt_dir }}/ssl-dhparams.pem"                |                                    |


A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Example Playbook
----------------

    - hosts: api
      gather_facts: no
      become: true
      roles:
        - letsencrypt
