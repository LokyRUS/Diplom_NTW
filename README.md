
# [файл.pkt для скачивания](https://github.com/LokyRUS/Diplom_NTW/blob/nevidimka/file/ntw_topology_8.2.2(%D0%B2%D0%B5%D1%80%D1%81%D0%B8%D1%8F%20%D0%B4%D0%BE%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%BA%D0%B0)%20.pkt)
# [Таблицу распределения подсетей и адресов](https://github.com/LokyRUS/Diplom_NTW/blob/nevidimka/file/ip-address-table%20.xlsx)
  
### Ответ на вопрос:  
Чем чреват выбор протокола GRE для объединения сети ЦО и филиала в 15 пункте. Какие более безопасные альтернативы можно предложить без потери функциональности?

Ответ: В данном случае у этого GRE отсутствует какое либо шифорвание, для безопасности, можно внедрить в тоннель IPSec. Альтернативой может послужить VPN тоннель. 

## Топология
![images1](https://github.com/LokyRUS/Diplom_NTW/blob/nevidimka/file/1.PNG)

# назначение Vlan на свиче swich0
```
Switch#configure terminal 
Switch(config)#vlan 100
Switch(config-vlan)#name mgmt
Switch(config-vlan)#ex
Switch(config)#vlan 200
Switch(config-vlan)#name user
Switch(config-vlan)#ex
Switch(config)#vlan 300
Switch(config-vlan)#name wlan
Switch(config-vlan)#ex
Switch(config)#vlan 400
Switch(config-vlan)#name voip
Switch(config-vlan)#ex
Switch(config)#vlan 500
Switch(config-vlan)#name printer
Switch(config-vlan)#ex

Switch(config)#
```
- Включение портфаста

```
Switch(config)#spanning-tree portfast default

```
- Включение RSTP
```
spanning-tree mode rapid-pvst
spanning-tree vlan 100,200,300,400,500 priority 49152

```
- ip dhcp snooping
```
Switch(config)#ip dhcp snooping
Switch(config)#interface range fastEthernet 0/2-3
Switch(config-if-range)#ip dhcp snooping trust 
``` 
- Настройка Port security
```
Switch#conf terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport port-security 
Switch(config-if)#switchport port-security mac-address sticky 
Switch(config-if)#switchport port-security maximum 3
Switch(config-if)#switchport port-security aging time 20
Switch(config-if)#switchport port-security violation restrict 
Switch(config-if)#ex
Switch(config)#ex
Switch#
%SYS-5-CONFIG_I: Configured from console by console
```
- Логирование
```
Switch(config)#logging on
Switch(config)#logging host 192.168.100.100
```
# назначение Vlan на свиче swich1
```
Switch#configure terminal 
Switch(config)#vlan 100
Switch(config-vlan)#name mgmt
Switch(config-vlan)#ex
Switch(config)#vlan 200
Switch(config-vlan)#name user
Switch(config-vlan)#ex
Switch(config)#vlan 500
Switch(config-vlan)#name printer
Switch(config-vlan)#ex

Switch(config)#
```
- Включение портфаста

```
Switch(config)#spanning-tree portfast default
```
- Включение RSTP
```
spanning-tree mode rapid-pvst
spanning-tree vlan 100,200,300,400,500 priority 40960
```
- ip dhcp snooping
```
Switch(config)#ip dhcp snooping
Switch(config)#interface range fastEthernet 0/2-3
Switch(config-if-range)#ip dhcp snooping trust 
``` 
- Настройка Port security
```
Switch#conf terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range fastEthernet 0/1, fastEthernet 0/4
Switch(config-if)#switchport port-security 
Switch(config-if)#switchport port-security mac-address sticky 
Switch(config-if)#switchport port-security maximum 3
Switch(config-if)#switchport port-security aging time 20
Switch(config-if)#switchport port-security violation restrict 
Switch(config-if)#ex
Switch(config)#ex
Switch#
%SYS-5-CONFIG_I: Configured from console by console
```
- Логирование
```
Switch(config)#logging on
Switch(config)#logging host 192.168.100.100
```

# назначение Vlan на свиче swich2
```
Switch#configure terminal 
Switch(config)#vlan 100
Switch(config-vlan)#name mgmt
Switch(config-vlan)#ex
Switch(config)#vlan 300
Switch(config-vlan)#name wlan
Switch(config-vlan)#ex
Switch(config)#vlan 400
Switch(config-vlan)#name voip
Switch(config-vlan)#ex
```
- Включение портфаста

```
Switch(config)#spanning-tree portfast default
```
- Включение RSTP
```
spanning-tree mode rapid-pvst

spanning-tree vlan 100,200,300,400,500 priority 36864
```
- ip dhcp snooping
```
Switch(config)#ip dhcp snooping
Switch(config)#interface range fastEthernet 0/1-2
Switch(config-if-range)#ip dhcp snooping trust 
Switch(config)#interface range fastEthernet 0/4
Switch(config-if-range)#ip dhcp snooping trust 

```
- Настройка Port security
```
Switch#conf terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range fastEthernet 0/3-5
Switch(config-if)#switchport port-security 
Switch(config-if)#switchport port-security mac-address sticky 
Switch(config-if)#switchport port-security maximum 3
Switch(config-if)#switchport port-security aging time 20
Switch(config-if)#switchport port-security violation restrict 
Switch(config-if)#ex
Switch(config)#ex
Switch#
%SYS-5-CONFIG_I: Configured from console by console
```
- Логирование
```
Switch(config)#logging on
Switch(config)#logging host 192.168.100.100
```

# Настройка портов на swich0

- Настройка порта рабочего места пользователя
```
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 200
Switch(config-if)#switchport voice vlan 400
Switch(config-if)#ex
Switch(config)#
```
- Настройка порта точки доступа wifi

```
Switch#configure terminal 
Switch(config)#interface range fastEthernet 0/4
Switch(config-if-range)#switchport mode trunk 
Switch(config-if-range)#switchport trunk allowed vlan 100,300
Switch(config-if-range)#switchport trunk native vlan 100
```
# Настройка портов на swich1

- Настройка порта рабочего места пользователя
```
Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 200
Switch(config-if)#ex
Switch(config)#
```
- Настройка порта зоны принтора
```
Switch(config)#interface fastEthernet 0/4
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 500
Switch(config-if)#ex
Switch(config)#
```
# Настройка портов на swich2
- Настройка порта зоны 
```
Switch(config)#interface fastEthernet 0/4
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 100
Switch(config-if)#ex
Switch(config)#
```
- Настройка порта зоны wlC
```
Switch#configure terminal 
Switch(config)#interface range fastEthernet 0/3
Switch(config-if-range)#switchport mode trunk 
Switch(config-if-range)#switchport trunk allowed vlan 100,3000
Switch(config-if-range)#switchport trunk native vlan 100
```
- Настройка порта зоны VOip

```
Switch(config)#interface fastEthernet 0/5
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 400
Switch(config-if)#ex
Switch(config)#
```


-Настройка портов к коре
```
Switch#configure terminal 
Switch(config)#interface range fastEthernet 0/1-2
Switch(config-if-range)#switchport mode trunk 
Switch(config-if-range)#switchport trunk allowed vlan 100,300,400
```




# Настройка cora1
Создание Vlan 
```
Switch#configure terminal 
Switch(config)#vlan 100
Switch(config-vlan)#name mgmt
Switch(config-vlan)#ex
Switch(config)#vlan 200
Switch(config-vlan)#name user
Switch(config-vlan)#ex
Switch(config)#vlan 300
Switch(config-vlan)#name wlan
Switch(config-vlan)#ex
Switch(config)#vlan 400
Switch(config-vlan)#name voip
Switch(config-vlan)#ex
Switch(config)#vlan 500
Switch(config-vlan)#name printer
Switch(config-vlan)#ex

Switch(config)#
```
перевод агрегированного какнала в trunk на с свиче l3
 
```
Switch(config)#interface f
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport trunk encapsulation dot1q 
Switch(config-if)#switchport mode trunk 
Switch(config)#interface f
Switch(config)#interface fastEthernet 0/7
Switch(config-if)#switchport trunk encapsulation dot1q 
Switch(config-if)#switchport mode trunk 

```
- Включение портфаста

```
Switch(config)#spanning-tree portfast default
```
- Включение RSTP
```
spanning-tree mode rapid-pvst
spanning-tree vlan 100,200,300,400,500 root primary 

```
- приоритет RSTP
```
spanning-tree vlan 10,20,30 root primary 
```
Настройка vlan интерфейсов
```
Switch(config)#interface vlan 100
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan100, changed state to up
Switch(config-if)#ip address 192.168.100.1 255.255.255.128
Switch(config-if)#ex
Switch(config)#interface vlan 200
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan200, changed state to up
Switch(config-if)#ip address 192.168.20.1 255.255.255.0
Switch(config-if)#ex
Switch(config)#
Switch(config)#interface vlan 300
Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan300, changed state to up
Switch(config-if)#ip address 192.168.30.1 255.255.255.0
Switch(config-if)#ex
Switch(config)#
Switch(config)#interface vlan 400
Switch(config-if)#ip address 192.168.40.1 255.255.255.0
Switch(config-if)#ex
Switch(config)#
Switch(config)#interface vlan 500
Switch(config-if)#ip address 192.168.50.1 255.255.255.128
Switch(config-if)#ex
Switch(config)#

```
Настройка ip хелпер на коре 1 для DHCP
```
Switch(config)#interface vlan 200
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex
Switch(config)#interface vlan 300
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex
Switch(config)#interface vlan 400
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex
Switch(config)#interface vlan 500
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex

```
HSRP

Назначены Layer Switch 1 
```
Switch(config)#interface vlan 100
Switch(config-if)#standby 1 ip 192.168.100.1
Switch(config-if)#standby 1 priority 100
standby 1 track FastEthernet0/4
Switch(config)#interface vlan 200
Switch(config-if)#standby 1 ip 192.168.20.1
Router(config-if)#standby 1 priority 100
standby 1 track FastEthernet0/5
Switch(config)#interface vlan 300
Switch(config-if)#standby 1 ip 192.168.30.1
Switch(config-if)#standby 1 priority 100
standby 1 track FastEthernet0/4
Switch(config)#interface vlan 400
Switch(config-if)#standby 1 ip 192.168.40.1
Switch(config-if)#standby 1 priority 100
standby 1 track FastEthernet0/4
Switch(config)#interface vlan 500
Switch(config-if)#standby 1 ip 192.168.50.1
Switch(config-if)#standby 1 priority 100
standby 1 track FastEthernet0/4
```


Настрокай статической агрегации L2 на обоих коммутаторах.
```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range fastEthernet 0/3 , fastEthernet 0/7
Switch(config-if-range)#shutdown 
Switch(config-if-range)#channel-group 1 mode on
Switch(config-if-range)#no shutdown 
```
- Настройка интерфейса в сторону asa1
```
Switch#configure terminal 
Switch(config)#interface fastEthernet 0/4
Switch(config-if)#no switchport 
Switch(config-if)#ip address 10.1.30.2 255.255.255.252
Switch(config-if)#no shutdown 
Switch(config-if)#
```
# Настройка cora2
Создание Vlan 
```
Switch#configure terminal 
Switch(config)#vlan 100
Switch(config-vlan)#name mgmt
Switch(config-vlan)#ex
Switch(config)#vlan 200
Switch(config-vlan)#name user
Switch(config-vlan)#ex
Switch(config)#vlan 300
Switch(config-vlan)#name wlan
Switch(config-vlan)#ex
Switch(config)#vlan 400
Switch(config-vlan)#name voip
Switch(config-vlan)#ex
Switch(config)#vlan 500
Switch(config-vlan)#name printer
Switch(config-vlan)#ex

Switch(config)#
```
перевод агрегированного какнала в trunk на с свиче l3
 
```
Switch(config)#interface f
Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport trunk encapsulation dot1q 
Switch(config-if)#switchport mode trunk 
Switch(config)#interface f
Switch(config)#interface fastEthernet 0/7
Switch(config-if)#switchport trunk encapsulation dot1q 
Switch(config-if)#switchport mode trunk 

```

- Включение портфаста

```
Switch(config)#spanning-tree portfast default
```
- Включение RSTP
```
spanning-tree mode rapid-pvst
spanning-tree vlan 100,200,300,400,500 root secendory 

```

Настройка vlan интерфейсов
```
Switch(config)#interface vlan 100
Switch(config-if)#
Switch(config-if)#ip address 192.168.100.2 255.255.255.128
Switch(config-if)#ex
Switch(config)#interface vlan 200
Switch(config-if)#
Switch(config-if)#ip address 192.168.20.2 255.255.255.0
Switch(config-if)#ex
Switch(config)#
Switch(config)#interface vlan 300
Switch(config-if)#
Switch(config-if)#ip address 192.168.30.2 255.255.255.0
Switch(config-if)#ex
Switch(config)#interface vlan 400
Switch(config-if)#
Switch(config-if)#ip ad
Switch(config-if)#ip address 192.168.40.2 255.255.255.0
Switch(config-if)#ex
Switch(config)#
Switch(config)#interface vlan 500
Switch(config-if)#
Switch(config-if)#ip ad
Switch(config-if)#ip address 192.168.50.2 255.255.255.128
Switch(config-if)#ex
Switch(config)#

```
Настройка ip хелпер на коре 1 для DHCP
```
Switch(config)#interface vlan 200
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex
Switch(config)#interface vlan 300
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex
Switch(config)#interface vlan 400
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex
Switch(config)#interface vlan 500
Switch(config-if)# ip helper-address 192.168.100.100
Switch(config-if)#ex

```
HSRP

Назначены Layer Switch 2 

```
Switch(config)#interface vlan 100
Switch(config-if)#standby 1 ip 192.168.100.50
Switch(config-if)#standby 1 priority 90
Switch(config-if)#standby 1 preempt
standby 1 track FastEthernet0/5

Switch(config)#interface vlan 200
Switch(config-if)#standby 1 ip 192.168.20.1
Switch(config-if)#standby 1 priority 90
Switch(config-if)#standby 1 preempt
standby 1 track FastEthernet0/5

Switch(config)#interface vlan 300
Switch(config-if)#standby 1 ip 192.168.30.1
Switch(config-if)#standby 1 priority 90
Switch(config-if)#standby 1 preempt
standby 1 track FastEthernet0/5

Switch(config)#interface vlan 400
Switch(config-if)#standby 1 ip 192.168.40.1
Switch(config-if)#standby 1 priority 90
Switch(config-if)#standby 1 preempt
standby 1 track FastEthernet0/5

Switch(config)#interface vlan 500
Switch(config-if)#standby 1 ip 192.168.50.1
Switch(config-if)#standby 1 priority 90
Switch(config-if)#standby 1 preempt
standby 1 track FastEthernet0/5
```
Настрокай статической агрегации L2 на обоих коммутаторах.
```
Switch>enable 
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#interface range fastEthernet 0/3 , fastEthernet 0/7
Switch(config-if-range)#shutdown 
Switch(config-if-range)#channel-group 1 mode auto
Switch(config-if-range)#no shutdown 
```
- Настройка итерфейса в соторону asa2
```
Switch>en
Switch>enable 
Switch#configure terminal 
Switch(config)#interface fastEthernet 0/5
Switch(config-if)#no switchport 
Switch(config-if)#ip address 10.1.40.2 255.255.255.252
Switch(config-if)#no shutdown 
Switch(config-if)#
```
# настройка border1 
- настройка интерфейса в сторонну asa1
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.1.10.1 255.255.255.252
Router(config-if)#no shutdown 
``` 
- настройка интерфейса в сторону border2
```
Router>enable 
Router#configure terminal 
Router(config)#interface gigabitEthernet 0/1/0
Router(config-if)#ip address 10.10.0.1 255.255.255.252
ciscoasa(config-if)#no shutdown 
```


# настройка asa1
```
ciscoasa>en
ciscoasa>enable 
Password: 
ciscoasa#
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/4
ciscoasa(config-if)#ip address 10.1.10.2 255.255.255.252
ciscoasa(config-if)#no shutdown
```
- настройка интерфейса в сторону core1
```
ciscoasa>enable 
Password: 
ciscoasa#
ciscoasa#
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 0/1/0
ciscoasa(config-if)#ip address 10.10.0.1 255.255.255.252
ciscoasa(config-if)#no shutdown 
```
- настройка интерфейса в сторону border2
```
ciscoasa>enable 
Password: 
ciscoasa#
ciscoasa#
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/1
ciscoasa(config-if)#ip address 10.1.30.1 255.255.255.252
ciscoasa(config-if)#no shutdown 
```
- Настройка доступности зон на asa 1

```
ciscoasa>enable 
Password: 
ciscoasa#
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/1
ciscoasa(config-if)#name
ciscoasa(config-if)#nameif INSIDE
INFO: Security level for "INSIDE" set to 0 by default.
ciscoasa(config-if)#security-level 100
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/4
ciscoasa(config-if)#nameif OUTSIDE
INFO: Security level for "OUTSIDE" set to 0 by default.
ciscoasa(config-if)#security-level 0
ciscoasa(config-if)#
```

# настройка border2 
```
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 10.1.20.1 255.255.255.252
Router(config-if)#no shutdown 
``` 


- настройка интерфейса в сторону border1
```
Router>enable 
Router#configure terminal 
Router(config)#interface gigabitEthernet 0/1/0
Router(config-if)#ip address 10.10.0.2 255.255.255.252
ciscoasa(config-if)#no shutdown 
```
# настройка asa2
- настройка интерфейса в сторону border2
```
ciscoasa>en
ciscoasa>enable 
Password: 
ciscoasa#
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/3
ciscoasa(config-if)#ip address 10.1.20.2 255.255.255.252
ciscoasa(config-if)#no shutdown
```
- настройка интерфейса в сторону core2
```
ciscoasa>enable 
Password: 
ciscoasa#
ciscoasa#
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/2
ciscoasa(config-if)#ip address 10.1.40.1 255.255.255.252
ciscoasa(config-if)#no shutdown 
```
- настройка интерфейса в сторону DMZ
```
Router>enable 
Router#configure terminal 
Router(config)#interface gigabitEthernet 1/1
Router(config-if)#ip address 192.168.200.1 255.255.255.192
ciscoasa(config-if)#no shutdown 
```

- Настройка доступности зон на asa 2

```
ciscoasa>enable 
Password: 
ciscoasa#
ciscoasa#
ciscoasa#configure terminal 
ciscoasa(config)#interface gigabitEthernet 1/2
ciscoasa(config-if)#nameif INSIDE
INFO: Security level for "INSIDE" set to 0 by default.
ciscoasa(config-if)#security-level 100
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/3
ciscoasa(config-if)#nameif OUTSIDE
INFO: Security level for "OUTSIDE" set to 0 by default.
ciscoasa(config-if)#security-level 0
ciscoasa(config-if)#ex
ciscoasa(config)#interface gigabitEthernet 1/1
ciscoasa(config-if)#nameif DMZ
INFO: Security level for "DMZ" set to 0 by default.
ciscoasa(config-if)#security-level 50
ciscoasa(config-if)#
```

# Настройка ospf маршрутизации

- Настройкака на core1
```
Switch#configure terminal 
Switch(config)#ip routing 
Switch(config)#router ospf 1
Switch(config-router)#ex
network 10.1.30.0 255.255.255.252 area 1
network 192.168.20.0 255.255.255.0 area 1
network 192.168.30.0 255.255.255.0 area 1
network 192.168.40.0 255.255.255.0 area 1
network 192.168.50.0 255.255.255.128 area 1
network 192.168.100.0 255.255.255.128 area 1
Switch(config)#interface fastEthernet 0/4
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 100
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 200
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 300
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 400
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex  
Switch(config)#interface vlan 500
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
```
- Настройкака на core2
```
Switch#configure terminal 
Switch(config)#ip routing 
Switch(config)#router ospf 1
Switch(config-router)#ex
Switch(config)#interface fastEthernet 0/5
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 100
Switch(config)#interface vlan 100
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 200
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 300
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
Switch(config)#interface vlan 400
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex  
Switch(config)#interface vlan 500
Switch(config-if)#ip ospf 1 area 1
Switch(config-if)#ex
```
# Настройка ospf маршрутизации - asa1
```
ciscoasa#configure terminal 
ciscoasa(config)#router ospf 1
ciscoasa(config-router)#network 10.1.30.0 255.255.255.252 area 1
ciscoasa(config-router)#network 10.1.10.0 255.255.255.252 area 1
ciscoasa(config-router)network 192.168.200.0 255.255.255.192 area 1
ciscoasa(config-router)#
```
# Настройка ospf маршрутизации - asa2
```
ciscoasa#configure terminal 
ciscoasa(config)#router ospf 1
ciscoasa(config-router)#network 10.1.40.0 255.255.255.252 area 1
ciscoasa(config-router)#network 10.1.20.0 255.255.255.252 area 1
ciscoasa(config-router)#ex
ciscoasa(config)#ex
```
# Настройка ospf маршрутизации - border1
- Настройка ospf
```
Router>enable 
Router#configure terminal 
Router(config)#router ospf 1
Router(config-router)#network 10.1.10.0 255.255.255.252 area 1
Router(config-router)#network 10.10.0.0 255.255.255.252 area 0
Router(config-router)#
```
# Настройка ospf маршрутизации - border2
- Настройка ospf
```
Router>enable 
Router#configure terminal 
Router(config)#router ospf 1
Router(config-router)#network 10.1.20.0 255.255.255.252 area 1
Router(config-router)#network 10.10.0.0 255.255.255.252 area 0
Router(config-router)#
```
#BGP 

- настройка bgp на роутере провайдера 
```
PROVIDER#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
PROVIDER(config)#router bgp 65100
PROVIDER(config-router)#network 172.1.0.0 mask 255.255.255.252
PROVIDER(config-router)#nei
PROVIDER(config-router)#neighbor 172.1.0.2 re
PROVIDER(config-router)#neighbor 172.1.0.2 remote-as 65200
PROVIDER(config-router)#network 172.2.0.0 mask 255.255.255.252
PROVIDER(config-router)#neighbor 172.2.0.2 remote-as 65200
PROVIDER(config-router)#
```
- настройка bgp на роутере border1

```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router bgp 65200
Router(config-router)#network 172.1.0.0 mask 255.255.255.252
Router(config-router)#neighbor 172.1.0.1 remote-as 65100
Router(config-router)#%BGP-5-ADJCHANGE: neighbor 172.1.0.1 Up
```
- настройка bgp на роутере border2

```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router bgp 65200
Router(config-router)#network 172.2.0.0 mask 255.255.255.252
Router(config-router)#neighbor 172.2.0.1 remote-as 65100
```


# Инспектирование трафика ASA1
- удаление дефолтной политики 
```
ciscoasa(config)#no service-policy global_policy global
ciscoasa(config)#no policy-map global_policy
ciscoasa(config)#no policy-map type inspect dns preset_dns_map
ciscoasa(config)#no class-map inspection_default
```

- настройка нспектирования трафика icmp

```
ciscoasa#configure terminal 
ciscoasa(config)#class-map inspection_default
ciscoasa(config-cmap)#match default-inspection-traffic 
ciscoasa(config-cmap)#ex
ciscoasa(config)#policy-map global_policy
ciscoasa(config-pmap)#class inspection_default
ciscoasa(config-pmap-c)#inspect icmp 
ciscoasa(config-pmap-c)#ex
ciscoasa(config)#service-policy global_policy global 
ciscoasa(config)#wr mem

```
- Доступ из зоны «Inside» во все зоны на Cisco ASA осущетсвлен по дефолту. 


access-list inside_access_in extended permit ip any any
Трафик от приватных адресов из Outside в Inside и DMZ на Cisco ASA
# asa1
```
access-list 100 extended permit icmp 192.168.21.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.41.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.51.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.101.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.31.0 255.255.255.0 any
access-list 100 extended permit udp 192.168.21.0 255.255.255.128 any
access-list 100 extended permit udp 192.168.41.0 255.255.255.128 any
access-list 100 extended permit udp 192.168.51.0 255.255.255.128 any
access-list 100 extended permit udp 192.168.31.0 255.255.255.0 any
access-list 100 extended permit udp 192.168.101.0 255.255.255.128 any
access-group 101 in interface OUTSIDE
access-group 101 in interface INSIDE
```
#asa2
```
access-list 100 extended permit icmp 192.168.21.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.41.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.51.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.101.0 255.255.255.128 any
access-list 100 extended permit icmp 192.168.31.0 255.255.255.0 any
access-list 100 extended permit udp 192.168.21.0 255.255.255.128 any
access-list 100 extended permit udp 192.168.41.0 255.255.255.128 any
access-list 100 extended permit udp 192.168.51.0 255.255.255.128 any
access-list 100 extended permit udp 192.168.31.0 255.255.255.0 any
access-list 100 extended permit udp 192.168.101.0 255.255.255.128 any
access-list 100 extended permit tcp 192.168.21.0 255.255.255.128 host 192.168.200.60 eq www
access-list 100 extended permit tcp 192.168.31.0 255.255.255.128 host 192.168.200.60 eq www
access-list 100 extended permit tcp any host 192.168.200.60 eq www
access-list 100 extended permit icmp any host 192.168.200.60
access-list 101 extended permit tcp 192.168.30.0 255.255.255.128 host 192.168.200.60 eq www
access-list 102 extended permit tcp host 192.168.200.60 192.168.30.0 255.255.255.0
access-list 102 extended permit tcp host 192.168.200.60 192.168.20.0 255.255.255.0
access-list 102 extended permit icmp host 192.168.200.60 any
access-group 100 in interface OUTSIDE
access-group 101 in interface INSIDE
access-group 102 in interface DMZ
```


# Малый офис

# назначение Vlan на свиче swich
```
Switch#configure terminal 
Switch(config)#vlan 100
Switch(config-vlan)#name mgmt
Switch(config-vlan)#ex
Switch(config)#vlan 201
Switch(config-vlan)#name user
Switch(config-vlan)#ex
Switch(config)#vlan 300
Switch(config-vlan)#name wlan
Switch(config-vlan)#ex
Switch(config)#vlan 401
Switch(config-vlan)#name voip
Switch(config-vlan)#ex
Switch(config)#vlan 501
Switch(config-vlan)#name printer
Switch(config-vlan)#ex

Switch(config)#
```

- Настройка порта рабочего места пользователя
```
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport mode access 
Switch(config-if)#switchport access vlan 201
Switch(config-if)#switchport voice vlan 401
Switch(config-if)#ex
Switch(config)#
```
- Настройка порта точки доступа wifi

```
Switch#configure terminal 
Switch(config)#interface range fastEthernet 0/1
Switch(config-if-range)#switchport mode trunk 
Switch(config-if-range)#switchport trunk allowed vlan 100,300
Switch(config-if-range)#switchport trunk native vlan 100
```

# натсройка роутера филиала
- Создание Vlan на роутере

- настройка саб интерфейсов для vlan 
```
Router#vlan database
Router(vlan)#vlan 100
VLAN 401 added:
    Name: VLAN0100
Router#vlan database
Router(vlan)#vlan 201
VLAN 401 added:
    Name: VLAN0201
	Router(vlan)#vlan 401
VLAN 401 added:
    Name: VLAN0401
	Router(vlan)#vlan 501
VLAN 401 added:
    Name: VLAN0501
```
-Cоздание vlan интерфейсов
```
Router(config)#interface vlan 101
Router(config-if)#
%LINK-5-CHANGED: Interface Vlan201, changed state to up
Router(config-if)#no shutdown 
Router(config-if)#ex

Router(config)#interface vlan 201
Router(config-if)#
%LINK-5-CHANGED: Interface Vlan201, changed state to up
Router(config-if)#no shutdown 
Router(config-if)#ex
Router(config)#interface vlan 401
Router(config-if)#
%LINK-5-CHANGED: Interface Vlan401, changed state to up
Router(config-if)#no shutdown 
Router(config-if)#ex
Router(config)#interface vlan 501
Router(config-if)#
%LINK-5-CHANGED: Interface Vlan501, changed state to up
Router(config-if)#no shutdown 
Router(config-if)#ex
```

```
Router#configure terminal 
Router(config)#interface gigabitEthernet 0/1.201
Router(config-subif)#encapsulation dot1Q 201
Router(config-subif)#ip address 192.168.21.1 255.255.255.128
Router(config-subif)#
Router#
outer#configure terminal 
Router(config)#interface gigabitEthernet 0/1.401
Router(config-subif)#encapsulation dot1Q 401
Router(config-subif)#ip address 192.168.41.1 255.255.255.128
Router(config-subif)#
Router#
outer#configure terminal 
Router(config)#interface gigabitEthernet 0/1.501
Router(config-subif)#encapsulation dot1Q 501
Router(config-subif)#ip address 192.168.51.1 255.255.255.128
Router(config-subif)#
Router#
```

# настройка GRE тоннеля бордер1 и OSPF

```
Router#configure terminal 
Router(config)#interface tunnel 0
Router(config-if)#ip address 192.16.0.1 255.255.255.0
Router(config-if)#tunnel source gigabitEthernet 0/1
Router(config-if)#tunnel destination 172.3.0.2
```
OSPF
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router ospf 1
Router(config-router)#network 192.16.0.0 255.255.255.0 area 2
Router(config-router)#
00:22:56: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.21.1 on Tunnel0 from LOADING to FULL, Loading Done
```
# настройка GRE тоннеля бордер2 и OSPF
```
Router#configure terminal 
Router(config)#interface tunnel 0
Router(config-if)#ip address 192.16.10.1 255.255.255.0
Router(config-if)#tunnel source gigabitEthernet 0/1
Router(config-if)#tunnel destination 172.3.0.2
```
OSPF
```
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#router ospf 1
Router(config-router)#network 192.16.10.0 255.255.255.0 area 2
Router(config-router)#
00:22:56: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.21.1 on Tunnel0 from LOADING to FULL, Loading Done
```


----------------------------------------

# настройка GRE тоннеля router филиала 
```
Router#configure terminal 
Router(config)#interface tunnel 0
Router(config-if)#ip address 192.16.0.3 255.255.255.0
Router(config-if)#tunnel source gigabitEthernet 0/0
Router(config-if)#tunnel destination 172.1.0.2

```
```
Router#configure terminal 
Router(config)#interface tunnel 1
Router(config-if)#ip address 192.16.10.3 255.255.255.0
Router(config-if)#tunnel source gigabitEthernet 0/0
Router(config-if)#tunnel destination 172.2.0.2

```
OSPF 
```
Router#configure terminal 
Router(config)#router ospf 1
Router(config-router)#network 192.16.0.0 255.255.255.0 area 2
Router(config-router)#network 192.16.10.0 255.255.255.0 area 2
Router(config-router)#network 192.168.21.0 0.0.0.127 area 2
Router(config-router)#network 192.168.41.0 0.0.0.127 area 2
Router(config-router)#network 192.168.51.0 0.0.0.127 area 2
Router(config-router)#
00:22:56: %OSPF-5-ADJCHG: Process 1, Nbr 192.16.0.1 on Tunnel0 from LOADING to FULL, Loading Done
```

# Настройка NAT 
бордер 0 
```
Router#configure terminal 
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip nat outside 
Router(config-if)#ex
Router(config)#
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip nat inside 
access-list 1 permit any
ip nat inside source list 1 interface GigabitEthernet0/1 overload
Router(config)#
```
бордер1 
```
Router#configure terminal 
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip nat outside 
Router(config-if)#ex
Router(config)#
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip nat inside 
access-list 1 permit any
ip nat inside source list 1 interface GigabitEthernet0/1 overload
Router(config)#
```
бордер - малый офис 
```
Router#configure terminal 
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip nat outside 
Router(config-if)#ex
Router(config)#
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip nat inside 
access-list 1 permit any
ip nat inside source list 1 interface GigabitEthernet0/1 overload
Router(config)#
`
