# Deploy 3Proxy and OpenVPN servers
## 3Proxy
### Description
Deploy tiny proxy server [3Proxy by Vladimir '3APA3A' Dubrovin](https://github.com/3proxy/3proxy) using [3Proxy Ansible Role](https://github.com/spector517/3proxy-ansible-role).
### Build parameters
| Name               | Default value | Description                                                                               | Required |
|--------------------|---------------|-------------------------------------------------------------------------------------------|----------|
| REMOTE_SERVER      | None          | Target server for deploy "3Proxy" (DNS or IPv4) (ip or name)                              | Yes      |
| REMOTE_SERVER_PORT | 22            | Target server ssh port                                                                    | No       |
| REMOTE_USER        | root          | Target server root user                                                                   | No       |
| REMOTE_PASSWORD    | None          | Target server root password                                                               | Yes      |
| CHECK_MODE         | No            | Run ansible playbook dry run (without changes on remote server)                           | No       |
| VERSION            | 0.9.4         | Distrib version of the "3Proxy" proxy server                                              | No       |
| HTTP_PORT          | 3128          | HTTP-proxy port of "3Proxy"                                                               | No       |
| SOCKS_PORT         | 1080          | SOCKSv4/SOCKSv5 port of "3Proxy"                                                          | No       |
| SYSTEM_GROUP_ID    | 200           | System group id on remotes                                                                | No       |
| SYSTEM_USER_ID     | 201           | System user id on remotes                                                                 | No       |
| DNS                | 1.1.1.1       | Comma separated list of DNS used by "3Proxy". For example: 8.8.4.4,8.8.8.8                | No       |
| USE_UFW            | Yes           | Use the Uncomplicated Firewall (or you may use other firewall)                            | No       |
| CLIENTS            | None          | Comma separated list of clients "3Proxy" server. For example: user1:pass1,user2:pass2     | No       |
| BUILD_ID           | No ID         | Build identifier (technical parameter)                                                    | No       |
**_NOTE:_**  All required parameters has no default value and required to fill out
### Stages
#### Initialization
1. Print the running parameters
2. Check if parameters REMOTE_SERVER and REMOTE_PASSWORD are not empty
#### Generate ansible files
1. Generate Ansible vars file using build parameters: VERSION, HTTP_PORT, SOCKS_PORT, SYSTEM_GROUP_ID, SYSTEM_USER_ID, DNS, USE_UFW, CLIENTS
2. Generate Ansible inventory file  using build parameters: REMOTE_SERVER, REMOTE_USER, REMOTE_PASSWORD
3. Generate Ansible playbook
#### Deploy 3Proxy server
1. Run Ansible playbook
#### Run tests
1. Get target servers ip
2. Send request to https://api.ipify.org via deployed HTTP-proxy
3. Send request to https://api.ipify.org via deployed SOCKSv5-proxy
4. Ensure target server ip is obtained ip from requests above
## OpenVPN
### Description
Deploy OpenVPN server using [OpenVPN Ansible role](https://github.com/spector517/openvpn-ansible-role) and generate clients configs.
The clients configs will be attached to the Jenkins build page.
### Build parameters
| Name               | Default value | Description                                                                               | Required |
|--------------------|---------------|-------------------------------------------------------------------------------------------|----------|
| REMOTE_SERVER      | None          | Target server for deploy OpenVPN server (DNS or IPv4)                                     | Yes      |
| REMOTE_SERVER_PORT | 22            | Target server ssh port                                                                    | No       |
| REMOTE_USER        | root          | Target server root user                                                                   | No       |
| REMOTE_PASSWORD    | None          | Target server root password                                                               | Yes      |
| CHECK_MODE         | No            | Run ansible playbook dry run (without changes on remote server)                           | No       |
| OPENVPN_PORT       | 1194          | OpenVPN server port                                                                       | No       |
| OPENVPN_PROTOCOL   | udp           | OpenVPN server protocol (UDP or TCP)                                                      | No       |
| DNS                | 1.1.1.1       | Comma separated list of DNS used by OpenVPN server. For example: 8.8.4.4,8.8.8.8          | No       |
| CLIENTS            | client        | Comma separated list of OpenVPN servers clients names. For example: client1,client2       | No       |
| RESET_EASY_RSA     | No            | Delete currently existing Easy RSA directory                                              | No       |
| USE_UFW            | Yes           | Use the Uncomplicated Firewall (or you may use other firewall)                            | No       |
| BUILD_ID           | No ID         | Build identifier (technical parameter)                                                    | No       |
**_NOTE:_**  All required parameters has no default value and required to fill out
### Stages
#### Initialization
1. Print the running parameters
2. Check if parameters REMOTE_SERVER and REMOTE_PASSWORD are not empty
#### Pull required files
1. Clone this repository
#### Generate ansible files
1. Generate Ansible vars file using build parameters: REMOTE_SERVER, OPENVPN_PORT, OPENVPN_PROTOCOL, USE_UFW, DNS, CLIENTS, RESET_EASY_RSA, REMOTE_SERVER
2. Generate Ansible inventory file  using build parameters: REMOTE_SERVER, REMOTE_USER, REMOTE_PASSWORD
3. Generate Ansible playbook
#### Deploy OpenVPN server
1. Run Ansible playbook
#### Run test
1. Get target servers ip
2. Find random client config file
3. Build docker image based on Alpine Linux with OpenVPN and cURL packages
4. Run OpenVPN with random client config file in docker container
5. Send request form docker container to https://api.ipify.org and get ip
6. Ensure target server ip is equal obtained ip from request above
#### Process results
1. Archive the clients configs