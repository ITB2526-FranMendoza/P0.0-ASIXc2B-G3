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

<img width="290" height="63" alt="image" src="https://github.com/user-attachments/assets/fc0bb254-174d-4f30-bcb1-a9f1ab966a47" />
<img width="348" height="484" alt="image" src="https://github.com/user-attachments/assets/d166868a-cd92-43b9-90d5-6df8fa24e4fa" />


## 2. Configuración de IP

*Propiedades de IPv4 → Usar la siguiente dirección IP*:

| Campo | Valor |
|---|---|
| Dirección IP | 192.168.3.22 |
| Máscara de subred | 255.255.255.0 |
| Puerta de enlace | 192.168.3.30 |
| DNS | 192.168.3.30 |

Se comprueba con:

```cmd
ipconfig /all
```

## 3. Usuario bchecker

Usuario `bchecker` con contraseña `bchecker121`, desde un CMD como administrador:

<img width="523" height="97" alt="image" src="https://github.com/user-attachments/assets/54373231-0e12-4e4c-a172-dec5bae93e26" />

## 4. Pruebas de conectividad

| Destino | IP | Responsable | Ping | Servicio |
|---|---|---|---|---|
| R-G3 (router/gateway) | 192.168.3.30 | Rubén | OK | n/a |
| SSH-G3 | 192.168.3.40 | Joel | OK | SSH OK |
| F-G3 (FTP) | 192.168.3.41 | Joel | OK | FTP OK (`bchecker`) |
| W-G3 (web) | 192.168.3.12 | Jesús | OK | Pendiente (HTTP) |
| B-G3 (BBDD) | 192.168.3.11 | Jesús | **Sin respuesta** | Pendiente (MySQL) |

### Router / DNS / DHCP

<img width="503" height="218" alt="image" src="https://github.com/user-attachments/assets/994cf3f4-4568-41d0-ab8f-76e5746063f7" />

### SSH

<img width="510" height="219" alt="image" src="https://github.com/user-attachments/assets/3e3a4d60-2b6e-4a1d-9954-b32e6efff05f" />

La primera conexión pide aceptar la huella del servidor (`yes`).

<img width="619" height="227" alt="image" src="https://github.com/user-attachments/assets/81f4d965-72a3-443d-98f3-ddde225ff2a1" />

### FTP

<img width="603" height="287" alt="image" src="https://github.com/user-attachments/assets/5dfcb4b1-e169-4897-84f7-2110ace2f592" />

Usuario `bchecker`. El servidor responde `220 (vsFTPd 3.0.5)` y `230 Login successful`.

<img width="560" height="259" alt="image" src="https://github.com/user-attachments/assets/69921fe8-a8fb-49de-af82-c20aae9ef690" />

### Web

<img width="598" height="260" alt="image" src="https://github.com/user-attachments/assets/c6ef23ac-dc5c-4879-8d90-7cf303b445ac" />

### Base de datos

```cmd
ping 192.168.3.11
```
