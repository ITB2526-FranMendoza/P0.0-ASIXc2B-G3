# Cliente Windows (PC-WIN-G3)

**Responsable:** Fran Mendoza · **Proyecto:** P0.0-ASIXc2B-G3

| Equipo | Sistema | Hostname | IP | Máscara |
|---|---|---|---|---|
| Cliente Windows | Windows 10 | `PC-WIN-G3` | 192.168.3.22 | 255.255.255.0 |

La VM se crea en **IsardVDI** con la tarjeta de red **ITB3** incluida en la configuración, para poder probar la conexión con los demás servidores.

## Índice

1. [Hostname](#1-hostname)
2. [Configuración de IP](#2-configuración-de-ip)
3. [Usuario bchecker](#3-usuario-bchecker)
4. [Pruebas de conectividad](#4-pruebas-de-conectividad)

---

## 1. Hostname

*Propiedades del sistema → Cambiar nombre del equipo* → `PC-WIN-G3`, grupo de trabajo `WORKGROUP`. Se comprueba con:

```cmd
hostname
```

## 2. Configuración de IP

*Propiedades de IPv4 → Usar la siguiente dirección IP*:

| Campo | Valor |
|---|---|
| Dirección IP | 192.168.3.22 |
| Máscara de subred | 255.255.255.0 |
| Puerta de enlace | *(pendiente: 192.168.3.30)* |
| DNS | *(pendiente)* |

Se comprueba con:

```cmd
ipconfig /all
```

## 3. Usuario bchecker

Usuario `bchecker` con contraseña `bchecker121`, desde un CMD como administrador:

```cmd
net user bchecker * /add
```

## 4. Pruebas de conectividad

| Destino | IP | Responsable | Ping | Servicio |
|---|---|---|---|---|
| R-G3 (router/gateway) | 192.168.3.30 | Rubén | OK | n/a |
| SSH-G3 | 192.168.3.40 | Joel | OK | SSH OK |
| F-G3 (FTP) | 192.168.3.41 | Joel | OK | FTP OK (`bchecker`) |
| W-G3 (web) | 192.168.3.12 | Jesús | OK | Pendiente (HTTP) |
| B-G3 (BBDD) | 192.168.3.11 | Jesús | **Sin respuesta** | Pendiente (MySQL) |

### Router / DNS / DHCP

```cmd
ping 192.168.3.30
```

### SSH

```cmd
ping 192.168.3.40
ssh 192.168.3.40
```

La primera conexión pide aceptar la huella del servidor (`yes`).

### FTP

```cmd
ping 192.168.3.41
ftp 192.168.3.41
```

Usuario `bchecker`. El servidor responde `220 (vsFTPd 3.0.5)` y `230 Login successful`.

### Web

```cmd
ping 192.168.3.12
```

### Base de datos

```cmd
ping 192.168.3.11
```

No hay respuesta desde Windows (en Linux sí). Pendiente de revisar: firewall de B-G3, firewall de Windows o regla del router.
