# configure Cisco switch
## connect to switch by PuTTY
- get PuTTY [PuTTY](https://www.putty.org/)
- connect by 
  - RS-232 cable or 
  - RJ45 cable at the back of switch which is named CONSOLE or
  - USB to RS-232 cable
- look for a new USB Serial Port connection like (COM4) - devmgmt.msc
- set PuTTY
#### USB to RS-232 cable
  - Session > Serial line - COM4 | Speed 9600 | choose Serial
  - Serial > Serial line to connect to - COM4 | Speed 9600 | Data bits 8 | Stop bits 1 | Parity None Flow Control XON/XOFF

#### RS-232 cable
  - Session > Serial line - COM1 | Speed 115200 | choose Serial
  - Serial > Serial line to connect to - COM1 | Speed 115200 | Data bits 8 | Stop bits 1 | Parity None Flow Control XON/XOFF

- connect with PuTTY
- login by default (cisco|cisco)
```cisco
User Name:cisco
Password:******
```
## erase old data
```cisco
Switch> enable
Switch> configure terminal
SWITCH# delete flash:vlan.dat
SWITCH# erase startup-config
SWITCH# reload
```
or
```cisco
Switch> enable
Switch> configure terminal
SWITCH# write erase 
SWITCH# reload
```
```cisco
Switch> enable
write erase

## configure Hostname
```cisco
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch1
```
## configure Local User & Password
```cisco
Switch1(config)# username user1 password cisco
```
## configure login banner, save logins
```cisco
Switch1(config)# banner motd #TEXT#
```
#### encrypted password for privileged exec mode
```cisco
Switch1(config)# enable password cisco
```
#### or decrypted password for privileged exec mode
```cisco
Switch1(config)# enable secret cisco
```
#### password for console mode | global config mode
```cisco
Switch1(config)# line console 0
Switch1(config-line)# password
Switch1(config-line)# login
```
## configure encrypt all passwords
```cisco
Switch1(config)# service password-encryption
Switch1(config)# exit
```
## configure ip interface for vlan
```cisco
Switch1(config)# interface vlan 1
Switch1(config)# ip address 192.168.5.25 255.255.255.0
Switch1(config)# exit
```
## configure IP Default-Gateway
```cisco
Switch1(config)# ip default-gateway 192.168.5.1
```
## configure SSH Server
```cisco
Switch1(config)# hostname <XXX>
Switch1(config)# ip domain-name <beispiel.de>
Switch1(config)# crypto key generate rsa
Switch1(config)# username <NAME>
Switch1(config)# line vty 0 15
Switch1(config-line)# password <XXX>
Switch1(config-line)# login
Switch1(config-line)# transport input ssh
```
Line vty 0 4 = 5 simultaneous virtual connections
Line vty 0 15 = 16 simultaneous virtual connections Maximum allowed values
```cisco
Switch1(config)# line vty 0 4
Switch1(config-line)# password cisco
Switch1(config-line)# login
Switch1(config-line)# exit
```
## configure Exec-Timeout
configures inactive session timeout in min for the vty 0 port
```cisco
Switch1(config)# line vty 0 15
Switch1(config)# exec-timeout 0 30
Switch1(config)# exit
```

## check your configuration
```cisco
Switch1# show running-config
```
## configure VLAN on port
VLAN1 default for router
VLAN 30 for DC
```cisco
Switch1# show vlan

Switch1# conf t
Switch1# int range <Fa0/1-24>
Switch1# switchport mode access
Switch1# switchport access vlan <ID>
Switch1# no shut
Switch1# end

Switch1# show vlan
```
## To save this configuration to NVRAM
```cisco
Switch1# copy running-config startup-config
Destination filename [startup-config]?y
Building configuration... [OK]
Switch1#write memory
Building configuration... [OK]
```
### check config
```cisco
Switch1# show startup-config
```
| Cisco IOS Command  | Description   |
| -----------------  | -------------:|
| show interface     | Displays current status and configuration details for all interfaces in the system |
| show processes cpu | Displays CPU utilization and the current processes running in the system |
| show buffers       | Shows how system buffers are currently allocated and functioning for packet forwarding |
| show memory        | Shows how memory is allocated to various system functions and memory utilization |
| show diag          | Displays details on hardware cards in the system |
| show ip route      | Displays the current active IP routing table |
| show arp           | Displays the current active IP address-to-MAC address mapping in the ARP table |

![Cisco modes](./img%20-%20cisco%20modes.png)

# configure Firewall / Router
wf.msc
```powershell
Get-NetAdapter -Name <Ethernet Name>
Rename-NetAdapter -Name <Ethernet Name> -NewName <new Ethernet Name>

Get-NetIPConfiguration -InterfaceAlias <new Ethernet Name> -Detailed

Get-NetIPInterface
```
set IP
```powershell
$ipParams = @{
InterfaceIndex = 8
IPAddress = "192.168.2.50"
PrefixLength = 24
AddressFamily = "IPv4"
}
New-NetIPAddress @ipParams
```