# Simulacro de Caso de Uso – Ansible Network Automation

**Autor:** Lucas Javier Darwich  
**Versión:** 1.0  
**Fecha de creación:** 12 de diciembre de 2025  
**Proyecto:** Simulación de Caso de Uso Integral – Network Automation  
**Entorno:** Ansible + Jinja2 + Cisco IOS (Lab EVE-NG)

---

## 📌 Descripción General

Este proyecto implementa una solución integral de **automatización de infraestructura de red** utilizando **Ansible**, siguiendo el paradigma de **Infraestructura como Código (IaC)**.

Toda la infraestructura se define mediante un **modelo de datos centralizado en YAML**, que luego es procesado por **templates Jinja2** para:

- Generar configuraciones Cisco IOS (`.cfg`)
- Generar documentación técnica en Markdown (`.md`)
- Aplicar configuraciones a dispositivos reales o simulados
- Unificar documentación en un reporte final

El objetivo es garantizar **consistencia, trazabilidad, reproducibilidad y validación** de la configuración de red.

---

## 🏗️ Arquitectura de la Infraestructura

### Topología de Red Simulada

La infraestructura contempla los siguientes dispositivos:

- **2 Switches Core**

  - `SwitchCore_1` – IP Gestión: `192.168.100.10`
  - `SwitchCore_2` – IP Gestión: `192.168.100.11`
  - Routing dinámico **EIGRP (AS 10)**
  - Redundancia de gateway mediante **VRRP**

- **2 Switches de Acceso**
  - `SwitchA` – IP Gestión: `192.168.100.12`
  - `SwitchB` – IP Gestión: `192.168.100.13`
  - `SwitchC` – IP Gestión: `192.168.100.14`

---

### VLANs Configuradas

| VLAN | Nombre     | Red IP           |
| ---: | ---------- | ---------------- |
|   10 | Ingeniería | 192.168.10.0 /24 |
|   20 | Producción | 192.168.20.0 /24 |
|   30 | Finanzas   | 192.168.30.0 /24 |

---

### Características de Alta Disponibilidad

- **VRRP (Virtual Router Redundancy Protocol)**  
  Gateways redundantes en los switches core.

- **EIGRP (Enhanced Interior Gateway Routing Protocol)**  
  Routing dinámico entre VLANs (AS 10).

- **Enlaces Troncales (Trunk)**  
  Transporte de múltiples VLANs entre access, core y data center.

---

## 📁 Estructura del Proyecto

```tree
laboratorio2/
├── configs/                    # Configuraciones generadas automáticamente
├── documentacion/              # Reporte final generado
│   └── modulos/                # Módulos Markdown por componente
├── inventario/
│   └── inventario.ini          # Inventario Ansible
├── jsons/
│   └── json-schema-modelo.json # JSON Schema para validar el modelo
├── modelo_datos/
│   └── modelo.yaml             # Modelo de datos centralizado
├── playbooks/
│   ├── play_create_codigo.yaml
│   ├── play_config_devices.yaml
│   └── play_create_documentacion.yaml
├── tasks/
│   ├── task_check_empty_dir.yaml
│   ├── task_timestamp.yaml
│   └── task_validator.yaml
├── templates/
│   ├── eigrp_cfg.j2
│   ├── inter_access_cfg.j2
│   ├── inter_trunk_cfg.j2
│   ├── inter_svi_cfg.j2
│   ├── vlans_cfg.j2
│   └── *_doc.j2
├── ansible.cfg
├── pyproject.toml
├── uv.lock
└── README.md
```

🧠 Componentes Principales

### 1. Modelo de Datos (modelo_datos/modelo.yaml)

- Archivo YAML que define toda la infraestructura:
- Metadatos del proyecto
- Grupos de hosts
- Dispositivos de red
- Interfaces (access / trunk / SVI)
- VLANs
- VRRP
- EIGRP

👉 El modelo es declarativo, desacoplado de la lógica de ejecución.

### 2. Templates Jinja2 (templates/)

Permiten generar:

- Configuración Cisco IOS (.cfg)
- Documentación técnica (.md)

Templates principales:

- inter_access_cfg.j2 – Puertos de acceso
- inter_trunk_cfg.j2 – Puertos troncales
- vlans_cfg.j2 – Creación de VLANs
- inter_svi_cfg.j2 – SVIs + VRRP
- eigrp_cfg.j2 – Routing EIGRP

### 3. Playbooks (playbooks/)

play_create_codigo.yaml

- Renderiza templates Jinja2
- Genera archivos .cfg en configs/
- Genera módulos .md en documentacion/modulos/

play_config_devices.yaml

- Aplica configuraciones a dispositivos Cisco IOS
- Usa network_cli
- Ejecuta cambios de forma idempotente

play_create_documentacion.yaml

- Unifica los módulos Markdown
- Genera un reporte final con timestamp

### 4. Inventario (inventario/inventario.ini)

- Define hosts, grupos y subgrupos
- Variables de conexión y ansible_network_os
- Direcciones IP de gestión

### 5. Tasks Reutilizables (tasks/)

- task_timestamp.yaml – Fecha y hora del sistema
- task_check_empty_dir.yaml – Validación de archivos
- task_validator.yaml – Validación del modelo con JSON Schema

### 6. Validación con JSON Schema (jsons/)

- Valida estructura, tipos y campos obligatorios del modelo
- Detecta inconsistencias antes de generar o aplicar configuraciones
- El esquema puede ajustarse según evolución del modelo

## Uso del Proyecto

### Requisitos Previos

```bash
# Python >= 3.12
# Ansible >= 13.0.0
# ansible-pylibssh >= 1.3.0
```

### Instalación

```bash
# Instalar dependencias con uv
uv sync
```

### Flujo de Trabajo Típico

#### 1. Generar Configuraciones

```bash
cd playbooks
ansible-playbook -i ../inventario/inventario.ini play_create_codigo.yaml
```

Esto generará:

- Archivos `.cfg` en `configs/`
- Archivos `.md` en `documentacion/modulos/`

#### 2. Validar Configuraciones

Revisar los archivos generados en `configs/` antes de aplicar.

#### 3. Aplicar Configuraciones a Dispositivos

```bash
ansible-playbook -i ../inventario/inventario.ini play_config_devices.yaml
```

**Nota**: Requiere conectividad con dispositivos reales o simulador.

#### 4. Generar Documentación

```bash
ansible-playbook -i ../inventario/inventario.ini play_create_documentacion.yaml
```

Crea un reporte consolidado en `documentacion/` con timestamp.

---

## Configuración

### Modificar la Infraestructura

Editar `modelo_datos/modelo.yaml`:

```yaml
devices:
  SW-Nuevo:
    management:
      ip: "192.168.100.xx"
      interface: "GigabitEthernet0/0"
    interfaces:
      - name: "GigabitEthernet0/1"
        description: "Conexion a SW-CORE_1"
        mode: trunk
        trunk_mode: auto
        allowed_vlans: 10,20
    vlans:
      - id: 10
        name: "Ingenieria"
```

### Actualizar Inventario

Editar `inventario/inventario.ini`:

```ini
[cisco_ios_nuevo_grupo]
SW-Nuevo ansible_host=10.2.0.10X
```

### Crear Nuevos Templates

1. Crear archivo `.j2` en `templates/`
2. Usar sintaxis Jinja2 con acceso a variables del modelo
3. Referenciar en playbooks con bucle para generar cfg y doc

---

## Referencias

- [Documentación Ansible](https://docs.ansible.com/)
- [Cisco IOS Collection](https://galaxy.ansible.com/cisco/ios)
- [Jinja2 Templates](https://jinja.palletsprojects.com/)
- [VRRP Protocol RFC 5798](https://tools.ietf.org/html/rfc5798)
- [EIGRP Protocol](https://www.cisco.com/c/en/us/support/docs/ip/enhanced-interior-gateway-routing-protocol-eigrp/16406-eigrp-toc.html)

---

## Licencia

Proyecto educativo - Network Automation Engineer Course

---

**Última actualización**: Diciembre 2025
