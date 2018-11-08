Ansible Role: ssl-ca-cert
=========

This role simplifies generating SSL certficates and signing them with a Certificate Authority (CA) certificate. 

The following files will be generated:

1. a private key,
2. a certificate signing request (CSR),
3. a public certificate,
4. a PKCS#12 certificate containing the provided CA certificate, `1.` and `2.`.


Requirements
------------

* `ansible` >= 2.7
* `python3`
* `python3-openssl`

Obviously, a CA certificate and the corresponding private key are required. Put them in the `files/` folder and override the role default variables accordingly.

Note that only Debian stretch is "officially" supported. Support for other systems could be integrated, but that would require some minor changes in the role. PRs are welcome, though.

Role Variables
--------------

Please refer to the `defaults/main.yml` file.

Dependencies
------------

There are no dependencies to other roles. Everything you need is provided by the packages listed under the __Requirements__ section.

Example Playbook
----------------

You can include this role in any playbook, however security best practises indicate that you should probably run this locally, e.g. by using `import_role` and `delegate_to: 127.0.0.1`. You could then copy the resulting certificate to the remote host via the `copy` module. 

If you do that, you should also override the default variables, otherwise the certificate will be issued to `localhost`, which is probably not what you want. 

Following is an example implementation:
```
- hosts: webservers
  gather_facts: true
  pre_tasks:
    - name: Generating SSL certificate for host.
      import_role:
        name: ssl-ca-cert
      vars:
        server_fqdn: "{{ ansible_fqdn }}"
        server_domain: "{{ ansible_domain }}"
        server_ip: "{{ ansible_default_ipv4.address }}"
      delegate_to: 127.0.0.1

  roles:
# [...]
```

License
-------
GNU General Public License Version 2
