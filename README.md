local_certificate_authority
===========================

This role implements a local, in ansible certificate authority,
such that:

* private keys and certificates reside on the local host
  from which the playbook gets executed

* private keys are encrypted and in ansible vaults

* keys and certificates can be commited to git into the
  same repository where ansible playbooks live

* key management can be done entirely locally

* keys and certificates can be simply copied to the
  hosts where they are required

Once created, both keys and certificates will **not**
be changed even if templates or parameters change.

Thus to create new keys/certificates, you must first remove the
existing ones.

Requirements
------------

The `openssl` binary needs to be available locally.

Role Variables
--------------

See `defaults/main.yml` for the parameters that can be set.

Dependencies
------------

None

Example Playbook
----------------

    - hosts: localhost
      vars:
        ca_dir: files/example_org_CA
        server_name: server.example.org
      roles:
         - role: tpo.local_certificate_authority/ca
           # is using playbook variables:
           # ca_dir: ...
           ca_name: "Example Org CA"
           ca_subj: "/C=CH/ST=GR/L=Maladers/O=Example Org/CN=Example Org CA"
           # Root CA certificate should be valid for 10 years
           ca_days: 3650

      roles:
         - role: tpo.local_certificate_authority/server_cert
           # is using playbook variables:
           # ca_dir: ...
           # server_name: ...
           server_subj: "/C=CH/ST=GR/L=Maladers/O=Example Org/CN={{ server_name }}"
           # server_subjectAlternativeName:
           # * optional
           # * set to DNS:server_name by default
           # Server certificate should be valid for 10 years
           server_days: 3650

      tasks:
        - name: verify generated certificates
          shell: openssl verify -CAfile {{ ca_dir }}/certs/ca.cert.pem {{ ca_dir }}/certs/{{ server_name }}.cert.pem
          changed_when: false # doesn't change anything

License
-------

BSD

Author Information
------------------

Tomáš Pospíšek <tpo_deb@sourcepole.ch>, http://sourcepole.ch
