بعد تشغيل الشبكة بشكل اعتيادي 
نقوم بتفعيل EIGRP في R1

R1(config)# router eigrp 1
R1(config-router)# no auto-summary
R1(config-router)# network 172.16.0.0
R1(config-router)# network 192.168.48.0
R1(config-router)# network 192.168.49.0
R1(config-router)# network 192.168.50.0
R1(config-router)# network 192.168.51.0
R1(config-router)# network 192.168.70.0

or

R1(config)# router eigrp 1
R1(config-router)# no auto-summary
R1(config-router)# network 172.16.0.0
R1(config-router)# network 192.168.0.0 0.0.255.255
________________________________

ثم نفعل ال EIGRP في الR2
R2(config)# router eigrp 1
R2(config-router)# no auto-summary
R2(config-router)# network 172.16.0.0
______________
ثم نقوم بتفعيل السموري في  R1
R1(config)# interface Serial0/0/0
R1(config-if)# ip summary-address eigrp 1 192.168.48.0 255.255.254.0
_____________
ثم نقوم بتفعيل الاوسبف على R2
R2(config)# interface Loopback100
R2(config-if)# ip ospf network point-to-point
R2(config-if)# exit
R2(config)# router ospf 1
R2(config-router)# network 172.16.23.0 0.0.0.255 area 0
R2(config-router)# network 172.16.100.0 0.0.0.255 area 10
______________
ثم نقوم بوضع ال ip ospf network على R3
R3(config)# interface Loopback0
R3(config-if)# ip ospf network point-to-point
R3(config-if)# exit
R3(config)# interface range lo 8 - 11                                 
R3(config-if-range)# ip ospf network point-to-point
R3(config-if-range)# exit
R3(config)# 
R3(config)# interface range lo 20, lo 25, lo 30, lo 35, lo 40           
R3(config-if-range)# ip ospf network point-to-point                   
R3(config-if-range)# exit
_______________
ثم نقوم بتفعيل الاوسبف على R3
R3(config)# router ospf 1
R3(config-router)# network 172.16.0.0 0.0.255.255 area 0
R3(config-router)# network 192.168.0.0 0.0.255.255 area 0
R3(config-router)# network 192.168.8.0 0.0.3.255 area 20
R3(config-router)# area 20 range 192.168.8.0 255.255.252.0
_________________
ثم redistribution  على R2
R2(config)# router ospf 1
R2(config-router)# redistribute eigrp 1 subnets
R2(config-router)# exit

R2(config)# router eigrp 1
R2(config-router)# redistribute ospf 1 metric 10000 100 255 1 1500
R2(config-router)# exit

R2(config)# router eigrp 1
R2(config-router)# default-metric 10000 100 255 1 1500
R2(config-router)# redistribute ospf 1
R2(config-router)# end

R2(config)# router ospf 1
R2(config-router)# summary-address 192.168.48.0 255.255.252.0

__________________

