# Ansible Role: fail2ban

An ansible role to install and configure fail2ban.

## Requirements

> This role has been tested on `Ubuntu 16.04` and `Ubuntu 16.10` only.

## Variables

- `fail2ban_loglevel`: which log level should it be output as.
  - Default: `3`
  - Options:
    - `1` = ERROR
    - `2` = WARN
    - `3` = INFO
    - `4` = DEBUG

- `fail2ban_logtarget`: where should log outputs be sent.
  - Default: `/var/log/fail2ban.log`
  - Options:
    - `SYSLOG`
    - `STDERR`
    - `STDOUT`
    - `filepath`

- `fail2ban_socket`: here should the socket be created.
  - Default: `/var/run/fail2ban/fail2ban.sock`

- `fail2ban_ignoreip`: which IP address, CIDR mark or DNS host should be ignored.
  - Default: `127.0.0.1/8`

- `fail2ban_bantime`: how long (seconds) should the ban last for
  - Default: `600`

- `fail2ban_maxretry`: default number of failed attempts before being banned.
  - Default: `4`

- `fail2ban_backend`: how should the file changes be detected
  - Default: `polling`
  - Options:
    - `gamin`
    - `polling`
    - `auto`

- `fail2ban_destemail`: where should e-mail reports be sent
  - Default: `root`

- `fail2ban_banaction`: default method of how the ban should be applied
  - Default: `iptables-multiport`
  - Options:
    - `iptables`
    - `iptables-new`
    - `iptables-multiport`
    - `shorewall`

- `fail2ban_mta`: what e-mail action should be used
  - Default: `sendmail`
  - Options:
    - `sendmail`
    - `mail`

- `fail2ban_protocol`: what should the default protocol be
  - Default: `tcp`

- `fail2ban_chain: which chain would the JUMPs be added into iptables-*
  - Default: `INPUT`

- `fail2ban_action`: default action fail2ban takes when it wants to institute a ban
  - Default: `action_mw`
  - Options:
    - `action_`: configure iptables to reject traffic
    - `action_mw`: send an email in addition to rejection
    - `action_mwl`: include relevant log lines in the email

- `fail2ban_services`: list of services fail2ban should monitor
  - Default:
    ```yaml
      - name: ssh
        port: ssh
        filter: sshd
        logpath: /var/log/auth.log
    ```

### Service Definition

Each service in the list should conform to the following definition.

- `name`: the name of the service.
  - **Required**

- `enabled`: enable monitoring this service. must be a string.
  - Default: `true`

- `port`: port used by the service. Separate multiple ports with comma. No spaces.
  - **Required**

- `protocol`: protocol used by the service.
  - Default: `tcp`

- `filter`: filter that will be used to decide whether a line in a log indicates a failed authentication.
  - **Required**

- `logpath`: where the logs for the service are located.
  - **Required**

- `maxretry`: number of faliled attempts before being banned.
  - Default: the value defined in `fail2ban_maxretry`

- `action`: action fail2ban takes when it wants to institute a ban
  - Default: the value defined in `fail2ban_action`
  - Options:
    - `action_`: configure iptables to reject traffic
    - `action_mw`: send an email in addition to rejection
    - `action_mwl`: include relevant log lines in the email

- `banaction`: method of how the ban should be applied
  - Default: the value defined in `fail2ban_banaction`
  - Options:
    - `iptables`
    - `iptables-new`
    - `iptables-multiport`
    - `shorewall`

## Usage Example

```yaml
- hosts: all
  vars:
    fail2ban_services:
      - name: ssh
        port: ssh
        filter: sshd
        logpath: /var/log/auth.log
      - name: postfix
        port: smtp,ssmtp
        filter: postfix
        logpath: /var/log/mail.log
  roles:
    - thedumbtechguy.fail2ban
```


## License

MIT / BSD

## Author Information

This role was created by [TheDumbTechGuy](https://github.com/thedumbtechguy) ( [twitter](https://twitter.com/frostymarvelous) | [blog](https://thedumbtechguy.blogspot.com) | [galaxy](https://galaxy.ansible.com/thedumbtechguy/) )

## Credits

This role was built upon the original work of:

- [nickjj/ansible-fail2ban](https://github.com/nickjj/ansible-fail2ban)

