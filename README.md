system_ca
=========

Ansible role which helps to add/remove system CA certificates.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be
done either by changing role parameters or by declaring completely new
configuration as a variable. That makes this role absolutely
universal. See the examples below for more details.

Please report any issues or send PR.


Examples
--------

```yaml
---

- name: Deploy CA certs
  hosts: all
  become: yes
  vars:
    # List of CAs
    system_ca_certs:
      # Creates CA called 'thawte_Primary_Root_CA.crt'
      - name: thawte_Primary_Root_CA
        # The content of the CA specified as test
        content: |
          -----BEGIN CERTIFICATE-----
          MIIEIDCCAwigAwIBAgIQNE7VVyDV7exJ9C/ON9srbTANBgkqhkiG9w0BAQUFADCB
          qTELMAkGA1UEBhMCVVMxFTATBgNVBAoTDHRoYXd0ZSwgSW5jLjEoMCYGA1UECxMf
          Q2VydGlmaWNhdGlvbiBTZXJ2aWNlcyBEaXZpc2lvbjE4MDYGA1UECxMvKGMpIDIw
          MDYgdGhhd3RlLCBJbmMuIC0gRm9yIGF1dGhvcml6ZWQgdXNlIG9ubHkxHzAdBgNV
          BAMTFnRoYXd0ZSBQcmltYXJ5IFJvb3QgQ0EwHhcNMDYxMTE3MDAwMDAwWhcNMzYw
          NzE2MjM1OTU5WjCBqTELMAkGA1UEBhMCVVMxFTATBgNVBAoTDHRoYXd0ZSwgSW5j
          LjEoMCYGA1UECxMfQ2VydGlmaWNhdGlvbiBTZXJ2aWNlcyBEaXZpc2lvbjE4MDYG
          A1UECxMvKGMpIDIwMDYgdGhhd3RlLCBJbmMuIC0gRm9yIGF1dGhvcml6ZWQgdXNl
          IG9ubHkxHzAdBgNVBAMTFnRoYXd0ZSBQcmltYXJ5IFJvb3QgQ0EwggEiMA0GCSqG
          SIb3DQEBAQUAA4IBDwAwggEKAoIBAQCsoPD7gFnUnMekz52hWXMJEEUMDSxuaPFs
          W0hoSVk3/AszGcJ3f8wQLZU0HObrTQmnHNK4yZc2AreJ1CRfBsDMRJSUjQJib+ta
          3RGNKJpchJAQeg29dGYvajig4tVUROsdB58Hum/u6f1OCyn1PoSgAfGcq/gcfomk
          6KHYcWUNo1F77rzSImANuVud37r8UVsLr5iy6S7pBOhih94ryNdOwUxkHt3Ph1i6
          Sk/KaAcdHJ1KxtUvkcx8cXIcxcBn6zL9yZJclNqFwJu/U30rCfSMnZEfl2pSy94J
          NqR32HuHUETVPm4pafs5SSYeCaWAe0At6+gnhcn+Yf1+5nyXHdWdAgMBAAGjQjBA
          MA8GA1UdEwEB/wQFMAMBAf8wDgYDVR0PAQH/BAQDAgEGMB0GA1UdDgQWBBR7W0XP
          r87Lev0xkhpqtvNG61dIUDANBgkqhkiG9w0BAQUFAAOCAQEAeRHAS7ORtvzw6WfU
          DW5FvlXok9LOAz/t2iWwHVfLHjp2oEzsUHboZHIMpKnxuIvW1oeEuzLlQRHAd9mz
          YJ3rG9XRbkREqaYB7FViHXe4XI5ISXycO1cRrK1zN44veFyQaEfZYGDm/Ac9IiAX
          xPcW6cTYcvnIc3zfFi8VqT79aie2oetaupgf1eNNZAqdE8hhuvU5HIe6uL17In/2
          /qxAeeWsEG89jxt5dovEN7MhGITlNgDrYyCZuen+MwS7QcjBAvlEYyCegc5C09Y/
          LHbTY5xZ3Y+m4Q6gLkH3LpVHz7z9M/P2C2F+fpErgUfCJzDupxBdN49cOSvkBPB7
          jVaMaA==
          -----END CERTIFICATE-----
      # Creates CA called 'InternalRootCA.crt'
      - name: InternalRootCA
        # Optionally specify file permissions
        owner: sys
        group: nobody
        mode: "0640"
        # The content of the CA specified a file
        content: "{{ lookup('file', 'InternalRootCA.crt') }}"
      # Blacklist CA called 'CompanyRootCA.crt' (works only on RedHat!)
      - name: CompanyRootCA
        # Places the cert into /etc/pki/ca-trust/source/blacklist instead of /etc/pki/ca-trust/source/anchors
        subdir: blacklist
        content: "{{ lookup('file', 'CompanyRootCA.crt') }}"
      # Removes CA named 'test.crt'
      - name: test
        state: absent
  roles:
    - system_ca
```


Role variables
--------------

Variables used by the role:

```yaml
# Base dir for RedHat CAs
system_ca_redhat_dest_base: /etc/pki/ca-trust/source

# Subdirectory in the /etc/pki/ca-trust/source
# (set it to empty string to place files in /etc/pki/ca-trust/source)
system_ca_redhat_dest_subdir: anchors

# Base dir for Debian/Ubuntu CAs
system_ca_debian_dest_base: /usr/local/share/ca-certificates

# Default cert owner
system_ca_owner: root

# Default cert group
system_ca_group: root

# Defautl cert mode
system_ca_mode: "0644"

# Update command for RedHat
system_ca_redhat_update_cmd: update-ca-trust extract

# Update command for Debian/Ubuntu
system_ca_debian_update_cmd: update-ca-certificates

# List of certificates (see README for more details)
system_ca_certs: []
```


License
-------

MIT


Author
------

Jiri Tyr
