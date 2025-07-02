# Configure the management interface
```
<HUAWEI> system-view
[~HUAWEI] user-interface maximum-vty 21
[~HUAWEI] user-interface vty 0 20
[*HUAWEI-hi-vty0-20] idle-timeout 0
[*HUAWEI-ui-vty0-20] authentication-mode aaa
[*HUAWEI-ui-vty0-20] protocol inbound all
[*HUAWEI-ui-vty0-20] commit
[~HUAWEI-ui-vty0-20] quit

[~HUAWEI] interface meth 0/0/0
[*HUAWEI-MEth0/0/0] ip address 192.168.30.1 24
[*HUAWEI-MEth0/0/0] commit
[~HUAWEI-MEth0/0/0] quit
```

# Configure SSH for login
```
[~HUAWEI] aaa
[~HUAWEI-aaa] undo local-user policy security-enhance
[*HUAWEI-aaa] commit
[~HUAWEI-aaa] local-user admin password irreversible-cipher admin2025
[*HUAWEI-aaa] local-user admin service-type telnet ssh
[*HUAWEI-aaa] local-user admin level 3
[*HUAWEI-aaa] local-user admin user-group manage-ug
[*HUAWEI-aaa] commit
[*HUAWEI-aaa] quit

[~HUAWEI] ssh user admin
[*HUAWEI] ssh user admin authentication-type password
[*HUAWEI] ssh user admin service-type all
[*HUAWEI] ssh authorization-type default aaa
[*HUAWEI] commit
[~HUAWEI] stelnet server enable
[*HUAWEI] snetconf server enbale
[*HUAWEI] commit

[~HUAWEI] rsa local-key-pair create
The key name will be:HUAWEI_Host
% RSA keys defined for HUAWEI_Host already exist.
Confirm to replace them？ Please select [Y/N]: y
The rangge of public key size is (2048 ~ 2048).
NOTE: Key pair generation will take a short while.
[*HUAWEI] commit
[~HUAWEI] undo telnet server disable
[*HUAWEI] commit
```

### Note
Simpan konfigurasi secara permanen:
```sh
<HUAWEI>save  ##Pastikan berada di user-view bukan di system-view
```
