# Security Hardening
This is a guide for security hardening (and STIGs) for Proxmox. Keep in mind I will be adding more to this as I go. Firewall rules should be tailored to your organizational needs and won't be included in this.

## rsyslog
```
apt install rsyslog
vim /etc/rsyslog.conf
	*.* @<IP>:514
	*.* @@<IP>:514
```
Configure rsyslog based on your organizations settings.

## auditd
```
apt install auditd
vim /etc/audit/rules.d/audit.rules
```
You can use the rules from my `audit.rules` to satisfy STIG requirements.

## snmp
```
apt install snmpd
```

## Sessions
```
vim /etc/ssh/sshd_config
	MaxAuthTries 3
    	MaxSessions 10
vim ~/.bashrc
    	TMOUT=300
```

## Password Quality
```
apt install libpam-pwquality
vim /etc/pam.d/common-password
    	password        required                        pam_pwquality.so retry=3
	password        required                        pam_pwquality.so minlin=15
	password        required                        pam_pwquality.so dcredit=-1
	password        required                        pam_pwquality.so ucredit=-1
	password        required                        pam_pwquality.so lcredit=-1
	password        required                        pam_pwquality.so ocredit=-1
	password        required                        pam_pwquality.so difok=8
```

## Message of the Day
```
vim /etc/motd
```

## chrony
```
vim /etc/chrony/chrony.conf
    	server <NTP IP> iburst
```

## Group Permissions
```
Datacenter > Permissions > Groups > Create
Datacenter > Permissions > Add > Group Permission
Datacenter > Permissions > Users > Edit > Group
```

## TFA (Yubikey)
```
Get Yubico API Key
    	https://upgrade.yubico.com/getapikey/
Datacenter > Permissions > Realms > Edit > Require TFA > Yubico
    	Add Yubico API Id
    	Add Yubico API Key
    	Select Default
Datacenter > Permissions > Two Factor > Add > Yubico OTP
  	Select User
  	Add Description
  	Add Yubico OTP value using your Yubikey
```
