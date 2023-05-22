Router R1
hostname R1
!
interface Loopback1
 description Engineering Department
 ip address 10.1.1.1 255.255.255.0
 ip ospf network point-to-point
!
interface Loopback30
 ip address 172.30.30.1 255.255.255.252
!
interface Serial0/0/0
 ip address 10.1.12.1 255.255.255.0
 clock rate 64000
 no shutdown
!
router ospf 1
 router-id 1.1.1.1
 network 10.1.1.0 0.0.0.255 area 0
 network 10.1.12.0 0.0.0.255 area 0
 default-information originate always
!
end
_____________________
Router R2
hostname R2
!
interface Loopback2
 description Marketing Department
 ip address 10.1.2.1 255.255.255.0
 ip ospf network point-to-point
!
interface Serial0/0/0
 ip address 10.1.12.2 255.255.255.0
 no shutdown
!
interface Serial0/0/1
 ip address 10.1.23.2 255.255.255.0
 clock rate 64000
 no shutdown
!
router ospf 1
 router-id 2.2.2.2
 area 23 virtual-link 3.3.3.3
 network 10.1.2.0 0.0.0.255 area 0
 network 10.1.12.0 0.0.0.255 area 0
 network 10.1.23.0 0.0.0.255 area 23
!
end
_________________________
Router R3
hostname R3
!
interface Loopback3
 description Accounting Department
 ip address 10.1.3.1 255.255.255.0
 ip ospf network point-to-point
!
interface Loopback100
 ip address 192.168.100.1 255.255.255.0
 ip ospf network point-to-point
!
interface Loopback101
 ip address 192.168.101.1 255.255.255.0
 ip ospf network point-to-point
!
interface Loopback102
 ip address 192.168.102.1 255.255.255.0
 ip ospf network point-to-point
!
interface Loopback103
 ip address 192.168.103.1 255.255.255.0
 ip ospf network point-to-point
!
interface Serial0/0/1
 ip address 10.1.23.3 255.255.255.0
 no shutdown
!
router ospf 1
 router-id 3.3.3.3
 area 23 virtual-link 2.2.2.2
 area 100 range 192.168.100.0 255.255.252.0
 network 10.1.3.0 0.0.0.255 area 23
 network 10.1.23.0 0.0.0.255 area 23
 network 192.168.100.0 0.0.3.255 area 100
!
end
