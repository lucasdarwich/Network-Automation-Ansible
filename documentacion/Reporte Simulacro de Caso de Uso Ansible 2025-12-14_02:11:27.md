# Reporte de automatizacion de configuraciones de red

## Servicio: Simulacro de Caso de Uso Ansible

Date: 2025-12-14 - Time: 02:11:27  
User: ldarwich

```
Network Automation Engineer - UTN-FRC
```

## Device - SwitchA

### Configuraciónes interfaces tipo Access

### hostname: SwitchA

```
!
interface GigabitEthernet1/1
 description Conexion a PC_ING_1
 switchport mode access
 switchport access vlan 10
!
interface GigabitEthernet1/2
 description Conexion a PC_PROD_1
 switchport mode access
 switchport access vlan 20
!
```

## Device - SwitchB

### Configuraciónes interfaces tipo Access

### hostname: SwitchB

```
!
interface GigabitEthernet1/1
 description Conexion a PC_ING_2
 switchport mode access
 switchport access vlan 10
!
interface GigabitEthernet1/2
 description Conexion a PC_PROD_2
 switchport mode access
 switchport access vlan 20
!
```

## Device - SwitchC

### Configuraciónes interfaces tipo Access

### hostname: SwitchC

```
!
interface GigabitEthernet1/1
 description Conexion a Servidor_WEB
 switchport mode access
 switchport access vlan 30
!
```

# Device - SwitchA

## Configuraciónes interfaces tipo Trunks

### hostname: SwitchA

```
!
interface GigabitEthernet0/1
 description Conexion a SW_CORE_1
 switchport trunk encapsulation dot1q
 switchport mode dynamic auto
 switchport trunk allowed vlan 10,20
!
interface GigabitEthernet0/2
 description Conexion a SW_CORE_2
 switchport trunk encapsulation dot1q
 switchport mode dynamic auto
 switchport trunk allowed vlan 10,20
!
```

# Device - SwitchB

## Configuraciónes interfaces tipo Trunks

### hostname: SwitchB

```
!
interface GigabitEthernet0/1
 description Conexion a SW_CORE_1
 switchport trunk encapsulation dot1q
 switchport mode dynamic auto
 switchport trunk allowed vlan 10,20
!
interface GigabitEthernet0/2
 description Conexion a SW_CORE_2
 switchport trunk encapsulation dot1q
 switchport mode dynamic auto
 switchport trunk allowed vlan 10,20
!
```

# Device - SwitchC

## Configuraciónes interfaces tipo Trunks

### hostname: SwitchC

```
!
interface GigabitEthernet0/1
 description Conexion a SwitchCore_1
 switchport trunk encapsulation dot1q
 switchport mode dynamic auto
 switchport trunk allowed vlan 30
!
interface GigabitEthernet0/2
 description Conexion a SwitchCore_2
 switchport trunk encapsulation dot1q
 switchport mode dynamic auto
 switchport trunk allowed vlan 30
!
```

# Device - SwitchCore_1

## Configuraciónes interfaces tipo Trunks

### hostname: SwitchCore_1

```
!
interface GigabitEthernet0/1
 description Conexion a SwitchA
 switchport trunk encapsulation dot1q
 switchport mode dynamic desirable
 switchport trunk allowed vlan 10,20,30
!
interface GigabitEthernet0/2
 description Conexion a SwitchB
 switchport trunk encapsulation dot1q
 switchport mode dynamic desirable
 switchport trunk allowed vlan 10,20,30
!
interface GigabitEthernet0/3
 description Conexion a SwitchC
 switchport trunk encapsulation dot1q
 switchport mode dynamic desirable
 switchport trunk allowed vlan 10,20,30
!
```

# Device - SwitchCore_2

## Configuraciónes interfaces tipo Trunks

### hostname: SwitchCore_2

```
!
interface GigabitEthernet0/1
 description Conexion a SwitchB
 switchport trunk encapsulation dot1q
 switchport mode dynamic desirable
 switchport trunk allowed vlan 10,20,30
!
interface GigabitEthernet0/2
 description Conexion a SwitchA
 switchport trunk encapsulation dot1q
 switchport mode dynamic desirable
 switchport trunk allowed vlan 10,20,30
!
interface GigabitEthernet0/3
 description Conexion a SwitchC
 switchport trunk encapsulation dot1q
 switchport mode dynamic desirable
 switchport trunk allowed vlan 10,20,30
!
```

# Device - SwitchA

## Configuraciónes de VLANs

### hostname: SwitchA

```
!
vlan 10
 name Ingenieria
!
vlan 20
 name Produccion
!
```

# Device - SwitchB

## Configuraciónes de VLANs

### hostname: SwitchB

```
!
vlan 10
 name Ingenieria
!
vlan 20
 name Produccion
!
```

# Device - SwitchC

## Configuraciónes de VLANs

### hostname: SwitchC

```
!
vlan 30
 name Finanzas
!
```

# Device - SwitchCore_1

## Configuraciónes de VLANs

### hostname: SwitchCore_1

```
!
vlan 10
 name Ingenieria
!
vlan 20
 name Produccion
!
vlan 30
 name Finanzas
!
```

# Device - SwitchCore_2

## Configuraciónes de VLANs

### hostname: SwitchCore_2

```
!
vlan 10
 name Ingenieria
!
vlan 20
 name Produccion
!
vlan 30
 name Finanzas
!
```

# Device - SwitchCore_1

## Configuraciónes interfaces tipo SVI & VRRP

### hostname: SwitchCore_1

```
interface Vlan10
  description SVI Ingenieria
  ip address 192.168.10.250 255.255.255.0
  vrrp 10 ip 192.168.10.254
  vrrp 10 priority 1
!
interface Vlan20
  description SVI Produccion
  ip address 192.168.20.250 255.255.255.0
  vrrp 20 ip 192.168.20.254
  vrrp 20 priority 2
!
interface Vlan30
  description SVI Finanzas
  ip address 192.168.30.250 255.255.255.0
  vrrp 30 ip 192.168.30.254
  vrrp 30 priority 1
!
```

# Device - SwitchCore_2

## Configuraciónes interfaces tipo SVI & VRRP

### hostname: SwitchCore_2

```
interface Vlan10
  description SVI Ingenieria
  ip address 192.168.10.251 255.255.255.0
  vrrp 10 ip 192.168.10.254
  vrrp 10 priority 2
!
interface Vlan20
  description SVI Produccion
  ip address 192.168.20.251 255.255.255.0
  vrrp 20 ip 192.168.20.254
  vrrp 20 priority 1
!
interface Vlan30
  description SVI Finanzas
  ip address 192.168.30.251 255.255.255.0
  vrrp 30 ip 192.168.30.254
  vrrp 30 priority 2
!
```

# Device - SwitchCore_1

## Configuraciónes EIGRP

### hostname: SwitchCore_1

```
!
router eigrp 10
  eigrp router-id 1.1.1.1
  network 192.168.10.0
  network 192.168.20.0
  network 192.168.30.0
!
```

# Device - SwitchCore_2

## Configuraciónes EIGRP

### hostname: SwitchCore_2

```
!
router eigrp 10
  eigrp router-id 1.1.1.3
  network 192.168.10.0
  network 192.168.20.0
  network 192.168.30.0
!
```
