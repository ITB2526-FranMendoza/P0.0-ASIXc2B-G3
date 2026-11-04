# Cliente Linux (PC-LIN-G3)

**Responsable:** Fran Mendoza · **Proyecto:** P0.0-ASIXc2B-G3

| Equipo | Sistema | Hostname | IP | Máscara |
|---|---|---|---|---|
| Cliente Linux | Ubuntu 24.04 | `PC-LIN-G3` | 192.168.3.21 | /24 |

La VM se crea en **IsardVDI** con la tarjeta de red **ITB3** incluida en la configuración, para poder probar la conexión con los demás servidores.

## Índice

1. [Hostname](#1-hostname)
2. [Configuración de IP](#2-configuración-de-ip)
3. [Usuario bchecker](#3-usuario-bchecker)
4. [Pruebas de conectividad](#4-pruebas-de-conectividad)
5. [Validación por clave pública/privada con GitHub](#5-validación-por-clave-públicaprivada-con-github)
6. [Flujo Git](#6-flujo-git)

---

## 1. Hostname

Se edita el archivo `/etc/hostname`:

<img width="658" height="66" alt="image" src="https://github.com/user-attachments/assets/bea42dfd-e112-4a0f-8da3-4ea5702a8339" />


## 2. Configuración de IP

La interfaz `enp2s0` tiene la IP `192.168.3.21/24`. Se comprueba con:

```bash
ip a
```

<img width="782" height="119" alt="image" src="https://github.com/user-attachments/assets/726458bf-3d84-41e8-a03e-dcabdd52b816" />

## 3. Usuario bchecker

Usuario `bchecker` con contraseña `bchecker121`:

<img width="471" height="104" alt="image" src="https://github.com/user-attachments/assets/26218201-41c2-41df-b11c-e72ce6feaa67" />

## 4. Pruebas de conectividad

| Destino | IP | Responsable | Ping | Servicio |
|---|---|---|---|---|
| R-G3 (router/gateway) | 192.168.3.30 | Rubén | OK | DHCP/DNS OK |
| SSH-G3 | 192.168.3.40 | Joel | OK | SSH OK |
| F-G3 (FTP) | 192.168.3.41 | Joel | OK | FTP OK (`bchecker`) |
| W-G3 (web) | 192.168.3.12 | Jesús | OK | HTTP OK |
| B-G3 (BBDD) | 192.168.3.11 | Jesús | OK | MySQL |

### Router / DNS / DHCP

<img width="456" height="183" alt="image" src="https://github.com/user-attachments/assets/db3a50d3-d63b-4a55-a2b0-72fe3926e2ec" />

### SSH

<img width="457" height="179" alt="image" src="https://github.com/user-attachments/assets/b547df7c-e28b-4e41-9d68-2b37898cca26" />

La primera conexión pide aceptar la huella del servidor (`yes`).

<img width="553" height="325" alt="image" src="https://github.com/user-attachments/assets/20f619b7-9cf1-4e7f-ab2f-33969341fcaf" />

### FTP

<img width="554" height="181" alt="image" src="https://github.com/user-attachments/assets/38f35691-f045-4154-874c-ebb81006c509" />

El servidor responde `220 (vsFTPd 3.0.5)` y `230 Login successful`.

<img width="535" height="322" alt="image" src="https://github.com/user-attachments/assets/142d752c-d33a-4a7e-a1b2-4071a7a84898" />

### Web

<img width="549" height="158" alt="image" src="https://github.com/user-attachments/assets/cc896a03-d3aa-411a-9adf-261442b59e4d" />

### Base de datos

<img width="558" height="187" alt="image" src="https://github.com/user-attachments/assets/273b2f57-8e2d-4c42-a41c-64c80c6ecc12" />

## 5. Validación por clave pública/privada con GitHub

Se autentica el equipo contra GitHub con **SSH y un par de claves ed25519**, sin contraseña. La clave **privada** nunca sale del equipo; solo se sube la **pública**.

### Generar la clave

```bash
ssh-keygen -t ed25519 -C "PC-LIN-G3-bchecker"
```

Se crean:

- `~/.ssh/id_ed25519` → clave privada (**no compartir**)
- `~/.ssh/id_ed25519.pub` → clave pública

### Copiar la clave pública

```bash
cat ~/.ssh/id_ed25519.pub
```

### Añadirla en GitHub

*Perfil → Settings → SSH and GPG keys → New SSH key*. Alternativa: en el repo, *Settings → Deploy keys → Add deploy key*, marcando *Allow write access* si va a hacer push.

### Probar la validación

```bash
ssh -T git@github.com
```

La primera vez pide confirmar la huella de `github.com` (`yes`). La respuesta correcta es:

```
Hi <usuario>! You've successfully authenticated, but GitHub does not provide shell access.
```

## 6. Flujo Git

### Clonar por SSH

```bash
git clone git@github.com:ITB2526-FranMendoza/P0.0-ASIXc2B-G3.git
cd P0.0-ASIXc2B-G3
```

### Identificarse (solo la primera vez)

Si `git commit` da error de identidad:

```bash
git config --global user.name "ITB2526-FranMendoza"
git config --global user.email "<tu_correo>"
```

### Crear, confirmar y subir cambios

```bash
nano test.md
git add .
git commit -m "prueba de claves ssh"
git push
```

| Comando | Qué hace |
|---|---|
| `git clone` | Descarga una copia del repositorio |
| `git add .` | Prepara los cambios para el commit |
| `git commit -m "mensaje"` | Guarda los cambios con un mensaje |
| `git push` | Sube los commits a GitHub |
| `git pull` | Baja los cambios de GitHub |

El archivo `test.md` aparece en GitHub con el mensaje del commit.

### Comprobación nube → local

```bash
git pull
```

Responde `Ja està al dia.` (ya está actualizado).

### Subida a producción

> **Pendiente:** se documentará cuando W-G3 tenga su propia clave SSH. Flujo previsto: `git push` desde el cliente y `git pull` en **W-G3**.
