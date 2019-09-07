# Syncrify-Server

Ansible role to install and configure a syncrify server.

## Requirements

An apt based system

## Role Variables

| Name                                              | Required/Default                            | Description                                  |
|---------------------------------------------------|:-------------------------------------------:|----------------------------------------------|
| `syncrify_server_admin_email`                     | :heavy_check_mark:                          |                                              |
| `syncrify_server_preferred_host_name_for_clients` | :heavy_check_mark:                          |                                              |
| `syncrify_server_smtp_security`                   | `None`                                      | TLS/SSL/None                                 |
| `syncrify_server_smtp_port`                       | `587`                                       | Smtp port                                    |
| `syncrify_server_smtp_server`                     | `null`                                      | Url of the smtp server                       |
| `syncrify_server_smtp_user`                       | `null`                                      | Smtp username                                |
| `syncrify_server_smtp_password`                   | `null`                                      | Password for the smtp user                   |
| `syncrify_server_use_web_smtp`                    | `false`                                     | Enable web smtp                              |
| `syncrify_server_email_server_web_service_host`   | `null`                                      | Web email service host url                   |
| `syncrify_server_email_server_web_service_port`   | `null`                                      | Port for web email                           |
| `syncrify_server_use_rdbms_for_delete_retension`  | `true`                                      | Enable or disable rdbms retension            |
| `syncrify_server_jvm_path`                        | `jre/bin/java`                              | Path where the java binary is located        |
| `syncrify_server_http_ip`                         | `127.0.0.1`                                 | Ip address to bind to                        |
| `syncrify_server_http_port`                       | `0`                                         | Http port to listen on (0 disables the port) |
| `syncrify_server_http_port2`                      | `5800`                                      | Http port to listen on                       |
| `syncrify_server_http_port_ssl`                   | `-1`                                        | SSL Port -1 disables the port                |
| `syncrify_server_vm_params`                       | `-Xmx512m -DLoggingConfigFile=logconfig.xm` | Java VM parameters                           |
| `syncrify_server_synametrics_url`                 | `http://synametrics.com/SynametricsWebApp/` | Synametrics URl                              |
| `syncrify_server_last_selected_tab`               | `1`                                         | Which tab was last selected                  |
| `syncrify_server_image_path`                      | `images/`                                   | Images path                                  |
| `syncrify_server_block_rdbms_to_handlers`         | `true`                                      | Block or allow handlers to rdbms             |
| `syncrify_server_default_operation`               | `door`                                      | Default operation                            |
| `syncrify_server_ransomware_file_name`            | `6LUtvYHu.jpg`                              | Filename for ransomeware check               |
| `syncrify_server_failure_over_http_port`          | `54222`                                     | Failover http port                           |
| `syncrify_server_disable_csrf_prevention`         | `true`                                      | Disable or enable csrf prevention            |
| `syncrify_server_cd`                              | `1565091689911`                             |                                              |
| `syncrify_server_nt_service_command`              | `systemctl start syncrify.service`          | The nt service command                       |
| `syncrify_server_mimic_html_files`                | `false`                                     | Mimic html files                             |
| `syncrify_server_user`                            | `syncrify`                                  | User under which to run the syncrify server  |
| `syncrify_server_group`                           | `syncrify`                                  | Group under which to run the syncrify server |


## Example

```yml
syncrify_server_admin_email: admin@example.net
syncrify_server_preferred_host_name_for_clients: backup.example.de
syncrify_server_smtp_security: TLS
syncrify_server_smtp_server: mail.example.de
syncrify_server_smtp_user: admin
syncrify_server_smtp_password: password
nginx_upstreams:
  - name: backend
    path: 127.0.0.1:5800

served_domains:
  - domains:
      - backup.example.de.
    default_server: true
    client_max_body_size: 50m
    crypto: true
    https: true
    locations:
      - condition: ~* ^/
        content: |
          root /opt/Syncrify/htdocs/webapps/ROOT;
          try_files $uri $uri/ @backend;
      - condition: "@backend"
        content: |
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto https;
          proxy_set_header Host $http_host;
          proxy_pass http://backend;
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

- [Fritz Otlinghaus (Scriptkiddi)](https://github.com/Scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
