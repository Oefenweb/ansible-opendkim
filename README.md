## opendkim

[![CI](https://github.com/Oefenweb/ansible-opendkim/workflows/CI/badge.svg)](https://github.com/Oefenweb/ansible-opendkim/actions?query=workflow%3ACI)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-opendkim-blue.svg)](https://galaxy.ansible.com/Oefenweb/opendkim)

Set up OpenDKIM in Ubuntu systems.

#### Requirements

None

#### Variables

 * `opendkim_install` [default: `[]`]: Additional packages to install

 * `opendkim_rundir` [default: `/run/opendkim`]: Specifies the path to "run" directory
 * `opendkim_socket` [default: `inet:8891@localhost`]: Specifies the socket that should be established by the filter to receive connections from `sendmail(8)` in order to provide service
 * `opendkim_user` [default: `opendkim`]: Attempts to become the specified userid before starting operations
 * `opendkim_group` [default: `opendkim`]: Attempts to become the specified groupid before starting operations
 * `opendkim_pidfile` [default: `"{{ opendkim_rundir }}/opendkim.pid"`]: Specifies the path to a file that should be created at process start containing the process ID
 * `opendkim_syslog` [default: `true`]: Log via calls to `syslog(3)` any interesting activity
 * `opendkim_syslog_success` [default: `true`]: Log via calls to `syslog(3)` additional entries indicating successful signing or verification of messages
 * `opendkim_log_why` [default: `false`]: If logging is enabled (see `Syslog` below), issues very detailed logging about the logic behind the filter’s decision to either sign a message or verify it
 * `opendkim_umask` [default: `'007'`]: Requests a specific permissions mask to be used for file creation
 * `opendkim_canonicalization` [default: `relaxed/simple`]: Selects the canonicalization method(s) to be used when signing messages
 * `opendkim_mode` [default: `sv`]: Selects operating modes
 * `opendkim_sub_domains` [default: `false`]: Sign subdomains of those listed by the `Domain` parameter as well as the actual domains
 * `opendkim_trust_anchor_file` [default: `/usr/share/dns/root.key`]: Specifies a file from which trust anchor data should be read when doing DNS queries and applying the DNSSEC protocol
 * `opendkim_oversign_headers` [default: `From`]: Specifies a set of header fields that should be included in all signature header lists (the "h=" tag) once more than the number of times they were actually present in the signed message

 * `opendkim_opendkim_conf` [default: `"{{ opendkim_preset_opendkim_conf }}"`, see the OS specific presets in `vars`]:
 * `opendkim_default_opendkim` [default: `"{{ opendkim_preset_default_opendkim }}"`, see the OS specific presets in `vars`]:

* `opendkim_key_map`: [default: `[]`]: Key declarations
* `opendkim_key_map.{n}.src`: [required, if `content` is not set]: The path of the file to copy
* `opendkim_key_map.{n}.remote_src`: [optional]: Whether the `src` is on the remote
* `opendkim_key_map.{n}.content`: [required, if `src` is not set]: The content to copy
* `opendkim_key_map.{n}.dest`: [default `src`]: The remote path of the file to copy
* `opendkim_key_map.{n}.owner`: [default `{{ opendkim_user }}`]: The name of the user that should own the file
* `opendkim_key_map.{n}.group`: [default `{{ opendkim_group }}`]: The name of the group that should own the file
* `opendkim_key_map.{n}.mode`: [default `0600`]: The mode of the file to copy

## Dependencies

None

#### Example

##### Simple

```yaml
---
- hosts: all
  roles:
    - oefenweb.opendkim
```

##### Advance

```yaml
- hosts: all
  roles:
    - oefenweb.opendkim
  vars:
    opendkim_opendkim_conf:
      - |
        Syslog {{ opendkim_syslog | bool | ternary('yes', 'no') }}
        UMask {{ opendkim_umask }}
        Domain {{ opendkim_key_file_domain }}
        KeyFile {{ opendkim_key_file_remote }}
        Selector {{ opendkim_key_file_selector }}
        Canonicalization {{ opendkim_canonicalization }}
        Mode {{ opendkim_mode }}
        SubDomains {{ opendkim_sub_domains | bool | ternary('yes', 'no') }}
        Socket {{ opendkim_socket }}
        PidFile {{ opendkim_pidfile }}
        OversignHeaders {{ opendkim_oversign_headers }}
        TrustAnchorFile {{ opendkim_trust_anchor_file }}
        UserID {{ opendkim_user }}

    opendkim_default_opendkim:
      - |
        RUNDIR={{ opendkim_rundir }}
        SOCKET="{{ opendkim_socket }}"
        USER={{ opendkim_user }}
        GROUP={{ opendkim_group }}
        PIDFILE={{ opendkim_pidfile }}
        EXTRAAFTER=

    opendkim_key_map:
      - src: "{{ opendkim_key_file_local }}"
        dest: "{{ opendkim_key_file_remote }}"

    opendkim_key_file_domain: example.com
    opendkim_key_file_selector: 2025
    opendkim_key_file_local: "{{ playbook_dir }}/files/opendkim/{{ opendkim_key_file_remote.lstrip('/') }}"
    opendkim_key_file_remote: "/etc/dkimkeys/{{ opendkim_key_file_domain }}/{{ opendkim_key_file_selector }}.private"
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-opendkim/issues)!
