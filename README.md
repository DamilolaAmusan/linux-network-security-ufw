# Linux Network Security and Firewall Management with UFW

## 1. Project Overview

This project involved configuring a Linux application server and securing incoming network traffic using UFW (Uncomplicated Firewall).

The objective was to allow administrators to manage the server through SSH, allow employees to access the hosted application through HTTP, and block unauthorised incoming connections.

## 2. Business Problem

A business needs to host an application on a Linux server. Administrators require SSH access to manage the server, while employees need HTTP access to use the application.

The server must restrict unnecessary incoming connections to reduce unauthorised access.

## 3. Infrastructure Architecture

The project used three Ubuntu Linux nodes in a KodeKloud multi-node environment.

| Node | Role | Purpose |
|---|---|---|
| Node 01 | Application server | Runs Apache and hosts the application |
| Node 02 | Administrator client | Connects to Node 01 through SSH |
| Node 03 | Employee client | Accesses the application through HTTP |

## 4. Technologies Used

- Ubuntu Linux 24.04
- Apache HTTP Server
- UFW (Uncomplicated Firewall)
- iptables (for inspecting underlying firewall rules)
- SSH
- curl
- Linux networking utilities

## 5. Network Inspection

Before configuring the firewall, the following were inspected:

- Network interfaces and IP addresses.
- Routing tables and network routes.
- Listening ports and services.
- Apache service availability.

Apache was configured to listen on TCP port 80, while SSH used TCP port 22.

## 6. Firewall Security Policy

A default-deny incoming policy was implemented.

| Traffic | Policy |
|---|---|
| Incoming SSH from Node 02 | Allow |
| Incoming SSH from other addresses | Deny |
| Incoming HTTP on port 80 | Allow |
| Other incoming traffic | Deny by default |
| Outgoing traffic | Allow |

The firewall rules were configured using UFW.

## 7. Testing and Validation

The following connectivity tests were performed:

| Test | Expected Result | Actual Result |
|---|---|---|
| Node 02 to Node 01 via SSH | Allow | Successful |
| Node 02 to Node 01 via HTTP | Allow | Successful |
| Node 03 to Node 01 via HTTP | Allow | Successful |
| Node 03 to Node 01 via SSH | Deny | Connection timed out |

The tests confirmed that the firewall enforced the intended access restrictions.

## 8. Troubleshooting

### UFW was not initially installed

The firewall commands could not be used because UFW was absent. The issue was identified and resolved by installing UFW.

### Apache was not running

HTTP connectivity tests initially failed because Apache had not been started after the lab was restarted.

The service status was checked, Apache was started, and the HTTP connectivity tests were repeated successfully.

These incidents demonstrated the importance of checking service availability before assuming that a failed connection is caused by firewall restrictions.

## 9. Inspecting the Underlying Firewall Rules

The command `iptables -L -n -v` was used to inspect the rules created by UFW.

The inspection revealed:

- The INPUT chain had a DROP default policy.
- The OUTPUT chain had an ACCEPT default policy.
- UFW created dedicated chains for organising firewall rules.
- The custom SSH and HTTP rules appeared in the `ufw-user-input` chain.
- Packet counters showed traffic matching firewall rules and packets being dropped.

This provided an introduction to how UFW uses lower-level packet-filtering infrastructure.

## 10. Security Limitations

HTTP was allowed from anywhere, meaning the application was not restricted to employees.

In a production environment, employee-only access would require stronger access controls, such as VPN access, private networking, authentication or IP-based restrictions.

HTTPS should also be used instead of unencrypted HTTP for production application traffic.

## 11. Skills Practised

- Linux network inspection.
- IP addressing and routing.
- Service management with systemd.
- Apache installation and configuration.
- SSH connectivity.
- Firewall policy design.
- UFW installation and configuration.
- Default-deny firewall policies.
- Network connectivity testing.
- Basic iptables inspection.
- Troubleshooting network access failures.

## 12. Future Improvements

- Implement firewall rules directly using iptables.
- Restrict application access to authorised employees.
- Configure HTTPS.
- Automate firewall configuration.
- Improve firewall logging and monitoring.
- Explore persistent firewall rules across server restarts.
