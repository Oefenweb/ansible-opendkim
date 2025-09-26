## opendkim

[![CI](https://github.com/Oefenweb/ansible-opendkim/workflows/CI/badge.svg)](https://github.com/Oefenweb/ansible-opendkim/actions?query=workflow%3ACI)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-opendkim-blue.svg)](https://galaxy.ansible.com/Oefenweb/opendkim)

Set up OpenDKIM in Ubuntu systems.

#### Requirements

None

#### Variables

 * `opendkim_options` [default: `''`]: Extra command line options for opendkim

## Dependencies

None

#### Example

```yaml
---
- hosts: all
  roles:
    - oefenweb.opendkim
```

#### License

MIT

#### Author Information

Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-opendkim/issues)!
