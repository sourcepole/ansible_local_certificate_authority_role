local_certificate_authority
===========================

This role implements a local, in ansible authority, such
that:

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
      roles:
         - role: tpo.local_certificate_authority/ca
           ca_dir: files/example_org_CA
           ca_name: "Example Org CA"
           ca_subj: "/C=CH/ST=GR/L=Maladers/O=Example Org/CN=Example Org CA"
           # Root CA certificate should be valid for 10 years
           ca_days: 3650

    - hosts: localhost
      roles:
         - role: tpo.local_certificate_authority/server_cert
           ca_dir: files/example_org_CA
           server_name: server.example.org
           server_subj: "/C=CH/ST=GR/L=Maladers/O=Example Org/CN=server.example.org"
           # Server certificate should be valid for 10 years
           server_days: 3650

License
-------

BSD

Author Information
------------------

Tomáš Pospíšek <tpo_deb@sourcepole.ch>, http://sourcepole.ch
