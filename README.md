# LabDHCPv6  
## Build the Network and Configure Basic Device Settings  
### •	Configure basic settings for each switch. (Optional)  
### •	Assign a device name to the switch  
![alt-текст](https://github.com/DanilChery/CiscoProjectsDHCPv6/blob/main/lab-dhcp-schema.jpg "Текст заголовка логотипа 1")   
### •	Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.  
no ip domain-lookup  
### •	Assign class as the privileged EXEC encrypted password  
### •	Assign cisco as the console password and enable login  
### •	Assign cisco as the VTY password and enable login  
line con 0  
 password 7 104D000A0618  
line aux 0  
line vty 0 4  
 password 7 1511021F0725  
 login  
line vty 5 15  
 password 7 1511021F0725  
 login  
  
### •	Encrypt the plaintext passwords.  
### •	Create a banner that warns anyone accessing the device that unauthorized access is prohibited.  
### •	Shutdown all unused ports  
### •	Save the running configuration to the startup configuration file.  
## •	Configure basic settings for each router.  
### •	Assign a device name to the router.  
### •	Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.  
### •	Assign class as the privileged EXEC encrypted password.  
### •	Assign cisco as the console password and enable login  
### •	Assign cisco as the VTY password and enable login  
line con 0  
 password 7 104D000A0618  
line aux 0  
line vty 0 4  
 password 7 1511021F0725  
 login  
line vty 5 15  
 password 7 1511021F0725  
 login	  
### •	Encrypt the plaintext passwords.  
### •	Create a banner that warns anyone accessing the device that unauthorized access is prohibited.  
### 	Enable IPv6 Routing  
### •	Save the running configuration to the startup configuration file.  
## •	Configure interfaces and routing for both routers.  
### •	Configure the G0/0 and G0/1 interfaces on R1 and R2 with the IPv6 addresses specified in the table above.  
### •	Configure a default route on each router pointed to the IP address of G0/0/0 on the other router.  
interface GigabitEthernet0/0  
 no ip address  
 duplex auto  
 speed auto  
 media-type rj45  
 ipv6 address FE80::1 link-local  
 ipv6 address 2001:DB8:ACAD:2::1/64  
 ipv6 enable  
!  
interface GigabitEthernet0/1  
 no ip address  
 duplex auto  
 speed auto  
 media-type rj45  
 ipv6 address FE80::1 link-local  
 ipv6 address 2001:DB8:ACAD:1::1/64  
 ipv6 enable  
 ipv6 nd other-config-flag  
 ipv6 dhcp server R1-STATELESS  
R2   
interface GigabitEthernet0/0  
 no ip address  
 duplex auto  
 speed auto  
 media-type rj45  
 ipv6 address FE80::2 link-local  
 ipv6 address 2001:DB8:ACAD:2::2/64  
 ipv6 enable  
!  
interface GigabitEthernet0/1  
 no ip address  
 duplex auto  
 speed auto  
 media-type rj45  
 ipv6 address FE80::1 link-local  
 ipv6 address 2001:DB8:ACAD:3::1/64  
 ipv6 enable  
!  
### •	Verify routing is working by pinging R2’s G0/0/1 address from R1  
R1#ping 2001:db8:acad:2::2  
Type escape sequence to abort.  
Sending 5, 100-byte ICMP Echos to 2001:DB8:ACAD:2::2, timeout is 2 seconds:  
!!!!!  
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/2/4 ms  
•	Save the running configuration to the startup configuration file.  
R1#wr mem  
Building configuration...  
[OK]  
R1#  
*Nov 21 11:48:02.901: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...  
*Nov 21 11:48:03.661: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.  
## •	Configure R1 to provide stateless DHCPv6 for PC-A.  
### •	Create an IPv6 DHCP pool on R1 named R1-STATELESS. As a part of that pool, assign the DNS server address as 2001:db8:acad::1 and the domain name as stateless.com. 
R1(config)# ipv6 dhcp pool R1-STATELESS  
R1(config-dhcp)# dns-server 2001:DB8:ACAD::1  
R1(config-dhcp)# domain-name STATELESS.com  
### •	Configure the G0/0/1 interface on R1 to provide the OTHER config flag to the R1 LAN, and specify the DHCP pool you just created as the DHCP resource for this interface.  
R1(config)# interface g0/1  
R1(config-if)# ipv6 nd other-config-flag  
R1(config-if)# ipv6 dhcp server R1-STATELESS  
### •	Save the running configuration to the startup configuration file.  
### •	Restart PC-A.   
### •	Examine the output of ipconfig /all and notice the changes.   
C:\Users\Student> ipconfig /all   
Windows IP Configuration   
  
    Host Name . . . . . . . . . . . . : DESKTOP-3FR7RKA  
   Primary Dns Suffix  . . . . . . . :   
   Node Type . . . . . . . . . . . . : Hybrid  
   IP Routing Enabled. . . . . . . . : No  
   WINS Proxy Enabled. . . . . . . . : No  
   DNS Suffix Search List. . . . . . : STATELESS.com  
  
Ethernet adapter Ethernet0:  
  
   Connection-specific DNS Suffix  . : STATELESS.com  
   Description . . . . . . . . . . . : Intel(R) 82574L Gigabit Network Connection  
   Physical Address. . . . . . . . . : 00-50-56-83-63-6D  
   DHCP Enabled. . . . . . . . . . . : Yes  
   Autoconfiguration Enabled . . . . : Yes  
   IPv6 Address. . . . . . . . . . . : 2001:db8:acad:1:5c43:ee7c:2959:da68(Preferred)  
   Temporary IPv6 Address. . . . . . : 2001:db8:acad:1:3c64:e4f9:46e1:1f23(Preferred)  
   Link-local IPv6 Address . . . . . : fe80::5c43:ee7c:2959:da68%6(Preferred)  
   IPv4 Address. . . . . . . . . . . : 169.254.218.104(Preferred)  
   Subnet Mask . . . . . . . . . . . : 255.255.0.0  
   Default Gateway . . . . . . . . . : fe80::1%6  
   DHCPv6 IAID . . . . . . . . . . . : 50334761  
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-24-F5-CE-A2-00-50-56-B3-63-6D  
   DNS Servers . . . . . . . . . . . : 2001:db8:acad::254  
   NetBIOS over Tcpip. . . . . . . . : Enabled  
   Connection-specific DNS Suffix Search List :  
                                       STATELESS.com  
### •	Test connectivity by pinging R2’s G0/0/1 interface IP address.  
ping 2001:db8:acad:3::1  
2001:db8:acad:3::1 icmp6_seq=1 ttl=64 time=9.932 ms  
2001:db8:acad:3::1 icmp6_seq=2 ttl=64 time=6.190 ms  
2001:db8:acad:3::1 icmp6_seq=3 ttl=64 time=2.734 ms  
2001:db8:acad:3::1 icmp6_seq=4 ttl=64 time=4.711 ms  
2001:db8:acad:3::1 icmp6_seq=5 ttl=64 time=6.739 ms  
## •	Configure a stateful DHCPv6 server on R1  
###  •	Create a DHCPv6 pool on R1 for the 2001:db8:acad:3:aaaa::/80 network. This will provide addresses to the LAN connected to interface G0/0/1 on R2. As a part of the pool, set the DNS server to 2001:db8:acad::254, and set the domain name to STATEFUL.com. 
R1(config)# ipv6 dhcp pool R2-STATEFUL   
R1(config-dhcp)# address prefix 2001:db8:acad:3:aaa::/80  
R1(config-dhcp)# dns-server 2001:db8:acad::254  
R1(config-dhcp)# domain-name STATEFUL.com  
### •	Assign the DHCPv6 pool you just creat ed to interface g0/0 on R1.  
R1(config)# interface g0/0   
R1(config-if)# ipv6 dhcp server R2-STATEFUL  
## •	Configure and verify DHCPv6 relay on R2.  
### •	Configure R2 as a DHCP relay agent for the LAN on G0/1.  
R2(config)# interface g0/1  
R2(config-if)# ipv6 nd managed-config-flag  
R2(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0  
### •	Save your configuration.  
R2#wr mem  
Building configuration...  
[OK]  
R1#  
*Nov 21 11:48:02.901: %GRUB-5-CONFIG_WRITING: GRUB configuration is being updated on disk. Please wait...  
*Nov 21 11:48:03.661: %GRUB-5-CONFIG_WRITTEN: GRUB configuration was written to disk successfully.  
## •	Attempt to acquire an IPv6 address from DHCPv6 on PC-B.  
### •	Open a command prompt on PC-B and issue the command ipconfig /all and examine the output to see the results of the DHCPv6 relay operation  
VPCS> show ipv6  
  
NAME              : VPCS[1]  
LINK-LOCAL SCOPE  : fe80::250:79ff:fe66:6802/64  
GLOBAL SCOPE      : 2001:db8:acad:3:2050:79ff:fe66:6802/64  
DNS               :  
ROUTER LINK-LAYER : 50:00:00:04:00:01  
MAC               : 00:50:79:66:68:02  
LPORT             : 20000  
RHOST:PORT        : 127.0.0.1:30000  
MTU:              : 1500  
### •	Test connectivity by pinging R1’s G0/0/1 interface IP address.  
ping 2001:DB8:ACAD:2::1/64  
  
2001:DB8:ACAD:2::1 icmp6_seq=1 ttl=63 time=13.415 ms  
2001:DB8:ACAD:2::1 icmp6_seq=2 ttl=63 time=5.717 ms  
2001:DB8:ACAD:2::1 icmp6_seq=3 ttl=63 time=5.228 ms   
2001:DB8:ACAD:2::1 icmp6_seq=4 ttl=63 time=4.721 ms  
2001:DB8:ACAD:2::1 icmp6_seq=5 ttl=63 time=7.066 ms  
  
  

[R1-Config](https://github.com/DanilChery/CiscoProjectsDHCPv6/blob/main/lab-dhcp-r1.txt "")  
[r2-Config](https://github.com/DanilChery/CiscoProjectsDHCPv6/blob/main/lab-dhcp-r2.txt "")  
[S1-Config](https://github.com/DanilChery/CiscoProjectsDHCPv6/blob/main/lab-dhcp-s1.txt "")  
[S2-Config](https://github.com/DanilChery/CiscoProjectsDHCPv6/blob/main/lab-dhcp-s2.txt "")  
