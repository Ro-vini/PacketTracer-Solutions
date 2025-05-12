COMANDOS UTILIZADOS

Roteador – Router 0 (R1)
Configuração das interfaces:

enable
configure terminal
interface GigabitEthernet0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
interface GigabitEthernet0/0
ip address 209.165.100.1 255.255.255.0
no shutdown
exit
ip route 0.0.0.0 0.0.0.0 209.165.100.2
do wr

Licenciamento e VPN IPsec:

license boot module c1900 technology-package securityk9

en
conf t
crypto isakmp policy 10
encryption aes 256
authentication pre-share
group 5
exit
crypto isakmp key secretkey address 209.165.200.1
exit
crypto ipsec transform-set R1-R3 esp-aes 256 esp-sha-hmac
exit
crypto map IPSEC-MAP 10 ipsec-isakmp 
set peer 209.165.200.1
set pfs group5
set security-association lifetime seconds 86400
set transform-set R1-R3 
match address 100
exit
interface GigabitEthernet0/0
crypto map IPSEC-MAP
exit
access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
do wr

Roteador – Router 1 (ISP)
Configuração das interfaces:
en
conf t
interface GigabitEthernet 0/1
ip address 209.165.200.2 255.255.255.0
no shutdown
interface GigabitEthernet 0/0
ip address 209.165.100.2 255.255.255.0
no shutdown
exit
do wr

Licenciamento e VPN IPsec:

license boot module c1900 technology-package securityk9

Roteador – Router 1 (R2)
Configuração das interfaces:

enable
conf t
interface GigabitEthernet 0/1
ip address 192.168.3.1 255.255.255.0
no shut
interface GigabitEthernet 0/0
ip address 209.165.200.1 255.255.255.0
no shut
exit
ip route 0.0.0.0 0.0.0.0 209.165.200.2
do wr

Licenciamento e VPN IPsec:

license boot module c1900 technology-package securityk9

en
conf t
crypto isakmp policy 10
encryption aes 256
authentication pre-share
group 5
exit
crypto isakmp key secretkey address 209.165.100.1
exit
crypto ipsec transform-set R3-R1 esp-aes 256 esp-sha-hmac
exit
crypto map IPSEC-MAP 10 ipsec-isakmp 
set peer 209.165.100.1
set pfs group5
set security-association lifetime seconds 86400
set transform-set R3-R1 
match address 100
exit
interface GigabitEthernet0/0
crypto map IPSEC-MAP
exit
access-list 100 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255
do wr

Switch Core
Configuração do roteamento e rota padrão:

conf t
ip routing
ip route 0.0.0.0 0.0.0.0 192.168.2.1
do wr

Criação das VLANs:

vlan 5
name Firewall
do wr
exit

vlan 10
name Vendas
do wr
exit

vlan 20
name RH
do wr
exit

vlan 30
name Financeiro
do wr
exit

vlan 40
name Marketing
do wr
exit

vlan 50
name Contabilidade
do wr
exit

vlan 60
name Recepcao
do wr
exit

vlan 70
name Gerentes
do wr
exit

vlan 80
name Visitantes
do wr
exit

Interface VLAN:

interface vlan 5
ip address 192.168.2.2 255.255.255.0 
no shutdown
do wr
interface vlan 10
ip address 192.168.10.1 255.255.255.192
no shutdown
do wr
exit
interface vlan 20
ip address 192.168.20.1 255.255.255.192
no shutdown
do wr
exit
interface vlan 30
ip address 192.168.30.1 255.255.255.192
no shutdown
do wr
exit
interface vlan 40
ip address 192.168.40.1 255.255.255.192
no shutdown
do wr
exit
interface vlan 50
ip address 192.168.50.1 255.255.255.192
no shutdown
do wr
exit
interface vlan 60
ip address 192.168.60.1 255.255.255.192
no shutdown
do wr
exit
interface vlan 70
ip address 192.168.70.1 255.255.255.192
no shutdown
do wr
exit
interface vlan 80
ip address 192.168.80.1 255.255.255.192
no shutdown
do wr
exit

Configuração das interfaces físicas:

interface GigabitEthernet1/0/1
switchport mode access
switchport access vlan 5
no shutdown
exit
interface GigabitEthernet1/0/2
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70
do wr
exit
interface GigabitEthernet1/0/3
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70
do wr
exit
interface GigabitEthernet1/0/4
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70
do wr
exit




Switchs de Borda
Tronco para o switch core:

enable
conf t
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,60,70
do wr
exit

Switch0
Configuração das interfaces físicas:

interface range FastEthernet0/1
switchport mode access
switchport access vlan 30
do wr
exit
interface range FastEthernet0/2
switchport mode access
switchport access vlan 30
do wr
exit
interface range FastEthernet0/3
switchport mode access
switchport access vlan 40
do wr
exit
interface range FastEthernet0/4
switchport mode access
switchport access vlan 40
do wr
exit

Switch1
Configuração das interfaces físicas:

interface range FastEthernet0/1
switchport mode access
switchport access vlan 50
do wr
exit
interface range FastEthernet0/2
switchport mode access
switchport access vlan 50
do wr
exit
interface range FastEthernet0/3
switchport mode access
switchport access vlan 50
do wr
exit
interface range FastEthernet0/4
switchport mode access
switchport access vlan 50
do wr
exit
interface range FastEthernet0/5
switchport mode access
switchport access vlan 50
do wr
exit
interface range FastEthernet0/6
switchport mode access
switchport access vlan 50
do wr
exit

Switch2
Configuração das interfaces físicas:

interface range FastEthernet0/1
switchport mode access
switchport access vlan 70
do wr
exit
interface range FastEthernet0/2
switchport mode access
switchport access vlan 70
do wr
exit
interface range FastEthernet0/3
switchport mode access
switchport access vlan 70
do wr
exit
interface range FastEthernet0/4
switchport mode access
switchport access vlan 70
do wr
exit
interface range FastEthernet0/5
switchport mode access
switchport access vlan 70
do wr
exit
interface range FastEthernet0/6
switchport mode access
switchport access vlan 60
do wr
exit
interface range FastEthernet0/7
switchport mode access
switchport access vlan 60
do wr
exit

Switch Core

vlan 99
name backbone
do wr
exit

interface Vlan99
ip address 192.168.99.1 255.255.255.0
no shutdown
do wr
exit

interface GigabitEthernet1/0/24
switchport mode access
switchport access vlan 99
no shutdown
exit
do wr

IP Helper Address para DHCP:

switch core
interface vlan 10
ip helper-address 192.168.99.100
do wr
exit
interface vlan 20
ip helper-address 192.168.99.100
do wr
exit
interface vlan 30
ip helper-address 192.168.99.100
do wr
exit
interface vlan 40
ip helper-address 192.168.99.100
do wr
exit
interface vlan 50
ip helper-address 192.168.99.100
do wr
exit
interface vlan 60
ip helper-address 192.168.99.100
do wr
exit
interface vlan 70
ip helper-address 192.168.99.100
do wr
exit
interface vlan 80
ip helper-address 192.168.99.100
do wr
exit

Switch Core
Configuração das interface Logica para os APs WLC:

vlan 1
do wr
exit

interface vlan 1
ip address 192.168.1.10 255.255.255.224
no shutdown
do wr
exit

interface vlan 1
ip helper-address 192.168.99.100
do wr

interface GigabitEthernet1/0/5
switchport mode trunk
switchport trunk allowed vlan 1
do wr
exit

Switchs de Borda

Switch3

Configuração das interfaces físicas para os APs WLC:


conf t
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 1
do wr
exit

interface range FastEthernet0/1
switchport mode access
switchport access vlan 1
do wr

interface range FastEthernet0/2
switchport mode access
switchport access vlan 1
do wr

interface range FastEthernet0/3
switchport mode access
switchport access vlan 1
do wr

interface range FastEthernet0/4
switchport mode access
switchport access vlan 1
do wr

interface range FastEthernet0/5
switchport mode access
switchport access vlan 1
do wr

interface range FastEthernet0/6
switchport mode access
switchport access vlan 1
do wr

interface range FastEthernet0/24
switchport mode access
switchport access vlan 1
do wr


WLC Controller

IP: 192.168.1.10

Login:
AdminWLC
secretPasswordAdminWLC!

WIFI

SSID: WIFI-Vendas
Senha: Vendas@1234

SSID: WIFI-Visitantes
Senha: secretPasswordWIFI!

SSID: WIFI-HRL
Senha: HRL@1234

SSID: WIFI-Gerentes
Senha: Gerentes@1234

SSID: WIFI-Empresa
Senha: Empresa@1234
