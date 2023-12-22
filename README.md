# RADIUS TACACS+

## Enabling AAA and Configuring a Local User for Fallback

Enable AAA services in global configuration mode.
```
Switch(config)# aaa new-model
```
Configure a local username.
```
Switch(config)# username admin secret adminpass
```
# RADIUS
## Configuring RADIUS for Console and vty Access

Configure the RADIUS server.
```
Router(config)# radius server RADSRV
Router(config-radius-server)# address ipv4 10.255.255.101 auth-port 1812 acct-port 1813
Router(config-radius-server)# key SecretRAD
```
Associate the RADIUS server with a server group.
```
Router(config)# aaa group server radius RADSRVGROUP 
Router(config-sg-radius)# server name RADSRV
```
Configure aaa authentication login to use a server group with a fallback to local authentication.
```
Router(config)# aaa authentication login SRVAUTH group RADSRVGROUP local
```

## RADIUS Configuring the Console and vty Access

Configure the named method list to the console.
```
Router(config)# line con 0
Router(config-line)# login authentication SRVAUTH
```
Configure the named method list to the vty 0 4 lines.
```
Router(config)# line vty 0 4 
Router(config-line)# login authentication SRVAUTH
```

# TACACS+
## Configure the TACACS+ server.
```
Router(config)# tacacs server TACSRV 
Router(config-server-tacacs)# address ipv4 10.255.255.102 
Router(config-server-tacacs)# key SecretTAC
```
Associate the TACACS+ server with a server group.
```
Router(config)# aaa group server tacacs+ TACSRVGROUP 
Router(config-sg-tacacs+)# server name TACSRV
```
Configure default AAA authentication to use a server group with fallback to local authentication. As the default method, it will apply automatically to all lines.
```
Router(config)# aaa authentication login default group TACSRVGROUP local
```
(alternative) Configure aaa authentication login to use a server group with a fallback to local authentication.
```
Router(config)# aaa authentication login MYTACAUTH group TACSRVGROUP local
```
## TACACS+ Configuring the Console and vty Access

Configure the named method list to the console.
```
Router(config)# line con 0
Router(config-line)# login authentication MYTACAUTH
```
Configure the named method list to the vty 0 4 lines.
```
Router(config)# line vty 0 4 
Router(config-line)# login authentication MYTACAUTH
```
