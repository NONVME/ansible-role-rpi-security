# RPI Server Security Role

**Important Note**: Ensuring the security of your servers is paramount and falls solely on your shoulders. Merely applying this role and implementing a firewall does not guarantee server security. Take the time to understand Linux, network, and application security principles. Remember, there's always room to enhance the security of your entire infrastructure.

This Ansible role focuses on fortifying Raspberry pi servers, regardless of distribution, with essential security configurations. It aims to:

- Implement software for monitoring unauthorized SSH access (fail2ban).
- Enhance SSH security by disabling root login, enforcing key-based authentication, and facilitating custom SSH port configuration.
- Establish automatic updates if configured.

In addition to these measures, consider other actions to bolster server security:

- Employing logwatch or a centralized logging server for log file analysis.
- Ensuring secure user account and SSH key configurations (avoiding password authentication and root logins).
- Setting up a robust firewall (e.g., explore available roles on Ansible Galaxy).

Remember: The security of your servers is your responsibility.

## Prerequisites

Ensure that `sudo` is installed for managing the sudoers file with this role.
No special requirements for any specific Linux distribution.

## Configuration Details

Below are the configurable variables along with their default values (refer to `defaults/main.yml`):

### SSH Configuration:

- `security_ssh_port`: Specifies the port through which SSH is accessible. Default is port 22.
- `security_ssh_password_authentication`: Controls whether password authentication is allowed for SSH. Default is `"no"`.
- `security_ssh_permit_root_login`: Controls whether root login via SSH is permitted. Default is `"no"`.
- `security_ssh_usedns`: Controls whether DNS resolution is used for client IP addresses. Default is `"no"`.
- `security_ssh_permit_empty_password`: Controls whether empty passwords are permitted for SSH. Default is `"no"`.
- `security_ssh_challenge_response_auth`: Controls whether challenge-response authentication is allowed for SSH. Default is `"no"`.
- `security_ssh_gss_api_authentication`: Controls whether GSSAPI-based authentication is allowed for SSH. Default is `"no"`.
- `security_ssh_x11_forwarding`: Controls whether X11 forwarding is allowed for SSH. Default is `"no"`.

### SSH User and Group Access:

- `security_ssh_allowed_users`: A list of users allowed to connect to the host over SSH. Default is an empty list.
- `security_ssh_allowed_groups`: A list of groups allowed to connect to the host over SSH. Default is an empty list.

### SSH Daemon State:

- `security_sshd_state`: Specifies the state of the SSH daemon. Typically, this should remain `started`. Default is `started`.
- `security_ssh_restart_handler_state`: Specifies the state of the handler to restart SSH. Typically, this should remain `restarted`. Default is `restarted`.

### Sudoers Configuration:

- `security_sudoers_passwordless`: A list of users who should be added to the sudoers file to run any command as root without a password. Default is an empty list.
- `security_sudoers_passworded`: A list of users who should be added to the sudoers file to run any command as root, requiring a password for each command. Default is an empty list.

### Automatic Updates:

- `security_autoupdate_enabled`: Specifies whether to install/enable automatic updates. Default is `true`.
- `security_autoupdate_blacklist`: A list of packages that should not be automatically updated. Default is an empty list.
- `security_autoupdate_reboot`: Specifies whether to reboot when needed during unattended upgrades. Default is `false`.
- `security_autoupdate_reboot_time`: Specifies the time to trigger a reboot, when needed, if `security_autoupdate_reboot` is set to `true`. Default is `"03:00"` in 24-hour clock format.
- `security_autoupdate_mail_to`: Specifies the email address to which unattended upgrades will send an email when some error occurs. Default is an empty string.
- `security_autoupdate_mail_on_error`: Specifies whether to send an email after every package install if an error occurs. Default is `true`.

### Fail2Ban Configuration:

- `security_fail2ban_enabled`: Specifies whether to install/enable fail2ban. Default is `true`.
- `security_fail2ban_custom_configuration_template`: Specifies the name of the template file used to generate fail2ban's configuration. Default is `"jail.local.j2"`.

These variables provide flexibility in configuring various aspects of server security.

## Usage Example

```yaml
- hosts: servers
  vars_files:
    - vars/main.yml
  roles:
    - ansible-role-security
```

*Inside `vars/main.yml`*:

```yaml
security_sudoers_passworded:
  - username
  - username2
```

## About the Author

This role is a community contribution.
